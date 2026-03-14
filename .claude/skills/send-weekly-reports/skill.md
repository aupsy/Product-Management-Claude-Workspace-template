---
name: send-weekly-reports
description: Email the weekly focus areas and/or customer feedback report to your stakeholders via Microsoft Graph API
user-invocable: true
argument-hint: [focus | feedback | both] [--dry-run] (defaults to both)
allowed-tools: Bash, Read, Glob
---

# Send Weekly Reports

Emails weekly reports to your stakeholders. Sends from your own account via delegated Microsoft Graph API (Mail.Send scope). Each email includes an AI-generated disclaimer.

**User's request:** $ARGUMENTS

<!-- ============================================================
  SETUP REQUIRED
  Update the Recipients section below with your actual stakeholders.
  Update [YOUR-PRODUCT-SHORT] and [YOUR-TEAM-NAME] with your values.
  ============================================================ -->

---

## Recipients

<!-- Replace these with your actual recipients. Use distribution lists where available. -->

| Report | To |
|--------|----|
| Weekly Focus Areas | [YOUR-TEAM-DL@company.com], [YOUR-MANAGER@company.com] |
| Customer Feedback Report | [YOUR-TEAM-DL@company.com], [YOUR-MANAGER@company.com], [YOUR-CSM@company.com] |

---

## Workflow

### Step 1: Parse Arguments

| Argument | Behaviour |
|----------|-----------|
| *(none)* | Send both reports |
| `focus` | Weekly focus areas email only |
| `feedback` | Customer feedback report email only |
| `both` | Both emails |
| `--dry-run` | Print email content without sending (safe to test) |

### Step 2: Install Dependencies

```bash
pip install msal requests -q
```

### Step 3: Locate Source Files

| Report | Source |
|--------|--------|
| Focus | Latest `*.md` in `Team/Weekly Focus Areas/` |
| Feedback | Latest `[YOUR-PRODUCT-SHORT]_Feedback_*.md` in `Customers/Weekly Feedback Reports/` |

If either file is missing, stop and tell the user:
> Run `/weekly-focus` first (for focus report) or `/feedback-report` first (for feedback report).

### Step 4: Run the Script

```bash
python ".claude/skills/send-weekly-reports/send_reports.py" --report [REPORT_TYPE]
```

Add `--dry-run` to preview without sending.

### Step 5: First-Run Auth (Microsoft Graph)

On first run, the script triggers MSAL device flow:

1. A URL and code are printed to the terminal
2. Open the URL in a browser and enter the code
3. Sign in with your work account
4. Grant `Mail.Send` permission (delegated — sends as you)
5. Token is cached at `~/.pm_workspace_msal_cache.json` — subsequent runs skip this step

### Step 6: Confirm to User

Report which emails were sent, to whom, and from which source files.

---

## Email Content

### Email A — Weekly Focus Areas
- **Subject**: `Week of [DATE] — [YOUR-TEAM-NAME] Focus [AI-Generated]`
- **Body**: P0/P1/P2 focus items with priority badges, from the latest weekly focus doc
- **AI disclaimer** header

### Email B — Customer Feedback Report
- **Subject**: `[YOUR-PRODUCT-SHORT] Customer Feedback Report — [DATE] [AI-Generated]`
- **Body**: Executive summary + top categories table + top feature requests, from the latest feedback report
- **AI disclaimer** header

---

## Usage Examples

```bash
/send-weekly-reports              # Send both
/send-weekly-reports focus        # Focus areas only
/send-weekly-reports feedback     # Feedback only
/send-weekly-reports both --dry-run  # Preview without sending
```

---

## Error Handling

| Error | Resolution |
|-------|-----------|
| No focus doc found | Run `/weekly-focus` first |
| No feedback report found | Run `/feedback-report` first |
| Auth failed | Re-run — token cache may be stale; device flow will re-authenticate |
| Send failed 401 | Token expired — re-auth |
| Send failed 403 | Check that your account has a mailbox and can send to the DL |
| `msal`/`requests` not installed | Script auto-installs via pip |

---

## Non-Microsoft Accounts

If you're not using a Microsoft/Office 365 account, replace the Graph API send logic in `send_reports.py` with your email provider's API:
- **Gmail**: Use Gmail API with OAuth 2.0
- **SMTP**: Use Python `smtplib` (simpler, works anywhere)
- **SendGrid / Mailgun**: Use their REST APIs
