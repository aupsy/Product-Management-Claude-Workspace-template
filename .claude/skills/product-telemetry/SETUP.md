# Product Telemetry — Setup Guide

## Overview

This skill connects Claude to your product's telemetry data source. It supports any stack — Kusto, Snowflake, BigQuery, Databricks, Redshift, or standard SQL.

## Setup Steps

### 1. Run the setup command

```
/product-telemetry setup
```

Claude will ask you to identify your telemetry stack and walk you through connecting to it.

### 2. What you'll need

- Your cluster/host address
- The database and schema where your product's events live
- Your auth method (SSO, API key, service account)
- 15-30 minutes to run schema discovery and validate the first queries

### 3. What gets created

After setup, you'll have:
- `config.md` — your connection details (no secrets stored)
- Calibrated query files (`usage-count.sql`, `user-engagement.sql`, etc.)
- Updated `weekly-business-review/skill.md` with your metric names

---

## Manual Setup (if you prefer)

If you'd rather configure manually:

1. Create `config.md` with your stack details (see template in `skill.md`)
2. Copy and fill in the template queries from `template-queries/`
3. Run `/calibrate-metric` to validate each query against your BI dashboard

---

## Supported Stacks

| Stack | What you need |
|-------|--------------|
| Azure Data Explorer / Kusto | Cluster URI, database name, Azure auth |
| Snowflake | Account URL, warehouse, database, schema, SSO or key pair |
| Google BigQuery | Project ID, dataset, service account or ADC |
| Databricks | Workspace URL, cluster ID or SQL warehouse, PAT token |
| Amazon Redshift | Cluster endpoint, database, username/password or IAM |
| PostgreSQL | Host, port, database, username |
| MySQL | Host, port, database, username |

---

## Troubleshooting

| Problem | Solution |
|---------|---------|
| Can't connect | Check VPN, auth, and that the cluster hostname is correct |
| Tables not found | Try a different schema — use schema discovery query to list all tables |
| Query returns wrong numbers | Run `/calibrate-metric` to debug against BI ground truth |
| Stack not listed | Use "other SQL" option; Claude will generate standard ANSI SQL |
