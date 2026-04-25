---
type: skill
skill: daily-note
status: active
date: 2026-04-24
tags: [skill, aios, daily, session, portable]
---

## Skill: Daily Note

When asked to log a session or update the daily note, follow this process.

---

### File Location

`01 Daily/YYYY-MM-DD.md`

Use today's date. If the file already exists, append a new session section. Do not overwrite existing sessions.

---

### Schema

Every session entry uses this fixed schema:

```
## Session Log: {Short Description of What Was Done}

**Date:** YYYY-MM-DD (append session number if multiple: "second session", "third session")
**Topics:** comma-separated list of main topics

**Decisions made:** (architectural, strategic, or product decisions — minimum one line, or "none")
**Lessons learned:** (what worked, what failed, what was discovered — minimum one line)
**Action items:** (concrete next steps, specific and actionable — use checkboxes)
```

Then add callout blocks for detail:

- `> [!success]` — what was built or completed
- `> [!important]` — key data, findings, or facts
- `> [!todo]` — open tasks and next steps
- `> [!warning]` — blockers, risks, or unresolved issues

---

### YAML Frontmatter

New daily note file:

```yaml
---
type: daily
date: YYYY-MM-DD
tags: [daily, {relevant-tags}]
---
```

---

### Rules

- Never put a `# Title` heading that duplicates the filename
- Use `[[wikilinks]]` for every project, person, and note reference
- Log only when there is something worth recording — not on every message
- If a decision was made, it should also go in `03 Intelligence/decisions/` as a separate record
- Keep session descriptions short and specific — "Bookly codebase read + README creation", not "worked on stuff"
