# CLAUDE.md — [YOUR-PRODUCT-NAME] PM Workspace

<!-- ============================================================
  HYDRATION CHECKLIST
  Complete every item below before using this workspace.
  Items marked [AUTO] may be pre-filled by /hydrate-knowledge.
  ============================================================

  [ ] [YOUR-PRODUCT-NAME]     — Full product name (e.g., "Payments Gateway")
  [ ] [YOUR-PRODUCT-SHORT]    — Short name or acronym (e.g., "PG")
  [ ] [YOUR-TEAM-NAME]        — Your team name (e.g., "Checkout Platform")
  [ ] [YOUR-MANAGER-NAME]     — Your manager's name
  [ ] [YOUR-COMPANY]          — Company and org context (e.g., "Stripe / Revenue Products")
  [ ] [YOUR-PRODUCT-DESC]     — 1–2 sentence description of what the product does [AUTO]
  [ ] [YOUR-USER-TYPE]        — Who uses it (e.g., "developers", "sales reps", "merchants")
  [ ] OKR table               — Replace all [YOUR-KR-*] rows with real OKRs + targets
  [ ] [YOUR-PRIORITY-LENS]    — The top-of-mind strategic question (e.g., "Does this get us to 1M users?")
  [ ] [YOUR-RITUAL-1/2/3]     — Your team's recurring rituals (standups, reviews, etc.)
  [ ] Working principles      — Keep as-is or replace with your team's principles
  [ ] Repo structure          — Update the folder tree if your structure differs

  Once complete, delete this checklist block.
============================================================ -->

This file provides guidance to Claude Code for the [YOUR-PRODUCT-NAME] PM workspace. It defines product context, team norms, and working principles for consistent, high-quality AI assistance.

## Product Context

This repository contains shared knowledge for **[YOUR-PRODUCT-NAME]** ([YOUR-PRODUCT-SHORT]).

<!-- [AUTO] Replace the description below with what /hydrate-knowledge finds, or write it yourself. -->
**Product**: [YOUR-PRODUCT-DESC]

**Team**: [YOUR-TEAM-NAME], reporting to [YOUR-MANAGER-NAME]

**Company / Org**: [YOUR-COMPANY]

**Users**: [YOUR-USER-TYPE]

## Repository Structure

```
/
├── CLAUDE.md              # AI workspace instructions (this file)
├── Analytics/             # Weekly business reviews, exec decks
│   ├── Weekly Reports/
│   └── Exec reviews/
├── Customers/             # One folder per key customer or segment
│   └── _SAMPLE-CUSTOMER/  # Copy and rename for each customer
├── Knowledge/
│   ├── compete/           # Competitive landscape and research
│   ├── metrics/           # Success metrics, telemetry, dashboards
│   ├── org/               # Team context, org updates
│   ├── product/           # Product docs, specs, research
│   ├── strategy/          # Roadmap proposals, strategy documents
│   └── user-research/     # User research and personas
├── Projects/              # Active initiative workstreams
│   └── _TEMPLATE-INITIATIVE/
└── Team/                  # Weekly focus docs, team rituals
    └── Weekly Focus Areas/
```

## Working Principles

<!-- Keep these as-is or replace with your team's principles. They are intentionally generic. -->

- **"How would the customer know?"** — Always consider the user experience; if a change is invisible to the user, question its priority
- **"Think 10x, ship 1x"** — Bold vision, incremental delivery
- **Ship to learn, not to be perfect** — Bias toward action over analysis paralysis
- **3-bullet rule** — If you can't say it in 3 bullets, you don't understand it well enough
- **Speed-to-impact > comprehensiveness** — A good answer now beats a perfect answer next week

## Team OKRs

<!-- Replace the rows below with your actual OKRs.
     Format: Objective → Key Result → Target → Current status
     Add/remove rows as needed. -->

### [YOUR-OBJECTIVE-1] (e.g., Drive Adoption)
| Key Result | Target | Current | Due |
|-----------|--------|---------|-----|
| [YOUR-KR-1] (e.g., Monthly Active Users) | [TARGET] | [CURRENT] | [DATE] |
| [YOUR-KR-2] (e.g., Leads processed) | [TARGET] | [CURRENT] | [DATE] |
| [YOUR-KR-3] (e.g., Revenue / usage metric) | [TARGET] | [CURRENT] | [DATE] |

### [YOUR-OBJECTIVE-2] (e.g., Improve Quality)
| Key Result | Target | Current | Due |
|-----------|--------|---------|-----|
| [YOUR-KR-4] | [TARGET] | [CURRENT] | [DATE] |
| [YOUR-KR-5] | [TARGET] | [CURRENT] | [DATE] |

### [YOUR-OBJECTIVE-3] (e.g., Grow Market)
| Key Result | Target | Current | Due |
|-----------|--------|---------|-----|
| [YOUR-KR-6] | [TARGET] | [CURRENT] | [DATE] |

*See `Knowledge/metrics/SMT-framework-template.md` for the full metrics framework*

## Priority Framework

**Strategic lens**: [YOUR-PRIORITY-LENS]
<!-- Example: "Does this get us to 1M MAU?" or "Does this unblock the enterprise tier?" -->

When asked to prioritize or suggest tasks:

- **P0**: Directly contributes to team OKRs, customer escalations, time-sensitive commitments
- **P1**: Advances 90-day deliverables, high-value customer or user learning
- **P2**: Supports broader team objectives, relationship building, long-term bets
- **P3**: Administrative tasks, speculative projects, nice-to-haves

**Decision filter**: Does this ladder up to team OKRs? Does it advance 90-day deliverables? Will it help learn faster or ship faster?

## Document Naming Convention

Strategic documents use YYYYMMDD timestamps: `document-name-20260120.md`

## Recurring Commitments

<!-- Replace with your team's actual rituals. Keep the format — it helps Claude understand cadence. -->

### [YOUR-RITUAL-1] (e.g., Weekly Business Review — every Monday)
**What**: [Brief description of what happens and why it matters]
**Prep needed**: [What Claude should help you prepare]

### [YOUR-RITUAL-2] (e.g., Monthly Feature Pitch — 4th Wednesday)
**What**: [Brief description]
**A strong pitch covers**: problem (with signal) → proposal (scoped) → why now → expected impact

### [YOUR-RITUAL-3] (e.g., Quarterly Exec Review)
**What**: [Brief description]
**Prep needed**: [What to prepare]

## Key Documents

<!-- Update these paths after hydrating your Knowledge/ folder. -->

- **`Knowledge/metrics/SMT-framework-template.md`** — Success metrics framework (Strategy → Metrics → Tactics)
- **`Knowledge/org/team-context-template.md`** — Team structure and organizational context
- **`Analytics/Weekly Business Review - Instructions.md`** — Weekly review question framework
