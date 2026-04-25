#!/usr/bin/env python3
"""
{{AI_NAME}} Session Capture Hook
Fires on PreCompact — reminds the AI to persist session knowledge
before the context window is compressed.
"""
from datetime import date

today = date.today().isoformat()

print(f"""[{{AI_NAME}} — Session Capture]
Context is about to compact. Persist this session to 01 Daily/{today}.md now.

Use this schema under a new ## Session Log entry:

**Decisions made:** (architectural, strategic, or product decisions taken this session)
**Lessons learned:** (what worked, what failed, what was discovered)
**Action items:** (concrete next steps — specific, owned, dated if possible)

Route any new project info, corrections, or decisions to the right vault file before compacting.
""")
