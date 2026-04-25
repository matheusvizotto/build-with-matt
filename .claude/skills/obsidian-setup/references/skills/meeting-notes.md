---
type: skill
skill: meeting-notes
status: active
date: 2026-04-24
tags: [skill, aios, meetings, intelligence, portable]
---

## Skill: Meeting Notes

When asked to document a meeting, follow this process.

---

### File Location

`03 Intelligence/meetings/{type}/YYYY-MM-DD-{slug}.md`

Meeting types and their subfolders:

| Meeting type | Subfolder |
|---|---|
| Team standups | `team-standups/` |
| Client calls | `client-calls/` |
| One-on-ones | `one-on-ones/` |
| General / other | `general/` |

---

### Schema

```markdown
---
type: meeting
date: YYYY-MM-DD
project: {project-name if applicable}
attendees: [Name1, Name2]
status: completed
tags: [meeting, {type}]
---

> Project: [[project-link]] · Next meeting: TBD

## Meeting: {Short Title}

**Date:** YYYY-MM-DD
**Attendees:** Name1, Name2
**Context:** One sentence on what this meeting was about

---

## Key Discussion Points

- Point 1
- Point 2

## Decisions Made

- Decision 1 (file as decision record if significant)

## Action Items

- [ ] Action 1 — Owner — Due date
- [ ] Action 2 — Owner — Due date

## Notes

Anything else worth capturing.
```

---

### After Saving

- If a significant decision was made during the meeting, also file it in `03 Intelligence/decisions/YYYY-MM-DD-{title}.md`
- Link the meeting note from the relevant project README if project-specific
- Add action items to task system if owners and due dates are known
