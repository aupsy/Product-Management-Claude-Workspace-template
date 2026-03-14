---
name: hydrate-knowledge
description: Bootstrap this workspace from a product URL, docs site, folder of existing documents, or connected productivity tool
user-invocable: true
argument-hint: <URL or folder path or "workspace"> [additional sources...]
allowed-tools: WebFetch, Read, Glob, Write, Edit, Bash, mcp__workiq__ask_work_iq
---

# Hydrate Knowledge

Bootstraps this workspace by extracting product context from a URL, a docs site, or a folder of existing documents. Fills in `CLAUDE.md` placeholders and seeds the `Knowledge/` folder.

**User's request:** $ARGUMENTS

---

## Step 0: Parse Arguments

Arguments can be one or more:
- **URLs** — product website, docs site, GitHub repo, marketing pages
- **Folder paths** — local folder containing PRDs, OKRs, roadmaps, strategy docs, meeting notes
- **`workspace`** — pull context from your connected productivity tool (email, meetings, Slack)

Examples:
```
/hydrate-knowledge https://docs.myproduct.com
/hydrate-knowledge "C:\Users\me\Documents\Product Docs"
/hydrate-knowledge https://docs.myproduct.com "C:\Docs\OKRs"
/hydrate-knowledge workspace
/hydrate-knowledge workspace https://docs.myproduct.com
```

Process all provided sources. Merge and deduplicate the extracted context at the end.

---

## Step 1A: Fetch from URLs

For each URL:

1. Fetch the page with `WebFetch`
2. If it's a documentation site, also try to fetch:
   - `/docs`, `/documentation`, `/guide`, `/overview`, `/getting-started`
   - Any navigation links visible on the homepage
3. Extract from each page:
   - Product name and tagline
   - What the product does (1-3 sentence description)
   - Who the users are (job titles, use cases)
   - Key features / capabilities
   - Pricing tiers or packaging (signals addressable market)
   - Known competitors (if mentioned)
   - Key metrics the product helps users achieve

Limit to 5-8 pages total to avoid excessive fetching.

---

## Step 1B: Read from Folder

For each folder path:

1. List all readable files:
   ```bash
   find "[FOLDER]" -type f \( -name "*.md" -o -name "*.txt" -o -name "*.csv" \)
   ```
2. Prioritize files by likely relevance (read these first):
   - Files named: `OKR*`, `okr*`, `strategy*`, `roadmap*`, `*PRD*`, `*prd*`, `*brief*`, `*vision*`, `*overview*`
   - Files in folders named: `strategy`, `planning`, `product`, `OKRs`, `roadmaps`
3. Read the prioritized files (up to 20 files, largest first within priority group)
4. Extract:
   - Product name and description
   - OKRs / key results with targets and current values
   - Strategic priorities
   - Team name and org context
   - Known customers or use cases
   - Competitive context
   - Feature areas and roadmap items
   - User personas

---

## Step 1C: Pull from Productivity Tool (if "workspace" argument provided)

Read `.claude/skills/productivity-tools.md` to determine which tool is active.

### If Microsoft 365 / WorkIQ:
Call `mcp__workiq__ask_work_iq`:
> "What product are you currently working on as a PM? Summarize: (1) the product name and what it does, (2) the team name and key stakeholders, (3) any OKRs, targets, or strategic priorities mentioned in recent documents or meetings, (4) top customer names or use cases, (5) known competitors or market context. Focus on the past 3 months of context."

### If Slack:
Search all configured channels for:
- Product name, description, and announcements
- OKR reviews or planning threads
- Customer mentions and use case discussions
- Competitive discussions

### If Google Workspace:
Query recent emails and docs for:
- Product planning documents
- OKR review threads
- Customer meeting follow-ups mentioning product context

Extract the same fields as Step 1B (product name, OKRs, team, customers, competitors). Merge with other sources.

---

## Step 2: Synthesize Extracted Context

Merge all extracted information into a structured context object:

```
product_name: [extracted]
product_short: [extracted acronym or abbreviation]
product_description: [1-2 sentences]
team_name: [extracted or "unknown"]
company_org: [extracted or "unknown"]
users: [who uses it]
okrs: [list of OKRs with targets if found]
key_features: [list]
competitors: [list]
strategic_themes: [list]
```

For each field: note the source (URL or file) and confidence (High / Medium / Low).

---

## Step 3: Update CLAUDE.md

Read `CLAUDE.md`. For each `[PLACEHOLDER]` that has a high-confidence match from Step 2, replace it:

| Placeholder | Replace with |
|-------------|-------------|
| `[YOUR-PRODUCT-NAME]` | extracted `product_name` |
| `[YOUR-PRODUCT-SHORT]` | extracted `product_short` |
| `[YOUR-PRODUCT-DESC]` | extracted `product_description` |
| `[YOUR-TEAM-NAME]` | extracted `team_name` (if found) |
| `[YOUR-COMPANY]` | extracted `company_org` (if found) |
| `[YOUR-USER-TYPE]` | extracted `users` |

For the OKR table: if OKRs were found with targets and current values, replace the placeholder rows. If only OKR names were found (no numbers), fill in the name and leave `[TARGET]` / `[CURRENT]` for manual completion.

**Do not replace** `[YOUR-MANAGER-NAME]`, `[YOUR-PRIORITY-LENS]`, or `[YOUR-RITUAL-*]` — these require human input.

---

## Step 4: Seed Knowledge/ Folder

Create starter files from the extracted context:

**`Knowledge/product/product-overview.md`**
```markdown
# [Product Name] — Product Overview

*Auto-generated by /hydrate-knowledge on [DATE]. Review and update.*

## What It Does
[extracted description]

## Who Uses It
[extracted user types]

## Key Capabilities
[extracted feature list]

## Sources
[list of URLs/files used]
```

**`Knowledge/compete/competitive-landscape.md`** (if competitors were found)
```markdown
# Competitive Landscape — [Product Name]

*Auto-generated by /hydrate-knowledge on [DATE]. Expand with /compete research.*

## Known Competitors
[list of competitors extracted]

## Notes
[any competitive context extracted from docs]
```

**`Knowledge/strategy/strategic-priorities.md`** (if strategy docs were read)
```markdown
# Strategic Priorities — [Product Name]

*Extracted from: [source files]*

## Themes
[extracted strategic themes]

## OKRs
[extracted OKRs]
```

---

## Step 5: Report to User

Show a summary:

```
✅ Hydration complete

Sources read:
  - [URL 1] — [X] pages fetched
  - [Folder path] — [Y] files read

CLAUDE.md placeholders filled:
  ✓ [YOUR-PRODUCT-NAME] → "Payments Gateway"
  ✓ [YOUR-PRODUCT-DESC] → "Payments Gateway processes..."
  ✓ [YOUR-USER-TYPE] → "developers, finance teams"
  ⚠ [YOUR-TEAM-NAME] → not found (fill in manually)
  ⚠ [YOUR-MANAGER-NAME] → not filled (personal info)

Knowledge files created:
  ✓ Knowledge/product/product-overview.md
  ✓ Knowledge/compete/competitive-landscape.md
  ✓ Knowledge/strategy/strategic-priorities.md

Still needed (complete the HYDRATION CHECKLIST in CLAUDE.md):
  - OKR targets and current values
  - Team name, manager name
  - Recurring rituals
  - Telemetry stack → run /product-telemetry setup
```

---

## Notes

- This skill never writes passwords, API keys, or PII to any file
- All generated files are marked with `*Auto-generated*` so you know what to review
- Running `/hydrate-knowledge` again will update existing files rather than creating duplicates
- The quality of hydration depends on the richness of the source docs — the more context in your docs/URL, the more gets filled in automatically
