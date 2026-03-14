---
name: product-telemetry
description: Connect to your product's telemetry stack and build calibrated queries for the weekly business review
user-invocable: true
argument-hint: [setup | query <natural language question> | list]
allowed-tools: mcp__azure__kusto, Read, Write, Edit, Glob, Bash
---

# Product Telemetry

Connects to your telemetry stack and translates natural language questions into queries — in the right language for your stack (KQL, SQL, etc.).

**User's request:** $ARGUMENTS

---

## Commands

| Command | What it does |
|---------|-------------|
| `/product-telemetry setup` | One-time setup: identify your stack, discover schema, calibrate first queries |
| `/product-telemetry query <question>` | Answer a natural language question using your telemetry |
| `/product-telemetry list` | Show all calibrated queries and their status |

---

## Step 0: Identify Your Stack

**On first run (`setup`)**, ask the user:

> What telemetry stack does your product use?
>
> 1. Azure Data Explorer / Kusto
> 2. Snowflake
> 3. Google BigQuery
> 4. Databricks / Delta Lake
> 5. Amazon Redshift
> 6. PostgreSQL / MySQL / other SQL
> 7. REST API / custom endpoint
> 8. I don't know — help me figure it out

Store the answer as `$STACK`. This determines the query language:

| Stack | Query Language | Connection method |
|-------|---------------|-------------------|
| Azure Data Explorer / Kusto | KQL | `mcp__azure__kusto` or Azure CLI |
| Snowflake | SQL | JDBC / Snowflake CLI / Python connector |
| BigQuery | SQL (BigQuery dialect) | `bq` CLI / Python `google-cloud-bigquery` |
| Databricks | SQL (Spark SQL) | Databricks CLI / JDBC |
| Redshift | SQL (PostgreSQL dialect) | `psql` / Python `psycopg2` |
| PostgreSQL / MySQL | SQL | `psql` / `mysql` CLI |
| REST API | HTTP + JSON | `curl` / Python `requests` |

Save the stack choice to `.claude/skills/product-telemetry/config.md`.

---

## Setup Flow (first-time)

### Step 1: Collect Connection Details

Ask for:
- **Cluster/host**: e.g., `mycompany.snowflakecomputing.com` or `mycluster.kusto.windows.net`
- **Database/schema**: e.g., `PROD_DB.ANALYTICS`
- **Auth method**: e.g., SSO, service account, API key

Save to `.claude/skills/product-telemetry/config.md` (never save passwords or secrets).

### Step 2: Schema Discovery

Probe the database to understand available tables. Run the appropriate discovery query:

**For SQL stacks (Snowflake, BigQuery, Redshift, PostgreSQL):**
```sql
-- List tables with row counts
SELECT table_name, row_count
FROM information_schema.tables
WHERE table_schema = '[YOUR-SCHEMA]'
ORDER BY row_count DESC NULLS LAST
LIMIT 50;
```

**For Kusto / ADX:**
```kql
.show tables
| project TableName, TotalRowCount
| order by TotalRowCount desc
```

Show the user the top tables and ask:
> Which table(s) contain your product's usage events? (e.g., "events", "telemetry", "page_views")

### Step 3: Column Discovery

For each identified table, run:

**SQL:**
```sql
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = '[TABLE]'
ORDER BY ordinal_position;
```

**Kusto:**
```kql
[TABLE]
| getschema
```

Show the column list. Ask the user to identify:
- **Event/activity column**: which column identifies the event type?
- **Timestamp column**: when did the event happen?
- **User/tenant ID column**: who triggered the event?
- **Product filter**: how to filter for just your product's events?

### Step 4: Build Template Queries

Using the discovered schema, generate calibrated versions of the template queries. Replace placeholders in the template query files:

- `template-queries/usage-count.md` → save as `usage-count.sql` (or `.kql`)
- `template-queries/user-engagement.md` → save as `user-engagement.sql`
- `template-queries/funnel-stages.md` → save as `funnel-stages.sql`

### Step 5: Validate

Run each query and show the user a sample of results. Ask:
> Do these numbers look right? (Compare against your BI dashboard if you have one)

If they look off, run `/calibrate-metric` to fine-tune.

### Step 6: Register in WBR

Update `.claude/skills/weekly-business-review/skill.md`:
- Replace `[YOUR-USAGE-QUERY]` with the actual query filename
- Replace `[YOUR-USER-METRIC]` with the actual metric name
- Update the Configuration Notes table

---

## Query Mode

When invoked as `/product-telemetry query <question>`:

1. Load the stack and connection details from `config.md`
2. Understand the question — what metric, what time range, what breakdown?
3. Check if a calibrated query exists for this type of question
4. If yes: run the calibrated query with the appropriate parameters
5. If no: generate a new query from schema knowledge, run it, and show results
6. Always show the query used (so the user can validate it)

---

## Configuration File

After setup, `.claude/skills/product-telemetry/config.md` contains:

```markdown
# Telemetry Stack Configuration

## Stack
[e.g., Snowflake]

## Connection
- Host: [e.g., mycompany.snowflakecomputing.com]
- Database: [e.g., PROD_DB]
- Schema: [e.g., ANALYTICS]
- Auth: [e.g., SSO via browser]

## Key Tables
- [TABLE_NAME]: [What it contains] — primary usage table
- [TABLE_NAME]: [What it contains] — user/account table

## Key Columns
- Event/activity: [column name]
- Timestamp: [column name]
- User/tenant ID: [column name]
- Product filter: [column = 'value']

## Calibrated Queries
- usage-count.sql — Calibrated ✓
- user-engagement.sql — Calibrated ✓
- funnel-stages.sql — In progress
```
