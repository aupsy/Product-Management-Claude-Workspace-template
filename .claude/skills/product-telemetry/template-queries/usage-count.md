# Template Query: Usage Count

Counts the primary usage events for your product, broken down by week and customer type (external vs internal).

Use this as the basis for the **[YOUR-USAGE-METRIC]** section in your weekly business review.

---

## SQL Version (Snowflake / BigQuery / Redshift / PostgreSQL)

```sql
-- ============================================================
-- Weekly Usage Count — [YOUR-PRODUCT-NAME]
-- ============================================================
-- Replace ALL [PLACEHOLDER] values after schema discovery.
-- Run /product-telemetry setup to auto-generate this file.
-- ============================================================

-- Parameters (set before running):
-- :start_date  e.g. '2026-01-01'
-- :end_date    e.g. '2026-03-31'

SELECT
    DATE_TRUNC('week', [TIMESTAMP-COLUMN])  AS week_start,
    [CUSTOMER-TYPE-COLUMN]                  AS customer_type,  -- e.g. 'External' / 'Internal'
    COUNT(DISTINCT [EVENT-ID-COLUMN])        AS total_events,
    COUNT(DISTINCT [TENANT-ID-COLUMN])       AS unique_customers
FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE]
WHERE
    [TIMESTAMP-COLUMN] >= :start_date
    AND [TIMESTAMP-COLUMN] < :end_date
    AND [PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    AND [IS-TEST-COLUMN] = FALSE              -- exclude test tenants
GROUP BY 1, 2
ORDER BY 1 DESC, 2 ASC;
```

**Placeholders to replace:**

| Placeholder | What to put |
|------------|-------------|
| `[TIMESTAMP-COLUMN]` | e.g., `created_at`, `event_time`, `timestamp` |
| `[CUSTOMER-TYPE-COLUMN]` | Column distinguishing internal/external, or derive with CASE |
| `[EVENT-ID-COLUMN]` | Primary key for a usage event |
| `[TENANT-ID-COLUMN]` | Organization or account ID |
| `[YOUR-SCHEMA].[YOUR-EVENTS-TABLE]` | e.g., `PROD_DB.ANALYTICS.EVENTS` |
| `[PRODUCT-FILTER-COLUMN]` | Column to filter for your product |
| `[YOUR-PRODUCT-VALUE]` | e.g., `'my_product'`, `'sales_agent'` |
| `[IS-TEST-COLUMN]` | Column indicating test/internal accounts; remove if not applicable |

---

## KQL Version (Azure Data Explorer / Kusto)

```kql
// ============================================================
// Weekly Usage Count — [YOUR-PRODUCT-NAME]
// ============================================================

declare query_parameters(
    startDate: datetime = datetime(2026-01-01),
    endDate: datetime = now()
);

[YOUR-EVENTS-TABLE]
| where [TIMESTAMP-COLUMN] >= startDate and [TIMESTAMP-COLUMN] < endDate
| where [PRODUCT-FILTER-COLUMN] == "[YOUR-PRODUCT-VALUE]"
| where [IS-TEST-FLAG] == false
| extend Week = startofweek([TIMESTAMP-COLUMN])
| extend CustomerType = iff([IS-INTERNAL-FLAG], "Internal", "External")
| summarize
    TotalEvents = count(),
    UniqueCustomers = dcount([TENANT-ID-COLUMN])
    by Week, CustomerType
| order by Week desc, CustomerType asc
```

---

## Expected Output

| week_start | customer_type | total_events | unique_customers |
|-----------|--------------|-------------|-----------------|
| 2026-03-03 | External | 15,420 | 48 |
| 2026-03-03 | Internal | 2,100 | 12 |
| 2026-02-24 | External | 14,200 | 45 |
| 2026-02-24 | Internal | 1,900 | 10 |

---

## Calibration

After filling in placeholders, run `/calibrate-metric usage-count` to validate against your BI dashboard.
