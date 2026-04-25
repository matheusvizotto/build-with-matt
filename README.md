# MattAI — Obsidian AI OS

A portable, tool-independent knowledge system for Obsidian. Works with Claude Code, Cursor, Gemini CLI, or any AI tool that reads markdown.

Built and maintained by [Matheus Vizotto](https://instagram.com/buildwithmatt.ai).

---

## What this is

A complete `.claude/` skill system that turns Obsidian into a personal AI OS. One command builds your entire vault from scratch — personalised to who you are and what you're working on.

---

## Quick start

1. Clone this repo into a folder you want as your vault:
   ```bash
   git clone https://github.com/matheusvizotto/build-with-matt.git my-vault
   cd my-vault
   ```

2. Open the folder as a vault in Obsidian.

3. Open Claude Code in that folder and run:
   ```
   /obsidian
   ```

4. Answer 5 questions. Your vault is built.

---

## What `/obsidian` builds

```
00 Inbox/           — capture zone
01 Daily/           — session logs
02 Context/         — your identity, strategy, brand
03 Intelligence/    — meetings, decisions, competitors, market
04 Resources/       — frameworks, templates, references
05 Archives/        — completed content
Projects/           — active work, intelligently structured
Second Brain/       — portable skill layer + automation scripts
knowledge/          — navigation index, rebuilt automatically
.claude/hooks/      — session automation (inject context, persist sessions)
```

Plus a personalised AI identity hub (you choose the name), context files filled from your answers, and project folders for anything you mentioned.

---

## Available skills (slash commands)

| Command | What it does |
|---------|-------------|
| `/obsidian` | Build a new vault from scratch |
| `/daily` | Create today's daily note |
| `/ingest` | Process and route any raw content |
| `/meeting` | Log a meeting with structured notes |
| `/decision` | File a decision record |
| `/project` | Set up a new project folder |
| `/linkedin` | Write a LinkedIn post in your voice |
| `/newsletter` | Draft a newsletter edition |
| `/blog` | Write a long-form blog post |
| `/seo-audit` | Full website SEO audit |
| `/seo-page` | Single page analysis |
| `/email-sequence` | Build an email sequence |

Full list: `/seo` for all SEO skills, `/marketing` for all marketing skills.

---

## Requirements

- [Obsidian](https://obsidian.md) (free)
- [Claude Code](https://claude.ai/code) (CLI or desktop)

---

## License

MIT
