---
type: skill
status: active
date: 2026-04-24
tags: [skill, linting, health-check, vault, automation]
---

# Skill: Vault Lint

Run this skill when the user says "lint the vault", "vault health check", "find gaps", "what's broken", or any equivalent.

Can also be run automatically via `Second Brain/scripts/vault-lint.py`.

---

## What This Skill Does

Audits the entire vault for health issues and produces a prioritised report with actionable fixes.

---

## Checks to Run

### 1. Orphan Notes
Find notes with zero incoming wikilinks — no other note links to them.

```
For each .md file: count how many other files contain [[filename]] or [[path/filename]]
Flag any file with count = 0 (except index files and maps)
```

### 2. Missing Frontmatter Fields
Every note should have: `type`, `status`, `date`, `tags`.
Flag notes missing any of these.

### 3. Stale Notes
Notes with `status: draft` older than 14 days — likely abandoned.
Notes with `status: active` that haven't been modified in 90+ days.

### 4. Broken Wikilinks
Find `[[wikilinks]]` that point to files that do not exist.
These are either: typos, renamed files, or notes that should be created.

### 5. Portuguese Content
Find file names, headings (# lines), or frontmatter values that contain Portuguese words.
All content must be in English.

### 6. Missing Project Links
Find notes with `project:` in frontmatter but no `[[wikilink]]` back to the project README.

### 7. Connection Opportunities
Find notes that mention the same concepts, people, or projects but don't link to each other.
Suggest specific wikilinks to add.

### 8. Knowledge Gaps
Find `[[wikilinks]]` to non-existent notes that appear in 3+ places — these are notes that should be created.

---

## Output Format

Produce a lint report as a vault note at `03 Intelligence/vault-lint-YYYY-MM-DD.md`:

```markdown
---
type: report
status: active
date: YYYY-MM-DD
tags: [vault, lint, health-check]
---

## Vault Lint Report — YYYY-MM-DD

### Summary
- X orphan notes
- X broken wikilinks  
- X notes missing frontmatter
- X stale drafts
- X connection opportunities
- X knowledge gaps (missing notes to create)

### Orphan Notes (fix: link from somewhere)
- [[note-name]] — suggested parent: [[suggested-note]]

### Broken Wikilinks (fix: rename or create target)
- In [[source-note]]: [[broken-link]] → does not exist

### Knowledge Gaps (fix: create these notes)
- [[concept-name]] — referenced in X notes, doesn't exist

### Connection Opportunities (fix: add wikilinks)
- [[note-a]] and [[note-b]] both discuss [topic] — link them

### Stale Drafts (fix: complete or archive)
- [[draft-note]] — status: draft since YYYY-MM-DD

### Portuguese Content (fix: translate)
- File: portuguese-filename.md → rename to english-name.md
```

---

## After Reporting

Fix issues in priority order:
1. Broken wikilinks (highest impact — broken graph)
2. Knowledge gaps (create missing notes)
3. Orphan notes (add incoming links)
4. Stale drafts (complete or move to `05 Archives/`)
5. Missing frontmatter (add fields)
6. Portuguese content (rename/translate)
