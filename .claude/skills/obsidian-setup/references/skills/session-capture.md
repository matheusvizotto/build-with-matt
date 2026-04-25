---
type: skill
skill: session-capture
status: active
date: 2026-04-24
tags: [skill, aios, session, daily, portable]
---

## Skill: Session Capture

When a session ends or context is about to compact, persist everything meaningful to the vault.

---

### Trigger

This runs automatically via `.claude/hooks/session-capture.py` on PreCompact.
Also run manually at the end of any substantive work session.

---

### What to Persist

| Content type | Route to |
|---|---|
| Session progress | `01 Daily/YYYY-MM-DD.md` (append, never overwrite) |
| Architectural or strategic decision | `03 Intelligence/decisions/YYYY-MM-DD-{title}.md` |
| Project update (status, feature, client) | `Projects/{name}/README.md` |
| New competitive insight | `03 Intelligence/competitors/{name}.md` |
| New market data | `03 Intelligence/market/{topic}.md` |
| User preference or correction | `02 Context/me.md` (Rules section) |
| Reusable resource | `04 Resources/` |

Skip if the session was casual chat with nothing worth recording.

---

### Daily Note Format

See `Second Brain/skills/daily-note.md` for the full schema.

Minimum required per session log:
- What was built or decided (use `> [!success]` callout)
- Decisions made
- Lessons learned
- Action items (checkboxes)

---

### Rules

- Never leave a session without persisting decisions and action items
- Never ask permission to save — route and save, then report what was saved
- If a decision was made, file both in the daily note AND in `03 Intelligence/decisions/`
- Update `knowledge/index.md` if a new key resource was created
- Update `Projects/Active Projects.md` if a project status changed
