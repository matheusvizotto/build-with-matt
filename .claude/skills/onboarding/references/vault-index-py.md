#!/usr/bin/env python3
"""
Vault Indexer — AI OS
Rebuilds knowledge/index.md — the master navigation map of the vault.

Usage:
    python3 vault-index.py
    python3 vault-index.py --report-only  # print index, don't write file
    python3 vault-index.py --vault /path  # override vault path
"""

import os
import re
import sys
import argparse
from pathlib import Path
from datetime import datetime, timedelta
from collections import defaultdict

VAULT = Path(__file__).parent.parent.parent
IGNORE_DIRS = {".obsidian", ".claude", "05 Archives", "Second Brain/scripts"}

SECTION_MAP = {
    "01 Daily": "Daily",
    "02 Context": "Context",
    "03 Intelligence": "Intelligence",
    "04 Resources": "Resources",
    "Projects": "Projects",
    "Second Brain": "Second Brain",
    "knowledge": "Knowledge",
    "AI-OS-Blueprint": "Blueprint",
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


def get_first_paragraph(content: str) -> str:
    in_frontmatter = False
    lines = content.splitlines()
    i = 0
    if lines and lines[0].strip() == "---":
        in_frontmatter = True
        i = 1
        while i < len(lines):
            if lines[i].strip() == "---":
                in_frontmatter = False
                i += 1
                break
            i += 1
    for j in range(i, len(lines)):
        line = lines[j].strip()
        if line and not line.startswith("#") and not line.startswith(">") and not line.startswith("|") and not line.startswith("!"):
            return line[:120]
    return ""


def note_key(path: Path, vault: Path) -> str:
    rel = path.relative_to(vault)
    return str(rel).replace("\\", "/").removesuffix(".md")


def get_section(key: str) -> str:
    for prefix, section in SECTION_MAP.items():
        if key.startswith(prefix + "/") or key == prefix:
            return section
    return "Other"


def run_index(vault: Path) -> dict:
    notes = get_all_notes(vault)
    now = datetime.now()
    cutoff_7 = now - timedelta(days=7)
    cutoff_14 = now - timedelta(days=14)

    stem_to_key: dict[str, list[str]] = defaultdict(list)
    for note in notes:
        key = note_key(note, vault)
        stem_to_key[note.stem].append(key)
        stem_to_key[key].append(key)

    incoming: dict[str, int] = defaultdict(int)
    note_data = {}

    for note in notes:
        try:
            content = note.read_text(encoding="utf-8")
        except Exception:
            continue
        fm = parse_frontmatter(content)
        links = get_wikilinks(content)
        mtime = datetime.fromtimestamp(note.stat().st_mtime)
        note_data[note] = {
            "content": content,
            "fm": fm,
            "links": links,
            "mtime": mtime,
            "key": note_key(note, vault),
        }
        for link in links:
            target_stem = link.split("/")[-1]
            for key in (stem_to_key.get(target_stem, []) + stem_to_key.get(link, [])):
                incoming[key] += 1

    recent_7: list[tuple[str, datetime, str]] = []
    recent_14: list[tuple[str, datetime, str]] = []
    by_section: dict[str, list[tuple[str, str]]] = defaultdict(list)
    active_count = 0

    for note, data in note_data.items():
        key = data["key"]
        fm = data["fm"]
        status = fm.get("status", "").lower()
        if status not in ("active", ""):
            continue
        active_count += 1
        summary = get_first_paragraph(data["content"])
        section = get_section(key)
        by_section[section].append((key, summary))

        if data["mtime"] >= cutoff_7:
            recent_7.append((key, data["mtime"], summary))
        elif data["mtime"] >= cutoff_14:
            recent_14.append((key, data["mtime"], summary))

    recent_7.sort(key=lambda x: x[1], reverse=True)
    recent_14.sort(key=lambda x: x[1], reverse=True)

    top_connected = sorted(
        [(key, count) for key, count in incoming.items() if key in {data["key"] for data in note_data.values()}],
        key=lambda x: x[1],
        reverse=True,
    )[:20]

    new_skills: list[str] = []
    aios_index_path = vault / "Second Brain" / "index.md"
    if aios_index_path.exists():
        aios_content = aios_index_path.read_text(encoding="utf-8")
        for note in notes:
            if "Second Brain/skills" in str(note):
                skill_key = note_key(note, vault)
                if skill_key not in aios_content:
                    new_skills.append(skill_key)

    return {
        "active_count": active_count,
        "total_count": len(note_data),
        "recent_7": recent_7,
        "recent_14": recent_14,
        "by_section": dict(by_section),
        "top_connected": top_connected,
        "new_skills": new_skills,
        "incoming": incoming,
    }


def format_index(data: dict) -> str:
    today = datetime.now().strftime("%Y-%m-%d")
    lines = [
        "---",
        "type: index",
        "status: active",
        f"date: {today}",
        "tags: [index, navigation, knowledge]",
        "---",
        "",
        "# Knowledge Index — AI OS",
        f"> Auto-generated {today}. {data['active_count']} active notes across the vault.",
        "",
        "---",
        "",
    ]

    if data["recent_7"]:
        lines += ["## Recent (Last 7 Days)", ""]
        for key, mtime, summary in data["recent_7"][:15]:
            date_str = mtime.strftime("%Y-%m-%d")
            desc = f" — {summary}" if summary else ""
            lines.append(f"- `{date_str}` [[{key}]]{desc}")
        lines.append("")

    if data["top_connected"]:
        lines += ["## Most Connected Notes", ""]
        for i, (key, count) in enumerate(data["top_connected"], 1):
            lines.append(f"{i}. [[{key}]] — {count} incoming links")
        lines.append("")

    section_order = ["Context", "Daily", "Intelligence", "Projects", "Resources", "Second Brain", "Knowledge", "Blueprint", "Other"]
    for section in section_order:
        items = data["by_section"].get(section, [])
        if not items:
            continue
        lines += [f"## {section}", ""]
        items.sort(key=lambda x: x[0])
        for key, summary in items[:30]:
            desc = f" — {summary}" if summary else ""
            lines.append(f"- [[{key}]]{desc}")
        if len(items) > 30:
            lines.append(f"- ... and {len(items) - 30} more")
        lines.append("")

    if data["recent_14"]:
        lines += ["## Recent Additions (Last 14 Days)", ""]
        for key, mtime, _ in data["recent_14"][:20]:
            lines.append(f"- [[{key}]] — added {mtime.strftime('%Y-%m-%d')}")
        lines.append("")

    return "\n".join(lines)


def main():
    parser = argparse.ArgumentParser(description="Vault Indexer — AI OS")
    parser.add_argument("--report-only", action="store_true", help="Print index, don't write file")
    parser.add_argument("--vault", default=str(VAULT), help="Vault path override")
    args = parser.parse_args()

    vault = Path(args.vault)
    print(f"Indexing vault: {vault}")

    data = run_index(vault)
    index_content = format_index(data)

    if args.report_only:
        print(index_content)
    else:
        index_path = vault / "knowledge" / "index.md"
        index_path.write_text(index_content, encoding="utf-8")
        print(f"Index written to: {index_path}")

    if data["new_skills"]:
        print(f"\nNew skills not in Second Brain/index.md:")
        for skill in data["new_skills"]:
            print(f"  - {skill}")

    print(f"\n{data['active_count']} active notes indexed ({data['total_count']} total).")
    return 0


if __name__ == "__main__":
    sys.exit(main())
