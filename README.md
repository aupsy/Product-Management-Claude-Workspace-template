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

## Frequently Asked Questions
### What does this cost?
The cost scales with usage. A Claude Pro subscription covers individual use to start. Once you're sharing the workspace across a team and running weekly automations, a team of five can expect to consume roughly 50M tokens per month by month two. Claude Pro subscriptions typically cover 2–3M tokens per PM per month, so the overage lands around $110/month in pay-as-you-go usage, or is covered entirely by a Claude Max subscription.

The useful frame is the value not the cost. By month two, the workspace produces roughly what a junior PM produces in output capacity. Hiring one costs 100–150K annually. The workspace runs on approx $200/month for the team.


### Can I use this workspace if I don't use Claude?
The workspace works across any agentic solution - Claude Code/Cowork, OpenAI Codex, Gemini cli, Github cli and even OpenClaw. Caveat: If you use anything other than Claude or OpenClaw, just ask the agent to update the claude/skill files to match its own taxonomy.

### How do I set this up?
#### Step 1: Install Claude Code or Cowork. 
Download from claude.ai/code. Desktop app for Mac and Windows — no terminal required.

#### Step 2: Get the template. 
If you're comfortable with git: git clone https://github.com/aupsy/Product-Management-Claude-Workspace-template.git my-pm-workspace. Otherwise: go to the GitHub repo, click Code → Download ZIP, unzip, and rename the folder. You can also drop this folder into a shared Google Drive or SharePoint for your team.

#### Step 3: Open the folder in Claude Code or Cowork. 
Claude reads CLAUDE.md automatically on startup. In the desktop app, create a new project using "Use an existing folder." If you feel stuck at any point, just ask Claude what to do next — it will walk you through it.

#### Step 4: Run /hydrate-knowledge. 
Point Claude at your existing product docs — a Confluence URL, a Notion export, a folder of PRDs. It scaffolds the Knowledge/ folder from whatever you already have, so you're not starting blank. Note: You don’t have to worry about these commands or use them as “/hydrate-knowledge”. Just ask Claude how you can setup the workspace initially with your product knowledge, and it will tell you what to do. This also applies for any step in the future where you feel stuck - just ask Claude.

#### Step 5: Fill in CLAUDE.md. 
Add your OKRs, team members, key customers, and working principles. You don't have to edit the file directly — paste the context into a prompt and ask Claude to remember it. This 20-minute step is the highest-value part of the entire setup.
Open [CLAUDE.md](CLAUDE.md) and complete the **HYDRATION CHECKLIST** at the top. The items marked `[AUTO]` may already be filled by Step 1. Complete the rest manually — especially:
- OKR table (targets and current numbers)
- Team name and manager
- Your team's recurring rituals
Delete the checklist block when done.

#### Step 6: Point it at your customer feedback and run /feedback-report. 
If feedback lives in a CRM or Jira, Claude will help set up a connector and prompt you to authenticate. If enterprise permissions block that, download a CSV export and point Claude at the file.


### What goes in the workspace folder vs. what stays in my existing tools?
Put reference context in the workspace: OKRs, product strategy, competitive summaries, working principles, synthesized customer themes. Documents that don't change often and don't have an authoritative live source elsewhere.

Leave live, frequently-updated data in its existing systems: Jira tickets, CRM records, Gong transcripts, engineering specs in Confluence. These have proper access controls and update continuously — connect to them via MCP connectors rather than copying them into the folder.

MCP (Model Context Protocol) is an open standard built into Claude Code that lets the AI connect directly to external tools. Configure a connector once, and Claude can pull a Jira ticket, query Salesforce, or read a GitHub PR as naturally as it reads a local file. A growing directory of pre-built connectors is at modelcontextprotocol.io.


### How do I share the workspace with my team?
GitHub (recommended for git-comfortable teams): Fork the template to a private repo in your company's internal Github. Everyone clones locally, works in Claude Code, and pushes updates. History is tracked, searchable, and survives team rotation.

SharePoint or Google Drive (no git required): Put the workspace folder in a shared drive everyone has mounted locally. Open it in Claude Code from there. Less version control, but it works — and it's the fastest path for teams without git experience.

Either way: one folder, one source of truth, every team member contributing to and drawing from the same knowledge base.


### What's the week-by-week ramp after Week 1?
Week 2: Set up the Monday /feedback-report automation and share output with your team. To automate: use scheduled tasks in Claude Code (ask Claude to set it up), or use the scheduled task feature in the Claude Desktop app and route the output to a Slack channel via Claude's Slack integration. Connect your most-used tools via MCP connectors.

Week 3: Use /write-prd for your next feature spec. Before writing a word, it interviews you against your OKRs and existing customer signals, surfacing tensions you hadn't made explicit. If it's a generative AI feature, combine with /write-eval-criteria to get evaluation rubrics into the PRD — this helps engineering make the right design decisions upfront.

Week 4: Run your metrics review with /weekly-business-review. Once connected to your telemetry, you can also ask Claude ad-hoc questions — "Why did weekly active usage fall 20% last week?" — and get a structured answer rather than a data analyst ti

---

## Tips

- **Add context incrementally** — you don't need everything filled in to get value. Start with CLAUDE.md and one customer folder.
- **Use `/weekly-focus` on Mondays** — it synthesizes your recent emails and project state into a prioritized week plan.
- **Run `/feedback-report` weekly** — if you have a customer feedback source, this turns raw data into themes in seconds.
- **Keep `Projects/` current** — Claude uses it to understand what's in-flight when prioritizing work.
- **Push to git** — this workspace is designed to live in a git repo. Track your knowledge base changes over time.

---

*Built on [Claude Code](https://claude.ai/code). Inspired by the PM OS pattern developed for Microsoft Dynamics 365 Sales.*
