---
type: skill
status: active
date: 2026-04-24
tags: [skill, inbox, compile, vault, automation]
---

# Skill: Process Inbox

Run this skill when the user says "process inbox", "clear inbox", "compile inbox", or any equivalent.

---

## What This Skill Does

Takes every item in `00 Inbox/` and converts it into a properly formatted vault note — with frontmatter, wikilinks, callouts, and correct routing to the right folder.

---

## Step 1: Read All Inbox Items

Read every file in `00 Inbox/` completely. Do not skip or skim any file.

---

## Step 2: For Each Item, Determine Routing

Use `Second Brain/knowledge-routing.md` to decide where it belongs. Common routing decisions:

| Content type | Route to |
|---|---|
| Project idea or feature | `Projects/` — correct project subfolder |
| Daily log or reflection | `01 Daily/YYYY-MM-DD.md` — append to today |
| Research finding, market data | `03 Intelligence/` |
| Reference material, tool, resource | `04 Resources/` |
| Person or contact note | `02 Context/People/` |
| Decision made | `03 Intelligence/decisions/` |
| Meeting notes | `03 Intelligence/meetings/` |

If unsure, route to `03 Intelligence/` and add `status: needs-routing`.

---

## Step 3: Compile Each Note

Transform the raw inbox item into a proper vault note:

1. **Add YAML frontmatter** — `type`, `status`, `date`, `tags`, `project` (if applicable)
2. **Add a clear title section** — no duplicate of filename, no Portuguese
3. **Add wikilinks** — every mention of a project, person, tool, or concept gets `[[wikilink]]`
4. **Add callouts** where appropriate — `> [!important]` for decisions, `> [!todo]` for actions, `> [!tip]` for wins
5. **Link from at least one existing note** — never create an orphan

---

## Step 4: Write the Compiled Note

Write the compiled note to its destination folder. Use the English filename format: `kebab-case.md`.

---

## Step 5: Delete or Archive the Inbox Item

After successful compilation and routing, either:
- Delete the original inbox item, OR
- Move it to `05 Archives/inbox-processed/YYYY-MM/`

---

## Step 6: Report

After processing all items, report:
- How many items processed
- Where each was routed
- Any items that could not be routed (explain why)
