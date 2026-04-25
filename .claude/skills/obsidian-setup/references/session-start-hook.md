#!/usr/bin/env python3
"""
{{AI_NAME}} Session Start Hook
Injects knowledge/index.md into context once per session.
Fires on PreToolUse — only outputs on the first tool call of each session.
"""
import os
import sys
from pathlib import Path

VAULT = Path.cwd()
INDEX = VAULT / "knowledge" / "index.md"

# One lock per Claude process — new session = new PID
lock = Path(f"/tmp/jarvis_{os.getppid()}.lock")

if lock.exists():
    sys.exit(0)

lock.touch()

if not INDEX.exists():
    print("[{{AI_NAME}}] knowledge/index.md not found — run `python3 Second Brain/scripts/vault-index.py` to create it.")
    sys.exit(0)

content = INDEX.read_text()
print(f"[{{AI_NAME}} — Session Start]\n\n{content}")
