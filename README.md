# Product Management Claude Workspace

A portable, AI-powered PM operating system built on [Claude Code](https://claude.ai/code). Open this folder in Claude Code and get an AI assistant that knows your product, tracks your customers, runs your weekly business review, and helps you write better PRDs — all within your own files.

---

## What's in This Workspace

| Folder | What it does |
|--------|-------------|
| `Analytics/` | Weekly business reviews, exec decks, telemetry queries |
| `Customers/` | One folder per key customer — engagement notes, health, pilots |
| `Knowledge/` | Competitive intel, metrics framework, strategy docs, user research |
| `Projects/` | Active initiative workstreams with structured overviews |
| `Team/` | Weekly focus docs, team rituals |
| `.claude/skills/` | AI skills you can invoke with `/skill-name` |

### Skills Available

| Skill | What it does |
|-------|-------------|
| `/weekly-business-review` | Generates your weekly data review from live telemetry |
| `/weekly-focus` | Synthesizes emails + project context into a prioritized week plan |
| `/feedback-report` | Turns a customer feedback export into a structured themes report |
| `/send-weekly-reports` | Emails reports to your stakeholders via Microsoft Graph API |
| `/write-prd` | Writes a structured PRD using an interactive interview |
| `/product-telemetry` | Connects to your telemetry stack and builds calibrated queries |
| `/hydrate-knowledge` | Bootstraps this workspace from a URL or a folder of existing docs |
| `/calibrate-metric` | Validates a dashboard metric against your telemetry source of truth |

---

## Getting Started: Hydrate in ~30 Minutes

### Step 1: Bootstrap your knowledge (5 min)

Point Claude at your existing docs or product website. Choose one or both:

**Option A — From a URL:**
```
/hydrate-knowledge https://your-product-docs.com
```

**Option B — From an existing folder** (PRDs, OKRs, roadmaps, strategy decks):
```
/hydrate-knowledge path/to/your/existing/docs/folder
```

Claude will read your documents, extract product context (name, description, OKRs, users, competitors), pre-fill `CLAUDE.md`, and seed the `Knowledge/` folder.

### Step 2: Finish filling CLAUDE.md (10 min)

Open [CLAUDE.md](CLAUDE.md) and complete the **HYDRATION CHECKLIST** at the top. The items marked `[AUTO]` may already be filled by Step 1. Complete the rest manually — especially:
- OKR table (targets and current numbers)
- Team name and manager
- Your team's recurring rituals

Delete the checklist block when done.

### Step 3: Set up telemetry (30–60 min, one-time)

Connect Claude to your product's data:
```
/product-telemetry setup
```

Claude will ask about your telemetry stack (Snowflake, BigQuery, Kusto, Databricks, etc.) and guide you through discovering schema and calibrating your first queries.

### Step 4: Add your first customer (5 min)

```bash
cp -r Customers/_SAMPLE-CUSTOMER Customers/YOUR-CUSTOMER-NAME
```

Open the copied folder and fill in the templates.

### Step 5: Configure email reports (5 min)

Open `.claude/skills/send-weekly-reports/skill.md` and update the recipients section with your stakeholders' email addresses.

### Step 6: Run your first weekly review (10 min)

```
/weekly-business-review
```

Review the output in `Analytics/Weekly Reports/`. Iterate from there.

---

## How Claude Uses This Workspace

When you open this folder in Claude Code, it reads `CLAUDE.md` automatically — giving Claude context about your product, OKRs, working principles, and priorities. Every response is grounded in your actual product context.

The `Knowledge/`, `Customers/`, and `Projects/` folders act as a living knowledge base. The more you add to them, the better Claude's responses get.

---

## Folder Structure Details

```
/
├── CLAUDE.md                          ← Claude's instructions (fill this in)
├── README.md                          ← This file
├── .gitignore
├── .claude/
│   └── skills/
│       ├── weekly-business-review/    ← /weekly-business-review
│       ├── feedback-report/           ← /feedback-report
│       ├── send-weekly-reports/       ← /send-weekly-reports
│       ├── weekly-focus/              ← /weekly-focus
│       ├── write-prd/                 ← /write-prd
│       ├── product-telemetry/         ← /product-telemetry
│       ├── hydrate-knowledge/         ← /hydrate-knowledge
│       └── calibrate-metric/          ← /calibrate-metric
├── Analytics/
│   ├── Weekly Business Review - Instructions.md
│   ├── Weekly Reports/               ← Auto-generated WBR reports
│   └── Exec reviews/                 ← Exec-facing decks and briefings
├── Customers/
│   └── _SAMPLE-CUSTOMER/             ← Copy for each customer
├── Knowledge/
│   ├── compete/                      ← Competitive intelligence
│   ├── metrics/                      ← Success metrics framework
│   ├── org/                          ← Team context
│   ├── product/                      ← Product docs
│   ├── strategy/                     ← Strategy documents
│   └── user-research/                ← User research
├── Projects/
│   └── _TEMPLATE-INITIATIVE/         ← Copy for each initiative
└── Team/
    └── Weekly Focus Areas/           ← Auto-generated weekly focus docs
```

---

## Tips

- **Add context incrementally** — you don't need everything filled in to get value. Start with CLAUDE.md and one customer folder.
- **Use `/weekly-focus` on Mondays** — it synthesizes your recent emails and project state into a prioritized week plan.
- **Run `/feedback-report` weekly** — if you have a customer feedback source, this turns raw data into themes in seconds.
- **Keep `Projects/` current** — Claude uses it to understand what's in-flight when prioritizing work.
- **Push to git** — this workspace is designed to live in a git repo. Track your knowledge base changes over time.

---

*Built on [Claude Code](https://claude.ai/code). Inspired by the PM OS pattern developed for Microsoft Dynamics 365 Sales.*
