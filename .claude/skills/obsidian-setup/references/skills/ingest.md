---
type: skill
skill: ingest
status: active
date: 2026-04-24
tags: [skill, aios, ingest, knowledge, routing, portable]
---

## Skill: Ingest

When a raw source arrives — URL, article, research paper, Apify scrape, Gist, social post, podcast note, or unprocessed Inbox file — run this skill to convert it into structured vault knowledge.

---

### Trigger

Run when:
- The user drops a URL, pastes content, or shares a file mid-session
- The user says "read this", "process this", "add this to the vault"
- `00 Inbox/` contains unprocessed files at session start
- An Apify actor run completes and returns structured data
- A web scrape, market research result, or competitive intel arrives

---

### Step 1 — Classify the Source

| Source type | Signals |
|---|---|
| Market research / industry data | Stats, percentages, trends, named studies |
| Competitive intel | Competitor product, pricing, strategy, positioning |
| Tool or framework reference | GitHub repo, Gist, docs site, library README |
| Personal brand insight | Content strategy, platform algorithm, engagement data |
| Project-relevant research | Directly about a client, product, or active project |
| Raw capture (unclassified) | Anything in `00 Inbox/` without clear type |

---

### Step 2 — Extract

Pull only what has lasting value. Skip summaries, recaps, and filler.

**Always extract:**
- Key claims with numbers (stat, source name, year)
- Strategic implications — what this means for Matheus's work specifically
- Contradictions with existing vault knowledge (flag these explicitly)

**Source quality check before extracting:**
- Name the source — never "a study found" or "experts say"
- Note the date the data was collected (not just published)
- Flag data older than 18 months as potentially stale

---

### Step 3 — Route

Use `Second Brain/knowledge-routing.md` to decide the file. Common routes:

| Source content | Route |
|---|---|
| Industry trend, platform data, market stat | `03 Intelligence/market/{topic}.md` |
| Competitor product or strategy update | `03 Intelligence/competitors/{name}.md` |
| Decision trigger (forces a choice) | `03 Intelligence/decisions/YYYY-MM-DD-{title}.md` |
| Tool or framework directly used in a project | `Projects/{name}/research/{topic}.md` |
| Personal brand / content strategy data | `Matt.AI/` — relevant skill or project file |
| Reusable prompt, template, or framework | `04 Resources/{topic}.md` |
| Cannot be classified yet | Leave in `00 Inbox/` with a `type: inbox` frontmatter tag and a one-line summary at the top |

---

### Step 4 — Write

Write to the target file. If the file exists, append — never overwrite.

**New file schema:**

```markdown
---
type: {market-research | reference | competitive-intel | resource}
topic: {Topic Name}
status: active
date: YYYY-MM-DD
tags: [{relevant}, {tags}]
---

> Related: [[linked-project]] · [[linked-context]]

## {Topic}

**What this covers:** One sentence.

**Last updated:** YYYY-MM-DD

---

## Key Findings

| Finding | Source | Year | Relevance |
|---|---|---|---|

## Strategic Implications

What this means for Matheus's active projects — specific, not generic.

## Sources

- [Source Name](URL) — what it covers, date accessed
```

**Appending to existing file:**
- Add new findings to the Key Findings table
- Add new implications below the existing ones with a date stamp
- Update the `Last updated` field in frontmatter

---

### Step 5 — Integrate

After writing, link the new content into the graph:

1. If it relates to an active project, add a reference in `Projects/{name}/README.md`
2. If it's significant market or competitive data, link from `03 Intelligence/Knowledge Hub.md`
3. If it changes a decision or strategy, link from `02 Context/strategy.md`
4. If it's a new resource, link from the relevant skill file in `Second Brain/skills/`

---

### Inbox Processing

When processing `00 Inbox/`:

1. List all files: `ls "00 Inbox/"`
2. For each file: classify → extract → route → delete or keep stub
3. Never leave a file in Inbox permanently
4. If a file cannot be routed yet, add a one-line summary to its frontmatter and note it in the daily log with an action item

---

### Rules

- Every stat needs: the number, the source name, the year
- Never create a new market or research file if one already exists on the same topic — append
- Contradictions with existing vault notes get flagged explicitly, not silently overwritten
- Route immediately — never pile content back into Inbox as a holding pattern
- After ingesting, update `01 Daily/YYYY-MM-DD.md` with what was processed and where it landed
