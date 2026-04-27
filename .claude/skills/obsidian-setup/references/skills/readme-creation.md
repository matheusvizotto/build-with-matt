---
type: skill
skill: readme-creation
status: active
date: 2026-04-24
tags: [skill, aios, readme, project, portable]
---

## Skill: README Creation

When asked to document a project or codebase, follow this process.

---

### Step 1 — Read Before Writing

Before writing a single line, read:
- `package.json` / `requirements.txt` / `Cargo.toml` — stack, deps, scripts
- `prisma/schema.prisma` or equivalent — data model
- `.env.local` or `.env.example` — real infrastructure (not guessed)
- `AGENTS.md`, `CHANGELOG.md`, `README.md` if they exist
- Key entry points: `src/`, `app/`, `routes/`, `lib/`
- Any existing vault notes for this project

Do not guess. Do not invent. If something is not in the code, flag it as unknown.

---

### Step 2 — File Location

Vault notes for projects:
- Website projects: `Projects/Website Projects/Site | {Name}/README.md`
- App projects: `Projects/App Project/{Name}/README.md`
- Repos: `Projects/Repository/{Name}/README.md`
- Client projects: `Projects/{Name} | Project/README.md`

Code stays in `~/Documents/Code/`. Vault notes stay in the vault.

---

### Schema

```markdown
---
type: project
project: {name}
status: {active | live | evaluating | paused | archived}
date: YYYY-MM-DD
tags: [project, {relevant-tags}]
---

> Links: [[related-note]] · [[another-note]]

## What It Is

One paragraph. What the project is, who it's for, what problem it solves.

## Status

Current deployment status, live URL if applicable, which clients/tenants are active.

## Stack

Tech stack. Be specific — include versions if relevant.

## Features

Split into: Live (confirmed working) vs Stub (UI exists, not functional).

## Key Architecture Notes

Non-obvious things about how it's built. Data model, multi-tenancy, auth, etc.

## Dev Commands

How to run it locally. Just the commands.

## Open Questions / Gaps

Things that are unclear, missing, or need investigation.

## Links

[[related-vault-note]] references.
```

---

### Rules

- Never create an orphan note — always link from at least one existing note (README, Active Projects, etc.)
- After creating a README, update `Projects/Active Projects.md` with the new entry
- Update `knowledge/index.md` if the project is new or the status changed significantly
- Use `[[wikilinks]]` for every project and note reference
- Status must reflect reality — if it's live with real clients, say so
