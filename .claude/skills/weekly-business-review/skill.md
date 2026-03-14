---
name: weekly-business-review
description: Generate the weekly business review report for [YOUR-PRODUCT-NAME], combining telemetry data with customer and funnel analysis
user-invocable: true
argument-hint: [YYYY-MM-DD week ending date] (optional, defaults to current week)
allowed-tools: mcp__azure__kusto, Read, Glob, Write, Bash
---

# Weekly Business Review — [YOUR-PRODUCT-NAME]

Generates a comprehensive weekly business review report saved to `Analytics/Weekly Reports/WBR-[DATE].md`.

**User's request:** $ARGUMENTS

<!-- ============================================================
  SETUP REQUIRED
  Before this skill works, complete these steps:
  1. Run /product-telemetry setup to connect your data source
  2. Calibrate your key metrics with /calibrate-metric
  3. Update the DATA SOURCES section below with your actual
     cluster URIs, databases, and query file paths
  4. Update the REPORT SECTIONS to match your product's metrics
  ============================================================ -->

---

## Prerequisites Check

Before running, verify:

1. **Telemetry connected**: `.claude/skills/product-telemetry/` has calibrated query files (no `[PLACEHOLDER]` remaining)
2. **CLAUDE.md hydrated**: OKRs have real targets and current values
3. **Data access**: You can reach your telemetry cluster (VPN, auth, etc.)

If any are missing, stop and run `/product-telemetry setup` first.

---

## Step 1: Set Date Parameters

```
WEEK_END_DATE = provided date OR current date
WEEK_START_DATE = 6 weeks before WEEK_END_DATE
CURRENT_MONTH_START = first of current month
```

---

## Step 1b: Pull Conversational Context (Optional)

Read `.claude/skills/productivity-tools.md` to determine which tool is active.

This step enriches the Named Customer section and Key Actions with context from recent communications — e.g., a customer escalation raised on a call, an open action item from a meeting, or a thread discussing a metric anomaly.

### If Microsoft 365 / WorkIQ:
Call `mcp__workiq__ask_work_iq`:
> "What are the most important customer conversations, escalations, or product discussions from the past week? Focus on: (1) any customer issues or at-risk signals, (2) open action items from customer calls or internal meetings, (3) any anomalies in product metrics or usage that were discussed."

### If Slack:
Search channels from `productivity-tools.md` for the past 7 days:
- Customer mentions with issues or escalations
- Threads referencing metric changes or incidents
- Open action items in team channels

### If Google Workspace:
Query recent Gmail / Calendar for past 7 days:
- Customer call follow-up emails
- Internal discussions about usage changes or product issues

Store result as `$COMMS_CONTEXT`. Use it in Step 4 (Named Customer Check) and Step 5 (Key Actions).

If no tool is configured, skip this step.

---

## Step 2: Query Usage Metrics

<!-- Replace [YOUR-USAGE-METRIC] with your product's primary usage signal,
     e.g., "API calls", "active sessions", "leads processed", "messages sent" -->

Run queries for **[YOUR-USAGE-METRIC]** from your telemetry stack.

See calibrated queries in `.claude/skills/product-telemetry/`.

**Query A: Usage by week (past 6 weeks)**
- Run query: `product-telemetry/[YOUR-USAGE-QUERY].sql` (or `.kql`)
- Break down by: customer type (external vs internal), region if applicable

**Query B: Usage by customer/org (current month)**
- Run query: `product-telemetry/[YOUR-CUSTOMER-USAGE-QUERY].sql`
- Get: top 10 customers by usage, new vs returning

**Query C: Customer/tenant count by month**
- Run query: `product-telemetry/[YOUR-CUSTOMER-COUNT-QUERY].sql`
- Get: monthly active customer count for last 5 months

---

## Step 3: Query User Engagement Metrics

<!-- Replace [YOUR-USER-METRIC] with your product's user engagement signal,
     e.g., "MAU", "DAU", "active sellers", "weekly active users" -->

Run queries for **[YOUR-USER-METRIC]**:

**Query D: MAU/DAU trend (past 6 weeks)**
- Run query: `product-telemetry/[YOUR-USER-ENGAGEMENT-QUERY].sql`

**Query E: User funnel (if applicable)**
- Run query: `product-telemetry/[YOUR-FUNNEL-QUERY].sql`
- Stages: Acquire → Activate → Engage → Retain → Scale

---

## Step 4: Named Customer Check

For each named customer in `Customers/`:
- Pull their usage for the past 4 weeks from Query B
- Flag if: (a) usage dropped >50% WoW, (b) no activity in 2+ weeks, (c) new deployment

Summarize as: Active / Ramping / At Risk / Churned / Not Yet Deployed

---

## Step 5: Generate the Report

Create `Analytics/Weekly Reports/WBR-[DATE].md` with this structure:

```markdown
# Weekly Business Review — [YOUR-PRODUCT-NAME]
**Week Ending:** [DATE]
**Generated:** [TIMESTAMP]

---

## Executive Summary

[3–5 sentence narrative: what went up, what went down, what to watch]

**Key Highlights:**
- [YOUR-USAGE-METRIC]: [X] (↑/↓ [Y]% vs last week)
- Active customers: [X] (↑/↓ [Z] vs last month)
- [YOUR-USER-METRIC]: [X] (↑/↓ [Y] vs last month)

**Progress to OKRs:**
| OKR | Target | Current | Status |
|-----|--------|---------|--------|
| [YOUR-KR-1] | [TARGET] | [CURRENT] | 🟢/🟡/🔴 |
| [YOUR-KR-2] | [TARGET] | [CURRENT] | 🟢/🟡/🔴 |

---

## 1. [YOUR-USAGE-METRIC] Trends

### 1.1 Weekly Trend (Past 6 Weeks)

| Week | External | Internal | Total | WoW |
|------|----------|----------|-------|-----|
| [data] | | | | |

### 1.2 Top Customers (Current Month)

| Rank | Customer | Usage | MoM Change |
|------|----------|-------|------------|
| [data] | | | |

### 1.3 Insights
[Auto-generated analysis: what's driving changes, new/churned customers]

---

## 2. User Engagement

### 2.1 [YOUR-USER-METRIC] Trend

| Month | External | Internal | Total | MoM |
|-------|----------|----------|-------|-----|
| [data] | | | | |

### 2.2 User Funnel (if applicable)

| Stage | Count | Conversion |
|-------|-------|------------|
| Acquire | [X] | — |
| Activate | [X] | [Y]% |
| Engage | [X] | [Y]% |
| Retain | [X] | [Y]% |

### 2.3 Insights
[Analysis of engagement trends]

---

## 3. Named Customer Health

| Customer | Status | Last Week Usage | Trend | Notes |
|----------|--------|----------------|-------|-------|
| [data from Customers/ folder] | | | | |

---

## 4. Key Actions This Week

Based on this data:
1. [Auto-generated action from insights]
2. [Auto-generated action from insights]
3. [Auto-generated action from insights]

---

## Appendix: Data Sources

- **Usage data**: [YOUR-CLUSTER/DATABASE]
- **User data**: [YOUR-CLUSTER/DATABASE]
- **Query timestamp**: [TIMESTAMP]
```

---

## Error Handling

**If telemetry queries fail:**
```
Telemetry queries could not run. Possible causes:
- Not connected to VPN / missing auth
- Query files still have [PLACEHOLDER] values (run /product-telemetry setup)
- Schema changed since last calibration (run /calibrate-metric [metric])
```

**If CLAUDE.md OKRs are not filled:**
```
OKR targets not found in CLAUDE.md. Please complete the HYDRATION CHECKLIST first.
```

---

## Configuration Notes

<!-- Fill these in after running /product-telemetry setup -->

| Setting | Value |
|---------|-------|
| Usage metric name | [YOUR-USAGE-METRIC] |
| User metric name | [YOUR-USER-METRIC] |
| Primary telemetry stack | [Snowflake / BigQuery / Kusto / etc.] |
| Usage query file | `product-telemetry/[FILENAME]` |
| User query file | `product-telemetry/[FILENAME]` |
| Funnel stages | Acquire → Activate → Engage → Retain → Scale |
