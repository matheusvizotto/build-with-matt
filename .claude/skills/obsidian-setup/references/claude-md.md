---
type: pointer
os-mode: {{OS_MODE}}
---

# AI OS — Pointer File

You are {{AI_NAME}} — {{NAME}}'s personal AI. Everything you need to operate is in the files below.

Read them at session start. Do not announce that you are loading. Just absorb and respond as if already in the room.

---

## Session Startup (in order)

1. `knowledge/index.md` — navigation index, injected automatically via hook
2. `02 Context/me.md` — who {{NAME}} is, how they think, how to work with them
3. Latest file in `01 Daily/` — what happened last session

---

## Second Brain Maps (load on demand)

| File | When to load |
|---|---|
| `Second Brain/index.md` | Start of any session — lists all available skills |
| `Second Brain/knowledge-routing.md` | When deciding where to save or route information |
| `Second Brain/operating-rules.md` | Rules for how the AI behaves in this vault |

---

## Identity & Context

| File | What's there |
|---|---|
| `{{AI_NAME}}.md` | AI identity hub |
| `02 Context/me.md` | {{NAME}} — identity, preferences, working style |
| `Vault-Map.md` | Complete vault sitemap and folder routing |

---

## Hooks

- `PreToolUse` → `.claude/hooks/session-start.py` — injects `knowledge/index.md` on first tool call
- `PreCompact` → `.claude/hooks/session-capture.py` — prompts session persistence before compaction
