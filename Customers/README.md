# Customers

One folder per key customer or customer segment. Claude reads these folders to give context-aware answers about customer health, prepare you for meetings, and surface at-risk accounts in the weekly business review.

## Naming Convention

```
Customers/
├── Acme-Corp/
├── Globex-Enterprise/
├── _SAMPLE-CUSTOMER/   ← Copy this for each new customer
```

Use the company name (or a recognizable short name). Avoid spaces — use hyphens.

## Getting Started

For each key customer:
1. Copy `_SAMPLE-CUSTOMER/` and rename it
2. Fill in `customer-health-summary.md` with what you know
3. Add meeting notes after every engagement call
4. Update `pilot-tracker.md` when they're in an active pilot or onboarding

## File Types

| File | When to create |
|------|---------------|
| `customer-health-summary.md` | When you start tracking a customer |
| `engagement-notes-YYYYMMDD.md` | After every call, demo, or significant email thread |
| `pilot-tracker-YYYYMMDD.md` | When a customer is in active pilot or onboarding |

## How Claude Uses These Files

- **`/weekly-business-review`** — scans for recent engagement notes and health summaries to include in the named customer section
- **`/weekly-focus`** — surfaces customers with upcoming meetings or open escalations as P0/P1 items
- **`/write-prd`** — reads customer pain points and use cases to inform feature descriptions
- **Natural queries** — "What's the status with Acme?" reads their folder and summarizes

## Tips

- Keep `customer-health-summary.md` updated — it's Claude's primary reference for customer context
- Even a brief 3-bullet note after a call is more valuable than nothing
- Use the naming convention `engagement-notes-YYYYMMDD.md` so files sort chronologically
