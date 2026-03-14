# Weekly Business Review — Instructions

<!-- ============================================================
  SETUP: Replace [YOUR-PRODUCT-NAME], [YOUR-USAGE-METRIC],
  [YOUR-USER-METRIC], and funnel stage names with your product's
  actual metrics and milestones.
  ============================================================ -->

## General Instructions

- Always start with an executive summary (3–5 sentences max)
- Include trend data to show direction, not just point-in-time numbers
- Track progress toward OKRs (from `CLAUDE.md`)
- Highlight **insights**, not just metrics — "What's changing? Why? What should we do?"
- Always include the Customer Lifecycle Cohort Funnel (Section 4) — it's mandatory every week
- Use ✅ / ⚠️ / 🔴 to signal health at a glance

---

## Section 1: [YOUR-USAGE-METRIC] Trends

*Replace "[YOUR-USAGE-METRIC]" with your product's primary usage signal — e.g., "API calls", "leads processed", "messages sent", "transactions".*

### 1.1 Usage Trends

**Data source:** `[YOUR-TELEMETRY-CLUSTER]` / `[YOUR-DB]`
**Query:** `product-telemetry/usage-count.[sql|kql]`

**Questions to answer:**
- What is the current and last 4 months [YOUR-USAGE-METRIC]?
- How much is External customers vs Internal (test/employee)?
- What are the weekly trends for the past 6 weeks?
- Is usage trending up or down? By how much (% change)?

### 1.2 Customer Adoption & Retention

**Questions to answer:**
- How many customers are active in the current month vs last 4 months?
- Are we gaining or losing customers (net new vs churn)?
- Who are the top 10 customers by [YOUR-USAGE-METRIC]?
- What's the retention rate (% of last month's customers still active)?

**Key OKR metric:** [YOUR-KR-1 from CLAUDE.md] → Track here

### 1.3 Named Customer Usage

For each customer in `Customers/`:
- What is their usage for the past 4 weeks?
- Are they growing, flat, or declining?
- Any customers with no activity for 2+ weeks? (Churn risk)

**Questions to answer:**
- Which named customers are active / ramping / at risk / churned?
- Any surprises (unexpected spikes or drops)?

### 1.4 Key Insights

- Is usage up or down overall? By how much?
- Which customers are driving the change?
- Concentration vs distribution (few big customers vs many small ones)?
- Any regional patterns?

---

## Section 2: User Engagement

*Replace "[YOUR-USER-METRIC]" with your user engagement metric — e.g., "MAU", "DAU", "weekly active users", "active sellers".*

### 2.1 [YOUR-USER-METRIC] Metrics

**Data source:** `product-telemetry/user-engagement.[sql|kql]`

**Questions to answer:**
- What is the current and last 4 months [YOUR-USER-METRIC]?
- What is the MAU/DAU ratio (stickiness indicator)?
- External vs Internal breakdown?
- Weekly trends for the past 6 weeks?

**Key OKR metric:** [YOUR-USER-OKR from CLAUDE.md] → Track here

### 2.2 Usage Quality

**Questions to answer:**
- Are users engaging deeply with the product's core value? (e.g., not just logging in, but completing key actions)
- What is the completion rate for the key workflow?
- Are errors or failures increasing or decreasing?

**Key OKR metric:** [YOUR-QUALITY-OKR] → Track here (e.g., error rate, failure rate)

### 2.3 Key Insights

- Is user engagement growing independently of customer count growth?
- Are existing users doing more, or are we just adding new users?
- Any cohort of users with declining engagement to investigate?

---

## Section 3: Customer Activation & Time-to-Value

### 3.1 Activation Funnel (New Customers)

**Data source:** `product-telemetry/funnel-stages.[sql|kql]`

Track new customers through your product's activation milestones:

| Stage | Definition | Metric |
|-------|-----------|--------|
| **Acquire** | Account created / signed up | New account count |
| **Activate** | First meaningful usage event | % of new accounts who activate |
| **Engage** | Regular usage pattern established | % of activated who engage |
| **Retain** | Active in month 2 | % still active 30–60 days later |
| **Scale** | Expansion: more users, more volume, more features | % growing MoM |

*Replace these stage definitions with your product's actual milestones.*

**Questions to answer:**
- What % of new customers complete each stage?
- Where is the biggest drop-off?
- Are newer customers converting better than older ones?

### 3.2 Time-to-Value

**Questions to answer:**
- How long does it take customers to reach their first value moment?
- P50 and P90 durations for each key transition
- Is time-to-value improving over time?

### 3.3 Key Insights

- Where is the funnel breaking? (Where do customers get stuck?)
- Is improving activation a bigger lever than acquiring more customers?
- What would it take to move the "Activate%" metric by 10 points?

---

## Section 4: Customer Lifecycle Cohort Funnel (MANDATORY — Include Every Week)

**This section is required in every WBR.** It tracks each monthly cohort through the full lifecycle and is the primary lens for whether product improvements are compounding across customer generations.

### Funnel Stage Definitions

| Stage | Generic Definition | Your Definition |
|-------|-------------------|----------------|
| Acquire | Account created | [Your milestone] |
| Activate | First meaningful value delivered | [Your milestone] |
| Engage | Habitual usage established | [Your milestone] |
| Retain | Active 30–60 days post-acquisition | [Your milestone] |
| Scale | Growing usage, expanding footprint | [Your milestone] |

### Required Table

Show each cohort (monthly) against the funnel:

```
| Cohort     | Acquired | Activate% | Engage% | Retain% | Scale% |
|-----------|---------|----------|--------|--------|--------|
| [Month 1] | [N]     | [X%]     | [X%]   | [X%]   | [X%]   |
| [Month 2] | [N]     | [X%]     | [X%]   | [X%]   | [X%]   |
| [Month 3] | [N]     | [X%]     | [X%]   | [X%]   | [X%]   |
```

Also include:
- Conversion waterfall across all cohorts combined
- Trend: are recent cohorts converting better than older ones?
- Identify data gaps (stages not yet measured)

### Key Questions

- Is Activate% improving? (This is usually the #1 lever)
- Are recent cohorts retaining better? (Are product improvements sticking?)
- Is top-of-funnel (Acquire) growing or shrinking?
- Which stage has the biggest drop-off — and what's the hypothesis for why?

---

## Section 5: Customer Deep Dives

### 5.1 Individual Customer Journey

Use when:
- Preparing for a customer meeting
- Investigating why a customer churned
- Understanding a power user's success pattern

**Questions to answer:**
- When did this customer first use the product?
- What were their key activation events and when?
- Have they deactivated / churned? When and why (if known)?
- How does their usage trend look week-over-week?

### 5.2 Customer Health Summary

For key customers (top 10 by usage + named customers in `Customers/`):

| Customer | Status | Usage Trend | Last Active | Notes |
|----------|--------|------------|-------------|-------|
| [Customer] | Active / At Risk / Churned | ↑/→/↓ | [Date] | [Context] |

---

## Insight Formatting Guide

For each section, use this format for insights:

- ✅ **Positive signal**: "[Metric] up [X]% driven by [cause]"
- ⚠️ **Watch closely**: "[Metric] flat — [hypothesis] — monitoring [indicator]"
- 🔴 **Action needed**: "[Metric] dropped [X]% — [root cause] — owner: [person], action by [date]"

---

## Available Queries

After running `/product-telemetry setup`, your calibrated queries will be listed here:

| Query | File | Status |
|-------|------|--------|
| Usage count (weekly) | `product-telemetry/usage-count.[sql\|kql]` | [Calibrated / In Progress] |
| User engagement (MAU/DAU) | `product-telemetry/user-engagement.[sql\|kql]` | [Calibrated / In Progress] |
| Funnel stages | `product-telemetry/funnel-stages.[sql\|kql]` | [Calibrated / In Progress] |
| Customer count | *(add after calibration)* | Not Started |
| Individual customer journey | *(add after calibration)* | Not Started |
