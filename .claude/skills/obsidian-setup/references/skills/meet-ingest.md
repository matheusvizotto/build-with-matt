---
type: skill
status: active
date: 2026-04-25
tags: [skill, meetings, google-meet, automation, ingest]
---

# Skill: Meet Ingest

Run this skill when the user says "process meetings", "import meeting notes", "sync Google Meet", or any equivalent.

The automated version runs every 30 minutes via launchd. This skill is the manual override.

---

## What This Skill Does

Pulls new Gemini meeting notes from Google Drive, parses them, and writes structured vault notes to `03 Intelligence/meetings/`.

---

## Automated Pipeline (always running)

```
Google Meet ends
  → Gemini writes "Notes from X" doc to Google Drive
  → meet-sync.py polls Drive every 30 min (launchd)
  → New doc found → exported as plain text
  → Parsed: summary, action items, participants, transcript
  → Meeting type detected → routed to correct subfolder
  → Vault note written to 03 Intelligence/meetings/{type}/{date}-{slug}.md
```

---

## Manual Trigger

```bash
# Check for new notes now (don't wait 30 min)
python3 "Second Brain/scripts/meet-sync.py"

# Dry run — see what would be processed
python3 "Second Brain/scripts/meet-sync.py" --dry-run

# Force reprocess a specific Drive file
python3 "Second Brain/scripts/meet-sync.py" --reprocess <FILE_ID>

# Check sync log
tail -50 ~/.config/meet-sync/meet-sync.log
```

---

## Meeting Type Routing

| Detected | Route to |
|---|---|
| Known 1:1 person (add names to ONE_ON_ONE_NAMES in meet-sync.py) | `one-on-ones/` |
| Client name (add names to CLIENT_NAMES in meet-sync.py) | `client-calls/` |
| Everything else | `general/` |

To add people or clients, update the `ONE_ON_ONE_NAMES` and `CLIENT_NAMES` sets in `meet-sync.py`.

---

## Output Format

```markdown
---
type: meeting
status: active
date: YYYY-MM-DD
participants: [Name, Name]
duration: X hr Y min
meeting-type: one-on-ones
tags: [meeting, one-on-one]
---

## Meeting Title

**Date:** YYYY-MM-DD HH:MM
**Duration:** X hr Y min
**Participants:** [[Person]]

> [!summary] Summary
> Gemini AI summary

### Action Items
- [ ] item (Person)

### Transcript (excerpt)
```

---

## First-Time Setup

```bash
bash "Second Brain/scripts/meet-setup.sh"
```

Steps it runs:
1. `pip3 install google-api-python-client`
2. OAuth2 flow (opens browser once)
3. Installs launchd agent at `~/Library/LaunchAgents/com.meet-sync.plist`
4. Runs first sync

**Prerequisite:** Google credentials JSON from Google Cloud Console → save to `~/.config/meet-sync/google-credentials.json`

---

## State and Logs

- `~/.config/meet-sync/meet-sync-state.json` — list of processed Drive file IDs (prevents duplicates)
- `~/.config/meet-sync/meet-sync.log` — stdout log
- `~/.config/meet-sync/meet-sync-error.log` — error log
