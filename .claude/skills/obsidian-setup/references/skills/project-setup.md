---
type: skill
skill: project-setup
status: active
date: 2026-04-24
tags: [skill, aios, project, setup, portable]
---

## Skill: Project Setup

When asked to set up a new project in the vault, follow this process.

---

### Step 1 — Determine Project Type

| Type | Vault location | Code location |
|---|---|---|
| Website client | `Projects/Website Project/Site \| {Name}/` | `~/Documents/Code/clients/websites/{slug}` |
| App | `Projects/App Project/{Name}/` | `~/Documents/Code/apps/{slug}` |
| Code repository | `Projects/Repository/{Name}/` | `~/Documents/Code/repos/{slug}` |
| Client (full engagement) | `Projects/{Name} \| Project/` | `~/Documents/Code/clients/{slug}` |
| Personal brand project | `Matt.AI/Projects/{name}/` | varies |

---

### Step 2 — Create the README

Follow `Second Brain/skills/readme-creation.md` for the full schema.

Minimum fields in frontmatter:
```yaml
---
type: project
project: {name}
status: active
date: YYYY-MM-DD
tags: [project, {type}]
---
```

---

### Step 3 — Link from Hub Notes

After creating the README, link it from:
- `Projects/Active Projects.md` — add row to the correct table
- `knowledge/index.md` — add to Key Projects table if significant
- Parent folder README if one exists (e.g. `Projects/Website Project/`)
- `02 Context/me.md` if it's a personal brand project

Never create an orphan note.

---

### Step 4 — Create Sub-structure (on demand)

Do not pre-create empty directories. Add subdirs as content arrives:

| Content type | Subdir |
|---|---|
| Research findings | `research/` |
| Specs or briefs | `specs/` |
| Drafts or written content | `drafts/` |
| Ideas or brainstorms | `ideas/` |
| Meeting notes | `meetings/` |
| Feedback | `feedback/` |

---

### Step 5 — Log in Daily Note

Add a session entry to `01 Daily/YYYY-MM-DD.md` noting the project was set up and what was created.
