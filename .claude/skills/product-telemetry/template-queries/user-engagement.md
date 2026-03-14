# Template Query: User Engagement (MAU / DAU)

Computes Monthly Active Users (MAU) and Daily Active Users (DAU) for your product.

---

## SQL Version

```sql
-- ============================================================
-- Monthly Active Users (MAU) — [YOUR-PRODUCT-NAME]
-- ============================================================

SELECT
    DATE_TRUNC('month', [TIMESTAMP-COLUMN])  AS month_start,
    [CUSTOMER-TYPE-COLUMN]                    AS customer_type,
    COUNT(DISTINCT [USER-ID-COLUMN])           AS mau,
    COUNT(DISTINCT [TENANT-ID-COLUMN])         AS active_orgs
FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE]
WHERE
    [TIMESTAMP-COLUMN] >= :start_date
    AND [TIMESTAMP-COLUMN] < :end_date
    AND [PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    AND [IS-TEST-COLUMN] = FALSE
GROUP BY 1, 2
ORDER BY 1 DESC, 2 ASC;
```

```sql
-- ============================================================
-- Daily Active Users (DAU) — [YOUR-PRODUCT-NAME]
-- ============================================================

SELECT
    DATE_TRUNC('day', [TIMESTAMP-COLUMN])  AS day,
    COUNT(DISTINCT [USER-ID-COLUMN])        AS dau
FROM [YOUR-SCHEMA].[YOUR-EVENTS-TABLE]
WHERE
    [TIMESTAMP-COLUMN] >= :start_date
    AND [TIMESTAMP-COLUMN] < :end_date
    AND [PRODUCT-FILTER-COLUMN] = '[YOUR-PRODUCT-VALUE]'
    AND [IS-TEST-COLUMN] = FALSE
GROUP BY 1
ORDER BY 1 DESC;
```

**Placeholders to replace:**

| Placeholder | What to put |
|------------|-------------|
| `[USER-ID-COLUMN]` | The column identifying a unique user (e.g., `user_id`, `aad_user_id`) |
| `[TENANT-ID-COLUMN]` | Organization/account ID |
| `[TIMESTAMP-COLUMN]` | Event timestamp column |
| `[CUSTOMER-TYPE-COLUMN]` | Internal vs external classification |

---

## KQL Version

```kql
// ============================================================
// Monthly Active Users — [YOUR-PRODUCT-NAME]
// ============================================================

declare query_parameters(
    startDate: datetime = ago(150d),
    endDate: datetime = now()
);

[YOUR-EVENTS-TABLE]
| where [TIMESTAMP-COLUMN] >= startDate and [TIMESTAMP-COLUMN] < endDate
| where [PRODUCT-FILTER-COLUMN] == "[YOUR-PRODUCT-VALUE]"
| where [IS-TEST-FLAG] == false
| extend Month = startofmonth([TIMESTAMP-COLUMN])
| extend CustomerType = iff([IS-INTERNAL-FLAG], "Internal", "External")
| summarize
    MAU = dcount([USER-ID-COLUMN]),
    ActiveOrgs = dcount([TENANT-ID-COLUMN])
    by Month, CustomerType
| order by Month desc, CustomerType asc
```

---

## Expected Output

| month_start | customer_type | mau | active_orgs |
|------------|--------------|-----|------------|
| 2026-03-01 | External | 312 | 45 |
| 2026-03-01 | Internal | 82 | 12 |
| 2026-02-01 | External | 298 | 43 |

---

## Notes

- MAU/DAU ratio indicates engagement intensity (higher = more habitual usage)
- Watch for divergence between MAU growth and active org growth — growth in one without the other tells different stories
- Internal users should be tracked separately and excluded from customer-facing OKR reporting
