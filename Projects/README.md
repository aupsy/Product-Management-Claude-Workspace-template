# Projects

One folder per active initiative or workstream. Claude reads these to understand what's in-flight when helping you prioritize, prepare for reviews, or write PRDs.

## Naming Convention

```
Projects/
├── _TEMPLATE-INITIATIVE/     ← Copy this for each new initiative
├── mobile-onboarding-revamp/
├── api-rate-limit-redesign/
└── enterprise-sso/
```

Use kebab-case names that are descriptive but concise.

## Initiative Lifecycle

| Stage | What to do |
|-------|-----------|
| **Scoping** | Create folder, fill in `INITIATIVE-OVERVIEW.md` with problem + hypothesis |
| **Active** | Update status weekly; add sub-documents as needed (PRD, research, decisions) |
| **Shipped** | Mark status as "Shipped" in frontmatter; add retrospective note |
| **Paused / Cancelled** | Mark status; add a note explaining why |

## What Goes in Each Folder

| File | When to create |
|------|---------------|
| `INITIATIVE-OVERVIEW.md` | Start of every initiative |
| `PRD-[name]-YYYYMMDD.md` | When writing a PRD (use `/write-prd`) |
| `decisions/` | Sub-folder for decision logs |
| `research/` | Sub-folder for any research or analysis |

## Tips

- Keep `INITIATIVE-OVERVIEW.md` current — Claude uses it to answer "what are we working on?" and to prioritize in `/weekly-focus`
- Even a one-line status update is valuable: "Status: Blocked on eng capacity" tells Claude something important
- Don't delete completed initiatives — they're valuable historical context
