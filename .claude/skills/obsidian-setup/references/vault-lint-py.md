#!/usr/bin/env python3
"""
Vault Linter — AI OS
Scans the vault for health issues and writes a lint report.

Usage:
    python3 vault-lint.py
    python3 vault-lint.py --fix          # auto-fix what's safe to fix
    python3 vault-lint.py --report-only  # print report, don't write file
"""

import os
import re
import sys
import argparse
from pathlib import Path
from datetime import datetime, timedelta
from collections import defaultdict

VAULT = Path(__file__).parent.parent.parent
REQUIRED_FRONTMATTER = {"type", "status", "date", "tags"}
STALE_DRAFT_DAYS = 14
STALE_ACTIVE_DAYS = 90
IGNORE_DIRS = {".obsidian", ".claude", "05 Archives", "Second Brain/scripts"}
IGNORE_ORPHAN_FILES = {"index.md", "README.md", "Vault-Map.md", "JARVIS.md", "CLAUDE.md"}
KNOWLEDGE_GAP_MIN_REFS = 3

PORTUGUESE_WORDS = {
    "projeto", "projetos", "diário", "reunião", "reuniões",
    "decisão", "decisões", "estratégia", "contexto", "recursos",
    "arquivo", "arquivos", "inteligência", "conhecimento", "tarefa", "tarefas",
    "não", "crescer", "manutenção", "implementação",
}


def get_all_notes(vault: Path) -> list[Path]:
    notes = []
    for path in vault.rglob("*.md"):
        skip = False
        for ignore in IGNORE_DIRS:
            if ignore in str(path):
                skip = True
                break
        if not skip:
            notes.append(path)
    return notes


def parse_frontmatter(content: str) -> dict:
    if not content.startswith("---"):
        return {}
    end = content.find("---", 3)
    if end == -1:
        return {}
    fm_block = content[3:end].strip()
    result = {}
    for line in fm_block.splitlines():
        if ":" in line:
            k, _, v = line.partition(":")
            result[k.strip()] = v.strip()
    return result


def get_wikilinks(content: str) -> set[str]:
    return set(re.findall(r'\[\[([^\]|#]+)(?:[|\]#][^\]]*)?]]', content))


def get_date_from_frontmatter(fm: dict) -> datetime | None:
    raw = fm.get("date", "")
    for fmt in ("%Y-%m-%d", "%Y/%m/%d"):
        try:
            return datetime.strptime(raw[:10], fmt)
        except (ValueError, TypeError):
            pass
    return None


def note_key(path: Path, vault: Path) -> str:
    rel = path.relative_to(vault)
    return str(rel).replace("\\", "/").removesuffix(".md")


def run_lint(vault: Path) -> dict:
    notes = get_all_notes(vault)
    now = datetime.now()

    # Build lookup: stem → note key (for wikilink resolution)
    stem_to_key: dict[str, list[str]] = defaultdict(list)
    for note in notes:
        key = note_key(note, vault)
        stem_to_key[note.stem].append(key)
        stem_to_key[key].append(key)

    # Build incoming link map
    incoming: dict[str, int] = defaultdict(int)
    broken_links: list[tuple[str, str]] = []
    wikilink_refs: dict[str, int] = defaultdict(int)

    note_data = {}
    for note in notes:
        try:
            content = note.read_text(encoding="utf-8")
        except Exception:
            continue
        fm = parse_frontmatter(content)
        links = get_wikilinks(content)
        note_data[note] = {"content": content, "fm": fm, "links": links}

        for link in links:
            target_stem = link.split("/")[-1]
            if target_stem in stem_to_key or link in stem_to_key:
                # valid link — count incoming
                for key in (stem_to_key.get(target_stem, []) + stem_to_key.get(link, [])):
                    incoming[key] += 1
            else:
                broken_links.append((note_key(note, vault), link))
                wikilink_refs[link] += 1

    # Track all referenced-but-missing links
    for link, count in wikilink_refs.items():
        pass  # already tracked in broken_links

    issues = {
        "orphans": [],
        "missing_frontmatter": [],
        "stale_drafts": [],
        "stale_active": [],
        "broken_links": broken_links,
        "portuguese_content": [],
        "knowledge_gaps": [],
    }

    missing_ref_counts: dict[str, int] = defaultdict(int)
    for source, link in broken_links:
        missing_ref_counts[link] += 1

    issues["knowledge_gaps"] = [
        link for link, count in missing_ref_counts.items()
        if count >= KNOWLEDGE_GAP_MIN_REFS
    ]

    for note, data in note_data.items():
        key = note_key(note, vault)
        fm = data["fm"]
        content = data["content"]

        # Orphans
        if note.name not in IGNORE_ORPHAN_FILES and incoming.get(key, 0) == 0:
            issues["orphans"].append(key)

        # Missing frontmatter
        missing_fields = REQUIRED_FRONTMATTER - set(fm.keys())
        if missing_fields:
            issues["missing_frontmatter"].append((key, sorted(missing_fields)))

        # Stale drafts
        note_date = get_date_from_frontmatter(fm)
        if fm.get("status", "").lower() == "draft":
            if note_date and (now - note_date).days > STALE_DRAFT_DAYS:
                issues["stale_drafts"].append((key, fm.get("date", "unknown")))

        # Stale active
        mtime = datetime.fromtimestamp(note.stat().st_mtime)
        if fm.get("status", "").lower() == "active":
            if (now - mtime).days > STALE_ACTIVE_DAYS:
                issues["stale_active"].append((key, mtime.strftime("%Y-%m-%d")))

        # Portuguese content
        filename_words = set(re.split(r'[-_/. ]', key.lower()))
        heading_words = set(
            word.lower()
            for line in content.splitlines()
            if line.startswith("#")
            for word in re.split(r'\W+', line)
        )
        found_pt = (filename_words | heading_words) & PORTUGUESE_WORDS
        if found_pt:
            issues["portuguese_content"].append((key, sorted(found_pt)))

    return issues


def format_report(issues: dict, vault: Path) -> str:
    today = datetime.now().strftime("%Y-%m-%d")
    total = sum(
        len(v) for v in issues.values()
        if isinstance(v, list)
    )

    lines = [
        "---",
        "type: report",
        "status: active",
        f"date: {today}",
        "tags: [vault, lint, health-check]",
        "---",
        "",
        f"## Vault Lint Report — {today}",
        "",
        "### Summary",
        f"- {len(issues['orphans'])} orphan notes",
        f"- {len(issues['broken_links'])} broken wikilinks",
        f"- {len(issues['missing_frontmatter'])} notes missing frontmatter fields",
        f"- {len(issues['stale_drafts'])} stale drafts (>{STALE_DRAFT_DAYS}d)",
        f"- {len(issues['stale_active'])} stale active notes (>{STALE_ACTIVE_DAYS}d without edit)",
        f"- {len(issues['knowledge_gaps'])} knowledge gaps (missing notes referenced {KNOWLEDGE_GAP_MIN_REFS}+ times)",
        f"- {len(issues['portuguese_content'])} notes with Portuguese content",
        f"- **{total} total issues**",
        "",
    ]

    if issues["broken_links"]:
        lines += ["### Broken Wikilinks (fix: rename or create target)", ""]
        for source, link in issues["broken_links"][:30]:
            lines.append(f"- In `[[{source}]]`: `[[{link}]]` → does not exist")
        if len(issues["broken_links"]) > 30:
            lines.append(f"- ... and {len(issues['broken_links']) - 30} more")
        lines.append("")

    if issues["knowledge_gaps"]:
        lines += ["### Knowledge Gaps (fix: create these notes)", ""]
        for link in issues["knowledge_gaps"]:
            count = sum(1 for _, l in issues["broken_links"] if l == link)
            lines.append(f"- `[[{link}]]` — referenced {count} times, note does not exist")
        lines.append("")

    if issues["orphans"]:
        lines += ["### Orphan Notes (fix: link from somewhere)", ""]
        for key in issues["orphans"][:20]:
            lines.append(f"- `[[{key}]]`")
        if len(issues["orphans"]) > 20:
            lines.append(f"- ... and {len(issues['orphans']) - 20} more")
        lines.append("")

    if issues["stale_drafts"]:
        lines += [f"### Stale Drafts (fix: complete or archive — older than {STALE_DRAFT_DAYS} days)", ""]
        for key, date in issues["stale_drafts"]:
            lines.append(f"- `[[{key}]]` — draft since {date}")
        lines.append("")

    if issues["missing_frontmatter"]:
        lines += ["### Missing Frontmatter (fix: add required fields)", ""]
        for key, fields in issues["missing_frontmatter"][:20]:
            lines.append(f"- `[[{key}]]` — missing: {', '.join(fields)}")
        lines.append("")

    if issues["portuguese_content"]:
        lines += ["### Portuguese Content (fix: rename/translate)", ""]
        for key, words in issues["portuguese_content"]:
            lines.append(f"- `[[{key}]]` — words found: {', '.join(words)}")
        lines.append("")

    if issues["stale_active"]:
        lines += [f"### Stale Active Notes (not edited in {STALE_ACTIVE_DAYS}+ days)", ""]
        for key, date in issues["stale_active"][:15]:
            lines.append(f"- `[[{key}]]` — last edited {date}")
        lines.append("")

    return "\n".join(lines)


def main():
    parser = argparse.ArgumentParser(description="Vault Linter — AI OS")
    parser.add_argument("--report-only", action="store_true", help="Print report, don't write file")
    parser.add_argument("--vault", default=str(VAULT), help="Vault path override")
    args = parser.parse_args()

    vault = Path(args.vault)
    print(f"Scanning vault: {vault}")

    issues = run_lint(vault)
    report = format_report(issues, vault)

    if args.report_only:
        print(report)
    else:
        today = datetime.now().strftime("%Y-%m-%d")
        report_path = vault / "03 Intelligence" / f"vault-lint-{today}.md"
        report_path.write_text(report, encoding="utf-8")
        print(f"Report written to: {report_path}")

    total = sum(len(v) for v in issues.values() if isinstance(v, list))
    print(f"\n{total} issues found across {sum(len(v) for v in issues.values() if isinstance(v, list))} checks.")
    return 0 if total == 0 else 1


if __name__ == "__main__":
    sys.exit(main())
