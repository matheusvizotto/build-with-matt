---
type: skill
skill: decision-record
status: active
date: 2026-04-24
tags: [skill, aios, decision, intelligence, portable]
---

## Skill: Decision Record

When a consequential architectural, strategic, or product decision is made, file it here.

---

### What Qualifies

File a decision record when:
- A technology or stack choice is made (e.g. "chose Supabase over Neon")
- A structural change to the vault or a project is decided
- A product direction is confirmed or reversed
- A pricing, positioning, or business decision is made
- A client relationship decision is made (e.g. "paused GS Porto engagement")

Do not file for: routine tasks, content choices, minor edits.

---

### File Location

`03 Intelligence/decisions/YYYY-MM-DD-{short-title}.md`

Use today's date. Title should be lowercase, hyphenated, specific.

Examples:
- `2026-04-24-bookly-switched-to-supabase.md`
- `2026-04-24-mirrofish-not-built-by-matheus.md`
- `2026-04-24-aios-folder-created.md`

---

### Schema

```markdown
---
type: decision
date: YYYY-MM-DD
project: {project-name or "aios" or "vault"}
status: active
tags: [decision, {relevant-tags}]
---

## Decision: {Title}

**What was decided:** One clear sentence.

**Why:** The reasoning — constraints, data, tradeoffs considered.

**Alternatives considered:** What else was on the table and why it was rejected.

**Consequences:** What this decision enables or closes off.

**Linked notes:** [[relevant-project]] · [[relevant-context]]
```

---

### Rules

- File immediately when the decision is made — not later
- Keep it short — the schema is the whole record, no padding
- Link to the project or context note it affects
- Reference from the project README if significant
