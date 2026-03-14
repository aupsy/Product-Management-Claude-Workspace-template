# Weekly Business Review Skill — Setup Guide

This skill generates your weekly business review report. It needs to be connected to your telemetry before it can run.

## Setup Steps

1. **Run `/product-telemetry setup`** — this identifies your stack and creates calibrated query files
2. **Update `skill.md`** — replace the `[YOUR-USAGE-METRIC]`, `[YOUR-USER-METRIC]`, and `[YOUR-*-QUERY]` placeholders with your actual metric names and query filenames
3. **Update the Configuration Notes table** at the bottom of `skill.md` with your actual settings
4. **Test**: Run `/weekly-business-review` and verify the output looks correct

## What to Configure

| Placeholder | What to put there |
|-------------|------------------|
| `[YOUR-USAGE-METRIC]` | Your primary usage signal (e.g., "API calls", "leads processed", "messages sent") |
| `[YOUR-USER-METRIC]` | Your user engagement metric (e.g., "MAU", "DAU", "weekly active users") |
| `[YOUR-USAGE-QUERY]` | Filename of the calibrated usage query (created by `/product-telemetry`) |
| `[YOUR-CUSTOMER-USAGE-QUERY]` | Filename of the per-customer usage query |
| `[YOUR-USER-ENGAGEMENT-QUERY]` | Filename of the user engagement query |
| `[YOUR-FUNNEL-QUERY]` | Filename of the funnel stages query (optional) |

## Output Location

Reports are saved to: `Analytics/Weekly Reports/WBR-YYYY-MM-DD.md`

## Scheduling

Run this every Monday morning as part of your weekly ritual. Combine with `/weekly-focus` for a full start-of-week briefing.
