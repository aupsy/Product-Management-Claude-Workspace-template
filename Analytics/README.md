# Analytics

This folder contains your weekly business reviews, exec briefings, and the instructions that guide how Claude generates them.

## Contents

| Item | What it is |
|------|-----------|
| `Weekly Business Review - Instructions.md` | The question framework that drives `/weekly-business-review` |
| `Weekly Reports/` | Auto-generated WBR reports (output of `/weekly-business-review`) |
| `Exec reviews/` | Exec-facing briefings and quarterly reviews |

## Two-Track Analytics

The WBR covers two types of data:

1. **Usage data** — How much is your product being used? (events, transactions, API calls)
2. **User engagement** — Who is using it and how deeply? (MAU, DAU, funnel stages)

These come from different queries but are combined into a single weekly narrative.

## Getting Started

1. Complete `/product-telemetry setup` to connect your data source
2. Update `Weekly Business Review - Instructions.md` with your product's specific metric names
3. Run `/weekly-business-review` — reports land in `Weekly Reports/`

## Cadence

- **Weekly**: `/weekly-business-review` every Monday morning
- **Monthly**: Update exec review in `Exec reviews/` before your monthly stakeholder meeting
- **Quarterly**: Full business review deck for leadership
