---
name: feedback-report
description: Generate a weekly customer feedback report from your feedback export (e.g., SuccessHub, Zendesk, Intercom, CSV)
user-invocable: true
argument-hint: [file path or directory] (optional)
allowed-tools: Bash, Read, Glob, Grep
---

# Customer Feedback Report Generator

Generates a structured weekly feedback report from a customer feedback export, categorized by theme and ranked by customer impact.

**User's request:** $ARGUMENTS

<!-- ============================================================
  SETUP REQUIRED
  Update these placeholders before using:
  - [YOUR-FEEDBACK-SOURCE]: Where feedback comes from (e.g., "SuccessHub", "Zendesk", "Intercom", "Pendo")
  - [YOUR-FEEDBACK-EXPORT-FORMAT]: File format (e.g., "Excel (.xlsx)", "CSV", "JSON")
  - [YOUR-FEEDBACK-DIR]: Default directory where exports land
  - [YOUR-PRODUCT-SHORT]: Your product name/acronym
  - [YOUR-FEEDBACK-FILENAME-PATTERN]: How export files are named (e.g., "feedback-*.xlsx", "All Feedback Items *.xlsx")
  ============================================================ -->

## Goal

Turn raw feedback exports from **[YOUR-FEEDBACK-SOURCE]** into a structured themes report in under 60 seconds. Surfaces the top 10 feedback categories, top feature requests, and executive summary.

---

## Step 0: Parse Arguments

**Case 1 — Specific file path provided:**
```
/feedback-report path/to/feedback.xlsx
```
Use the provided file.

**Case 2 — Directory path provided:**
```
/feedback-report path/to/exports/folder
```
Find the latest `[YOUR-FEEDBACK-FILENAME-PATTERN]` file in that folder.

**Case 3 — No arguments:**
```
/feedback-report
```
Use the default directory: `[YOUR-FEEDBACK-DIR]`

---

## Step 1: Locate the Feedback File

1. Apply the case logic above to find the file
2. If no file is found, guide the user:
   > **No feedback export found.**
   >
   > Please export feedback from [YOUR-FEEDBACK-SOURCE]:
   > 1. Go to [YOUR-FEEDBACK-SOURCE]
   > 2. Filter for [YOUR-PRODUCT-SHORT] feedback
   > 3. Export as [YOUR-FEEDBACK-EXPORT-FORMAT]
   > 4. Save to: `[YOUR-FEEDBACK-DIR]`
   > 5. Re-run: `/feedback-report [path-to-file]`

---

## Step 2: Read and Parse the Feedback

Read the feedback file. Extract for each item:
- **Title** / summary
- **Description** / full text
- **Customer name** / account
- **Status** (Active, Resolved, Closed, etc.)
- **Priority** (if available)
- **Date created**
- **Category / tags** (if pre-labeled)

If the file is Excel (`.xlsx`), use:
```bash
python -c "import pandas as pd; df = pd.read_excel('[FILE]'); print(df.to_csv(index=False))"
```

---

## Step 3: Categorize by Theme

Read through the feedback and assign each item to one or more themes. Derive themes from the data — do not assume fixed categories.

**Approach for theme discovery:**
1. Read all feedback descriptions
2. Identify the top recurring topics (aim for 10–15 themes)
3. Name each theme clearly (e.g., "Onboarding friction", "API rate limits", "Reporting gaps")
4. Assign items to themes (items can belong to multiple themes)
5. Rank themes by: (a) number of customers affected, (b) number of items

**Example themes** (replace with what you find in your data):
- Feature gaps
- Performance / reliability
- Onboarding / setup
- Documentation
- Integrations
- Pricing / packaging
- Mobile experience
- Admin / configurability

---

## Step 4: Generate the Report

Save to the feedback directory as: `[YOUR-PRODUCT-SHORT]_Feedback_[YYYY-MM-DD].md`

Also copy to: `Customers/Weekly Feedback Reports/[YOUR-PRODUCT-SHORT]_Feedback_[YYYY-MM-DD].md`

Report structure:

```markdown
# [YOUR-PRODUCT-SHORT] Customer Feedback Report
**Generated:** [DATE]
**Source:** [YOUR-FEEDBACK-SOURCE] export — [filename]
**Total items:** [X] active, [Y] resolved

---

## Executive Summary

**What customers are saying:**
- [Top positive signal]
- [Top negative signal / pain point]
- [Trend vs prior period if available]

**Top 5 themes by customer impact:**
1. [Theme] — [X] items, [Y] customers
2. ...

**Critical items needing attention:** [N]

---

## Top 10 Categories

| # | Category | Items | Customers | % of Total |
|---|----------|-------|-----------|------------|
| 1 | [Theme]  | [X]   | [Y]       | [Z]%       |
| ...

---

## Top 10 Feature Requests

| # | Feature | Customers Requesting |
|---|---------|---------------------|
| 1 | [Feature description] | [X] |
| ...

---

## Theme Deep Dives

### [Theme 1 Name]
[1-2 sentence summary]

**Representative items:**
- **[Customer]**: "[Feedback quote or summary]" *(Status: Active)*
- ...

[Repeat for each theme]

---

## Resolution Tracking

### Recently Resolved / Released
| Item | Customer | Resolution |
|------|----------|------------|
| [Item] | [Customer] | [What was done] |

```

---

## Step 5: Display Key Results

Show the user:
1. Executive summary (top highlights)
2. Report file path
3. Top 3 actionable insights

---

## Error Handling

| Error | Resolution |
|-------|-----------|
| File not found | Guide to export from [YOUR-FEEDBACK-SOURCE] |
| File locked | Ask user to close the file before running |
| Parse error | Check file format; try opening in Excel first |
| `pandas` not installed | Run `pip install pandas openpyxl` |
