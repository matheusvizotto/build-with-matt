---
name: obsidian-setup
description: Build a complete AI-powered Obsidian vault from scratch. Asks 3 rounds of questions, then creates the full structure — numbered folders, Second Brain portable layer, knowledge index, automation scripts, hooks, and personalised context files. Trigger with /obsidian.
---

# Obsidian AI OS — Setup

USE WHEN the user runs `/obsidian` or asks to set up, bootstrap, or build an Obsidian vault.

This skill builds a portable, tool-independent knowledge system that works with Claude Code, Cursor, Gemini CLI, or any AI tool that reads markdown.

---

## Pre-flight Check

Check if `claude.md` or `CLAUDE.md` exists in the current working directory (CWD only — do not check parent directories).

**If it exists:**
Ask the user:
> "This folder already has an AI OS set up. What would you like to do?
> 1. **Re-run the interview** — keep the structure, update your context files
> 2. **Upgrade** — add the Second Brain portable layer if it's missing
> 3. **Cancel**"

Wait for their choice. Handle each path. Do not proceed with full setup without confirmation.

**If it does not exist:** proceed with Phase 0.

---

## Phase 0 — Mode Selection

Ask this question. Wait for the answer before continuing.

> **What kind of vault do you want to build?**
>
> **1. Personal AI OS** — for solopreneurs, freelancers, creators, and professionals who wear many hats. Blends work and personal. Best for individuals.
>
> **2. Business AI OS** — for teams, agencies, and companies. Adds departments, processes, stakeholders, and team-level structure.
>
> *(If you're unsure, pick 1 — you can always expand later.)*

Map:
- "1", "personal", "solo", "professional", "freelancer", "creator" → `os-mode: personal`
- "2", "business", "team", "company", "agency", "org" → `os-mode: business`
- Unclear → default to `os-mode: personal`

Store the selected mode. Used throughout Phase A and B.

---

### AI Name (ask immediately after mode selection)

> **What do you want to call your AI?**
> This is the name that appears in your vault hub and session logs — something personal like "JARVIS", "Atlas", or your own name.
>
> *(Skip to keep it as "Claude")*

- If the user gives a name → store as `{{AI_NAME}}`
- If the user skips or says "Claude", "default", "no preference" → use `Claude` as `{{AI_NAME}}`

This name will be used in `{{AI_NAME}}.md` (the AI identity hub), in hook messages, and in the `claude.md` pointer file.

---

## Phase 1 — The Three Questions

Ask these three questions **one at a time**. Wait for each answer before asking the next.
Do not ask follow-up questions. Accept whatever the user gives — a sentence, a wall of text, a LinkedIn bio, or "skip".

---

### Question 1 — Who are you?

> **Tell me about yourself.**
> Paste your bio, LinkedIn profile, personal website, or just describe who you are, what you do, and what you care about. The more you share, the better I can personalise your vault.
>
> *(You can skip — I'll use placeholders and you can fill them in later.)*

Extract from the answer:
- Full name
- Current role(s) or hat(s)
- Location (if mentioned)
- Skills or domains of expertise
- Goals or values (if mentioned)
- Company or brand name (if applicable)
- Social profiles (LinkedIn, Instagram, etc.)

---

### Question 2 — What are you working on?

> **What are you working on right now?**
> List your active projects, clients, goals, or initiatives. Paste a brief, a doc, a list — whatever you have. Or skip.

Extract from the answer:
- Project names
- Project types (website, app, content, client work, product, research)
- Status (planning, in progress, nearly done)
- Any collaborators or clients mentioned

---

### Question 3 — How do you communicate?

> **Do you create content or communicate publicly?**
> LinkedIn posts, newsletter, blog, YouTube, Instagram, podcasts — tell me what you publish, who your audience is, and what your goals are.
>
> *(Skip if you don't create content.)*

Extract from the answer:
- Platforms (LinkedIn, newsletter, blog, Instagram, YouTube, X)
- Audience description
- Content goals (build audience, attract clients, share knowledge)
- Voice or tone cues (if mentioned)
- Brand or publication name (if mentioned)

---

## Phase 2 — Build

Work silently. Do not narrate each step. Build everything, then confirm once at the end.

### Step A — Create Folder Structure

**All modes:**

```bash
mkdir -p "00 Inbox"
mkdir -p "01 Daily"
mkdir -p "02 Context"
mkdir -p "02 Context/voice"
mkdir -p "03 Intelligence/meetings/one-on-ones"
mkdir -p "03 Intelligence/meetings/client-calls"
mkdir -p "03 Intelligence/meetings/general"
mkdir -p "03 Intelligence/meetings/team-standups"
mkdir -p "03 Intelligence/decisions"
mkdir -p "03 Intelligence/competitors"
mkdir -p "03 Intelligence/market"
mkdir -p "04 Resources"
mkdir -p "05 Archives"
mkdir -p "Projects"
mkdir -p "Second Brain/skills"
mkdir -p "Second Brain/scripts"
mkdir -p "Second Brain/blueprint"
mkdir -p "knowledge"
mkdir -p ".claude/hooks"
```

**Business mode adds:**

```bash
mkdir -p "03 Intelligence/meetings/board-reviews"
mkdir -p "03 Intelligence/meetings/all-hands"
mkdir -p "03 Intelligence/processes"
mkdir -p "Departments"
mkdir -p "Teams"
```

**If content platforms mentioned in Question 3:**

```bash
mkdir -p "Content/linkedin"
mkdir -p "Content/newsletter"   # if newsletter mentioned
mkdir -p "Content/blog"         # if blog mentioned
mkdir -p "Content/instagram"    # if Instagram mentioned
```

---

### Step B — Write System Files

Read each reference file listed below and write its content to the target path. Replace all `{{PLACEHOLDER}}` tokens with real data from the interview before writing.

| Reference | Target path | Notes |
|---|---|---|
| `references/claude-md.md` | `./claude.md` | Pointer file — replace {{NAME}}, {{AI_NAME}} |
| `references/jarvis-md.md` | `./{{AI_NAME}}.md` | AI identity hub — replace all tokens. File is named after the chosen AI name |
| `references/vault-map.md` | `./Vault-Map.md` | Sitemap — add any extra folders |
| `references/second-brain-index.md` | `./Second Brain/index.md` | Entry point for all skills |
| `references/second-brain-operating-rules.md` | `./Second Brain/operating-rules.md` | How the AI operates |
| `references/second-brain-knowledge-routing.md` | `./Second Brain/knowledge-routing.md` | Where info gets routed |
| `references/session-start-hook.md` | `./.claude/hooks/session-start.py` | Auto-inject index at session start |
| `references/session-capture-hook.md` | `./.claude/hooks/session-capture.py` | Persist session before compaction |
| `references/vault-lint-py.md` | `./Second Brain/scripts/vault-lint.py` | Vault health check script |
| `references/vault-index-py.md` | `./Second Brain/scripts/vault-index.py` | Index rebuild script |
| `references/settings-json.md` | `./.claude/settings.json` | Hook config |

Make the hook files executable:
```bash
chmod +x .claude/hooks/session-start.py
chmod +x .claude/hooks/session-capture.py
```

---

### Step C — Write Context Files

Create these from the interview answers. Use the reference templates as structure guides — fill them with real content from what the user told you. Do NOT create empty placeholder files.

#### Always create: `02 Context/me.md`

The primary identity file. Fill with everything extracted from Question 1.

```markdown
---
type: reference
status: active
date: YYYY-MM-DD
tags: [identity, context, me]
---

# {{FULL NAME}}

> One-line summary of who this person is and what they do.

## Who I Am
[Role, background, what drives them]

## What I Do
[Current work, skills, domains]

## How I Work Best
[Communication preferences, work style, what to avoid]

## Current Focus
[Top 1-3 priorities right now]

## Goals
[Short and long-term goals if mentioned]

## Navigation
- [[Vault-Map]] — full vault structure
- [[{{AI_NAME}}]] — AI operating here
- [[Second Brain/index]] — all available skills
```

#### Create only if the user mentioned it:

- `02 Context/strategy.md` — if goals, vision, or priorities mentioned
- `02 Context/brand.md` — if voice, tone, content style, or audience mentioned
- `02 Context/business.md` — if company, clients, or business context mentioned (personal mode)
- `02 Context/organization.md` — business mode: company, product, org structure
- `02 Context/team.md` — business mode: team members and roles

For `02 Context/brand.md`, if content platforms were mentioned in Question 3:

```markdown
---
type: reference
status: active
date: YYYY-MM-DD
tags: [brand, voice, content]
---

# Brand & Voice

## Platforms
[LinkedIn / Newsletter / Blog / Instagram / YouTube — what they use]

## Audience
[Who they're writing for]

## Voice & Tone
[How they sound — direct, warm, data-driven, conversational, etc.]

## Content Goals
[What content is meant to accomplish]

## What to Avoid
[Phrases, formats, or approaches they dislike]
```

---

### Step D — Write Skill Files

Copy all portable skills into `Second Brain/skills/`. Write each one from the reference files.

| Reference | Target |
|---|---|
| `references/skills/daily-note.md` | `./Second Brain/skills/daily-note.md` |
| `references/skills/session-capture.md` | `./Second Brain/skills/session-capture.md` |
| `references/skills/process-inbox.md` | `./Second Brain/skills/process-inbox.md` |
| `references/skills/vault-lint.md` | `./Second Brain/skills/vault-lint.md` |
| `references/skills/vault-index.md` | `./Second Brain/skills/vault-index.md` |
| `references/skills/meet-ingest.md` | `./Second Brain/skills/meet-ingest.md` |
| `references/skills/ingest.md` | `./Second Brain/skills/ingest.md` |
| `references/skills/decision-record.md` | `./Second Brain/skills/decision-record.md` |
| `references/skills/meeting-notes.md` | `./Second Brain/skills/meeting-notes.md` |
| `references/skills/readme-creation.md` | `./Second Brain/skills/readme-creation.md` |
| `references/skills/project-setup.md` | `./Second Brain/skills/project-setup.md` |

**If content platforms mentioned (Question 3), also write:**

| Reference | Target |
|---|---|
| `references/skills/linkedin-post.md` | `./Second Brain/skills/linkedin-post.md` |
| `references/skills/newsletter-post.md` | `./Second Brain/skills/newsletter-post.md` |
| `references/skills/blog-post.md` | `./Second Brain/skills/blog-post.md` |
| `references/skills/instagram-post.md` | `./Second Brain/skills/instagram-post.md` |

---

### Step E — Create Project Folders

From Question 2, create a `Projects/{name}/README.md` for each project mentioned.

Apply the same logic as the MattAI setup skill:
- Simple mention → just `README.md`
- Moderate detail → `README.md` + relevant subdirs
- Rich detail → full structure

README template:
```markdown
---
type: project
status: active
date: YYYY-MM-DD
tags: [project]
---

## Overview
[What this project is]

## Current Status
[Where things stand]

## Next Steps
[What needs to happen next]
```

---

### Step F — Create Knowledge Index

Write `./knowledge/index.md` as a lightweight starting index:

```markdown
---
type: index
status: active
date: YYYY-MM-DD
tags: [index, navigation, knowledge]
---

# Knowledge Index

> Navigation hub. Loaded automatically at session start.
> Run `python3 Second Brain/scripts/vault-index.py` to rebuild after major changes.

---

## Identity & Context

| File | What's inside |
|---|---|
| `02 Context/me.md` | Who {{NAME}} is — identity, preferences, working style |
[add other context files created]

---

## Active Work

| File | What's inside |
|---|---|
| `Projects/` | All active projects |
| `01 Daily/` | Session logs |

---

## Second Brain — Portable Operating Layer

| File | What's inside |
|---|---|
| `Second Brain/index.md` | Entry point — all skills and maps |
| `Second Brain/knowledge-routing.md` | Where to route every type of information |
| `Second Brain/operating-rules.md` | How the AI behaves in this vault |
```

---

### Step G — First Daily Note

Create `./01 Daily/YYYY-MM-DD.md` (today's date):

```markdown
---
type: daily
date: YYYY-MM-DD
tags: [daily, setup]
---

## Session Log: Vault Setup

**Date:** YYYY-MM-DD
**Topics:** Initial vault setup and onboarding

> [!success] Vault Built
> - [x] Full AI OS structure created
> - [x] Context files personalised from onboarding
> - [x] Second Brain portable layer installed
> - [x] Automation scripts ready
> - [x] Hooks configured
[List projects created if any]

> [!todo] Next Steps
> - [ ] Open this folder as a vault in Obsidian
> - [ ] Run `python3 Second Brain/scripts/vault-lint.py` for first health check
> - [ ] Run `python3 Second Brain/scripts/vault-index.py` to rebuild the index
> - [ ] Add your first daily note tomorrow
```

---

## Phase 3 — Confirm

Tell the user what was created. Be specific, not generic.

```
✓ Vault built in [personal/business] mode.

Structure:
  00 Inbox/ — capture zone
  01 Daily/ — session logs (first note created)
  02 Context/ — [list which context files were created]
  03 Intelligence/ — meetings, decisions, market, competitors
  04 Resources/ — frameworks, references
  05 Archives/ — completed content
  Projects/ — [list project folders created]
  Second Brain/ — [X] skills + 2 automation scripts
  knowledge/ — navigation index

Your AI is named {{AI_NAME}} and lives at {{AI_NAME}}.md.

Next steps:
  1. Open this folder as a vault in Obsidian
  2. Run: python3 Second Brain/scripts/vault-index.py
  3. In your next session, just say "resume" — {{AI_NAME}} will orient itself from the index

Your AI OS is live. Every session from here builds on what came before.
```

---

## Rules

- Ask questions one at a time — never batch them
- Never create empty placeholder files — only create files with real content
- Replace all `{{PLACEHOLDER}}` tokens before writing
- Build silently after the interview — no narration, one confirmation at the end
- Accept any format: a sentence, a wall of text, a LinkedIn bio, or "skip"
- If the user skips all three questions, build the structure with generic placeholders and flag what needs filling in
