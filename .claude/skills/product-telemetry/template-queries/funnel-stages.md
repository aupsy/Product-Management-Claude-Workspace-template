# Template Query: Funnel Stages

Computes conversion rates across the customer journey funnel: Acquire → Activate → Engage → Retain → Scale.

Adapt the stage definitions to match your product's actual lifecycle milestones.

---

## SQL Version

```sql
-- ============================================================
-- Customer Lifecycle Funnel — [YOUR-PRODUCT-NAME]
-- ============================================================
-- Adapt stage definitions to your product's actual milestones.
-- Each CTE defines one stage; a customer "passes" a stage if
-- they meet the condition within the cohort window.
-- ============================================================

WITH cohort AS (
    -- Stage 0: Acquired — customer provisioned / account created
    SELECT DISTINCT
        [TENANT-ID-COLUMN]      AS tenant_id,
        MIN([TIMESTAMP-COLUMN]) AS first_seen
    FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE]
    WHERE [TIMESTAMP-COLUMN] >= :cohort_start
      AND [TIMESTAMP-COLUMN] < :cohort_end
      AND [PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    GROUP BY 1
),
activated AS (
    -- Stage 1: Activated — first meaningful usage event
    -- e.g., first API call, first workflow run, first lead processed
    SELECT DISTINCT e.[TENANT-ID-COLUMN] AS tenant_id
    FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE] e
    JOIN cohort c ON e.[TENANT-ID-COLUMN] = c.tenant_id
    WHERE e.[TIMESTAMP-COLUMN] >= c.first_seen
      AND e.[ACTIVITY-TYPE-COLUMN] = '[YOUR-ACTIVATION-EVENT]'
),
engaged AS (
    -- Stage 2: Engaged — ≥ N usage events within 30 days of activation
    -- Replace 10 with your "engaged" threshold
    SELECT e.[TENANT-ID-COLUMN] AS tenant_id
    FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE] e
    JOIN activated a ON e.[TENANT-ID-COLUMN] = a.tenant_id
    WHERE e.[PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    GROUP BY 1
    HAVING COUNT(*) >= 10
),
retained AS (
    -- Stage 3: Retained — active in month 2 (30-60 days after first seen)
    SELECT DISTINCT e.[TENANT-ID-COLUMN] AS tenant_id
    FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE] e
    JOIN cohort c ON e.[TENANT-ID-COLUMN] = c.tenant_id
    WHERE e.[TIMESTAMP-COLUMN] >= DATEADD(day, 30, c.first_seen)
      AND e.[TIMESTAMP-COLUMN] < DATEADD(day, 60, c.first_seen)
      AND e.[PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
),
scaled AS (
    -- Stage 4: Scaled — high-volume usage or multi-region/multi-team
    -- Define "scaled" based on your product's expansion signal
    SELECT e.[TENANT-ID-COLUMN] AS tenant_id
    FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE] e
    JOIN cohort c ON e.[TENANT-ID-COLUMN] = c.tenant_id
    WHERE e.[PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    GROUP BY 1
    HAVING COUNT(*) >= 100  -- replace with your scale threshold
)
SELECT
    COUNT(DISTINCT c.tenant_id)   AS acquired,
    COUNT(DISTINCT ac.tenant_id)  AS activated,
    COUNT(DISTINCT en.tenant_id)  AS engaged,
    COUNT(DISTINCT re.tenant_id)  AS retained,
    COUNT(DISTINCT sc.tenant_id)  AS scaled,
    ROUND(COUNT(DISTINCT ac.tenant_id) * 100.0 / NULLIF(COUNT(DISTINCT c.tenant_id), 0), 1) AS acquire_to_activate_pct,
    ROUND(COUNT(DISTINCT en.tenant_id) * 100.0 / NULLIF(COUNT(DISTINCT ac.tenant_id), 0), 1) AS activate_to_engage_pct,
    ROUND(COUNT(DISTINCT re.tenant_id) * 100.0 / NULLIF(COUNT(DISTINCT en.tenant_id), 0), 1) AS engage_to_retain_pct,
    ROUND(COUNT(DISTINCT sc.tenant_id) * 100.0 / NULLIF(COUNT(DISTINCT re.tenant_id), 0), 1) AS retain_to_scale_pct
FROM cohort c
LEFT JOIN activated ac ON c.tenant_id = ac.tenant_id
LEFT JOIN engaged en ON c.tenant_id = en.tenant_id
LEFT JOIN retained re ON c.tenant_id = re.tenant_id
LEFT JOIN scaled sc ON c.tenant_id = sc.tenant_id;
```

---

## KQL Version

```kql
// ============================================================
// Customer Lifecycle Funnel — [YOUR-PRODUCT-NAME]
// ============================================================

declare query_parameters(
    cohortStart: datetime = startofmonth(now(), -3),
    cohortEnd: datetime = now()
);

let cohort = [YOUR-EVENTS-TABLE]
    | where [TIMESTAMP-COLUMN] >= cohortStart and [TIMESTAMP-COLUMN] < cohortEnd
    | where [PRODUCT-FILTER-COLUMN] == "[YOUR-PRODUCT-VALUE]"
    | summarize FirstSeen = min([TIMESTAMP-COLUMN]) by [TENANT-ID-COLUMN];

let activated = [YOUR-EVENTS-TABLE]
    | where [ACTIVITY-TYPE-COLUMN] == "[YOUR-ACTIVATION-EVENT]"
    | join kind=inner cohort on [TENANT-ID-COLUMN]
    | summarize by [TENANT-ID-COLUMN];

let engaged = [YOUR-EVENTS-TABLE]
    | where [PRODUCT-FILTER-COLUMN] == "[YOUR-PRODUCT-VALUE]"
    | join kind=inner cohort on [TENANT-ID-COLUMN]
    | summarize EventCount = count() by [TENANT-ID-COLUMN]
    | where EventCount >= 10
    | summarize by [TENANT-ID-COLUMN];

print
    Acquired  = toscalar(cohort | count),
    Activated = toscalar(activated | count),
    Engaged   = toscalar(engaged | count)
```

---

## Stage Definitions to Customize

| Stage | Generic definition | Replace with your product's milestone |
|-------|-------------------|--------------------------------------|
| Acquire | Account created / provisioned | [e.g., "first lead imported"] |
| Activate | First meaningful usage event | [e.g., "first API call with result"] |
| Engage | ≥10 events in first 30 days | [e.g., "≥5 workflows completed"] |
| Retain | Active in month 2 | [e.g., "active 30-60 days after signup"] |
| Scale | High volume or expansion | [e.g., "≥3 teams using it"] |

---

## Expected Output

| Stage | Count | Conversion |
|-------|-------|-----------|
| Acquired | 208 | — |
| Activated | 175 | 84% |
| Engaged | 120 | 69% |
| Retained | 95 | 79% |
| Scaled | 42 | 44% |
