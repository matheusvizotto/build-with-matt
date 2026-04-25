---
type: reference
status: active
tags: [navigation, vault-map, ai-os]
updated: {{DATE}}
---

# Vault Map

Complete sitemap of this vault. Read this file whenever you need to know where something lives or where to route new information.

---

## Root Structure

```
claude.md                      — AI pointer file (thin — logic lives in me.md)
JARVIS.md                      — AI identity hub
Vault-Map.md                   — This file: full sitemap + routing
00 Inbox/                      — Capture zone: unprocessed notes, drop here first
01 Daily/                      — Session logs (YYYY-MM-DD.md)
02 Context/                    — Identity layer (me.md, strategy.md, brand.md)
03 Intelligence/               — Meetings, decisions, competitors, market
04 Resources/                  — Prompts, frameworks, swipe files, references
05 Archives/                   — Completed and historical content
Projects/                      — All active projects
Second Brain/                          — Portable operating layer: skills, maps, scripts
knowledge/                     — Auto-generated navigation index
```

---

## Routing Table

Every piece of information has a home.

| Type | Route to |
|---|---|
| Unprocessed capture | `00 Inbox/` — process and route later |
| Session log | `01 Daily/YYYY-MM-DD.md` |
| Identity, preferences, voice | `02 Context/me.md` |
| Goals and strategy | `02 Context/strategy.md` |
| Brand, tone, content rules | `02 Context/brand.md` |
| Meeting notes | `03 Intelligence/meetings/{type}/` |
| Decision records | `03 Intelligence/decisions/` |
| Competitor research | `03 Intelligence/competitors/` |
| Market data | `03 Intelligence/market/` |
| Reference material, frameworks | `04 Resources/` |
| Completed projects | `05 Archives/` |
| Any active project | `Projects/{project-name}/` |

---

## Projects

Each project lives in `Projects/{name}/` with a `README.md` as entry point.
See [[Projects]] for the full list.

---

## Second Brain — Portable Operating Layer

| File | Purpose |
|---|---|
| `Second Brain/index.md` | Entry point — all skills and scripts |
| `Second Brain/operating-rules.md` | How the AI behaves in this vault |
| `Second Brain/knowledge-routing.md` | Detailed routing rules |
| `Second Brain/skills/` | Portable process skills |
| `Second Brain/scripts/` | vault-lint.py, vault-index.py |

---

## Naming Conventions

- Vault notes: `kebab-case.md`
- Daily notes: `YYYY-MM-DD.md`
- Projects: `Project Name/README.md`
- Decisions: `YYYY-MM-DD-decision-title.md`
- Meetings: `YYYY-MM-DD-person-or-topic.md`

## Wikilink Rules

- Always use `[[wikilinks]]` for internal notes — never `[markdown](links)`
- Every note must link FROM at least one existing note (no orphans)
- Every project, person, and key concept gets a wikilink when mentioned
