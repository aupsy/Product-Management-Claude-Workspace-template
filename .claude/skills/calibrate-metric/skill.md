---
name: calibrate-metric
description: Calibrate a dashboard metric — guide ground truth capture, create/validate queries, and track convergence
user-invocable: true
argument-hint: [metric name or "list"]
allowed-tools: mcp__azure__kusto, Read, Write, Edit, Glob, Grep, Bash
---

# Calibrate Metric

Validate a telemetry query against your ground truth (e.g., a BI dashboard) so `/weekly-business-review` can serve it with confidence.

**User's request:** $ARGUMENTS

<!-- ============================================================
  SETUP REQUIRED
  After running /product-telemetry setup:
  1. Replace [YOUR-BI-TOOL] with your dashboard tool (e.g., Tableau, Looker, Power BI)
  2. Replace [YOUR-CLUSTER-URI] and [YOUR-DATABASE] with your telemetry connection
  3. Add your metrics to the Metric Catalog below as you calibrate them
  ============================================================ -->

## Goal

Every metric served by `/weekly-business-review` should be validated against your BI dashboard. This skill makes calibration a guided, repeatable process:

1. Identify the metric to calibrate
2. Guide you through capturing ground truth from your BI tool
3. Create or update the query file
4. Run validation and compare results
5. Iterate until convergence (<1% delta)
6. Branch, commit, and offer a PR

---

## Metric Catalog

| ID | Metric | BI Tool Visual | Query File | Status |
|----|--------|----------------|------------|--------|
| *(add rows as you calibrate)* | | | | |

**Status key:**
- **Calibrated** — query file exists and ground truth rows all PASS (<1% delta)
- **In Progress** — calibration started but not converged
- **Not Started** — metric identified but no work done

---

## Step-by-Step Flow

### Step 0: Create a feature branch

```bash
git checkout -b calibrate-[metric-id]-[YYYYMMDD]
```

### Step 1: Identify the metric

Parse `$ARGUMENTS` against the catalog:
- `list` → show catalog with live status
- Metric name → look it up; if not found, add it as "In Progress"
- No args → show catalog and ask which metric

### Step 2: Show what we know

Display current status, whether a query file exists, and any prior ground truth rows.

### Step 3: Guide ground truth capture

Tell the user exactly where to find the metric in **[YOUR-BI-TOOL]**:

> 1. Open **[YOUR-BI-TOOL]** and navigate to the **[dashboard name]** dashboard
> 2. Find the **[metric name]** visual
> 3. Set the date filter to **[range 1]** — note the value
> 4. Set the date filter to **[range 2]** — note the value
> 5. Set the date filter to **[range 3]** — note the value
>
> Provide values like: `Feb 10-14: 8602 / Feb 1-14: 10605 / Feb 1-26: 46390`

### Step 4: Record ground truth

Append to `_temp/dashboard-ground-truth.csv`:

```csv
metric_id,metric_name,expected_value,start_date,end_date,filters,query_result,delta_pct,status,captured_date
```

Set `query_result`, `delta_pct` to empty and `status` to `PENDING`.

### Step 5: Create or update the query file

Create a parameterized query file in `.claude/skills/product-telemetry/`:
- Header with: source, metric description, iteration log, ground truth table
- Parameters: `startDate`, `endDate`, and any relevant filters
- Language: SQL or KQL depending on your stack (see `/product-telemetry`)

### Step 6: Run validation

Execute the query for each ground truth row. Compare result to `expected_value`:

```
delta_pct = (query_result - expected_value) / expected_value * 100
PASS if abs(delta_pct) < 1%, FAIL otherwise
```

Show a convergence table:
```
| Range    | Expected | Query Result | Delta % | Status |
|----------|----------|-------------|---------|--------|
| Feb 10-14 | 8,602   | 8,571       | -0.36%  | PASS   |
```

Update the CSV with results.

### Step 7: Iterate or converge

**If ALL deltas < 1% (CONVERGED):**
1. Update query file header with ground truth table
2. Update the Metric Catalog in this file: set status to "Calibrated"
3. Register the query in `/weekly-business-review` skill

**If ANY delta >= 1%:**
1. Analyze which part of the query is contributing to the delta
2. Suggest likely causes (filter mismatch, aggregation difference, data freshness)
3. Propose a fix and re-run from Step 6

**Convergence criterion:** `abs(query - BI) / BI < 1%` for ALL ground truth rows.

*Note: approximate aggregations (HyperLogLog, COUNT DISTINCT on large sets) may have 0.5–1% natural variance.*

### Step 8: Offer to push and PR

Once calibration is complete, offer to stage, commit, push, and create a PR.

---

## Ground Truth File

Location: `_temp/dashboard-ground-truth.csv`

This file is the regression test set for all calibrated metrics. Re-run periodically to catch query breakage from schema changes.

---

## Tips

- **BI is the oracle** — if your query disagrees with the dashboard, the query is wrong until proven otherwise
- **Start with unfiltered ranges** — easier to debug, add filtered ground truth after the base converges
- **Data freshness matters** — BI dashboards often use import snapshots, live queries may differ slightly for recent data
