---
type: skill
status: active
date: 2026-04-24
tags: [skill, indexing, knowledge, vault, automation]
---

# Skill: Vault Index

Run this skill when the user says "rebuild the index", "update the knowledge index", "reindex the vault", or any equivalent.

---

## What This Skill Does

Rebuilds `knowledge/index.md` — the master navigation map of the vault. Scans all active notes, extracts key concepts and summaries, and produces a fresh index that JARVIS loads at session start.

---

## Step 1: Scan All Active Notes

Read every `.md` file in the vault that has `status: active` in frontmatter. Skip drafts, archives, and templates.

Collect for each note:
- Title (from filename or first heading)
- Type and tags
- One-line summary (from first paragraph or description)
- Project association
- Incoming and outgoing wikilinks

---

## Step 2: Group by Folder

Organise the index by vault section:

| Section | Covers |
|---|---|
| Daily | Recent daily notes (last 7 days) |
| Context | Identity, brand, strategy, people |
| Intelligence | Decisions, research, meetings, market data |
| Resources | Tools, references, external material |
| Projects | All active projects |
| Second Brain | Skills, maps, operating rules |

---

## Step 3: Extract Key Concepts

Find the most-linked notes across the vault — these are the conceptual hubs. List the top 20 by incoming link count.

---

## Step 4: Write the Index

Write to `knowledge/index.md`:

```markdown
---
type: index
status: active
date: YYYY-MM-DD
tags: [index, navigation, knowledge]
---

# Knowledge Index
> Auto-generated YYYY-MM-DD. X active notes across X folders.

## Recent (Last 7 Days)
- [[01 Daily/YYYY-MM-DD]] — summary

## Most Connected Notes
1. [[note]] — X incoming links — one-line summary
2. ...

## By Section

### Context
- [[02 Context/me]] — identity, preferences, working style
...

### Active Projects
- [[Projects/App Project/App | SportFlow/README]] — summary
...

### Intelligence
- [[03 Intelligence/...]] — summary
...

### Second Brain Skills
- [[Second Brain/skills/...]] — what it does
...

## Recent Additions (Last 14 Days)
- [[note]] — date added
```

---

## Step 5: Update Second Brain Index

After rebuilding `knowledge/index.md`, check if any new skills were added to `Second Brain/skills/` that aren't listed in `Second Brain/index.md`. Add them to the correct section.

---

## When to Run

- Start of a new week
- After a major vault cleanup
- After importing large batches of content (like Notion exports)
- After running `vault-lint` and fixing issues
