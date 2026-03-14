# Productivity Tools — Configuration

This file configures which productivity tool Claude uses to pull context from recent emails, meetings, Slack threads, and recorded calls.

Claude reads this file in `/weekly-focus`, `/weekly-business-review`, and customer-related skills to enrich output with real conversational context.

---

## Your Stack

<!-- Set one of the options below to "active: true". The others should be "active: false". -->

### Option A: Microsoft 365 (WorkIQ)

```yaml
productivity_stack: microsoft365
tool: workiq
active: true       # ← set to true if you use Microsoft 365
mcp_tool: mcp__workiq__ask_work_iq
```

**Setup**: WorkIQ MCP must be configured in your Claude Code settings. If `mcp__workiq__ask_work_iq` is available as a tool, it's already set up.

**What it provides**: Recent emails, Teams meetings, calendar events, and documents from your Microsoft 365 account.

---

### Option B: Google Workspace

```yaml
productivity_stack: google_workspace
tool: google_calendar_gmail
active: false      # ← set to true if you use Google Workspace
```

**Setup**: Install the Google Workspace MCP server or configure Gmail / Calendar API access.

Steps:
1. Install MCP server: `npm install -g @google/mcp-server-gmail` (or equivalent)
2. Add to your Claude Code MCP config
3. Authenticate with your Google account

**What it provides**: Recent Gmail threads, Google Calendar meeting notes, and Google Meet transcripts (if enabled).

---

### Option C: Slack

```yaml
productivity_stack: slack
tool: slack_mcp
active: false      # ← set to true if your team primarily uses Slack
mcp_tool: mcp__slack__search_messages   # adjust to your Slack MCP tool name
channels_to_monitor:
  - "[YOUR-TEAM-CHANNEL]"           # e.g., "#your-product-team"
  - "[YOUR-PRODUCT-CHANNEL]"        # e.g., "#your-product-name"
  - "[YOUR-CUSTOMER-CHANNEL]"       # e.g., "#customer-discussions"
```

**Setup**: Install the Slack MCP server and authenticate with your workspace.

**What it provides**: Recent messages from specified channels — useful for surfacing customer issues, team decisions, and open questions.

---

### Option D: No productivity tool / manual

```yaml
productivity_stack: none
active: false
```

When no productivity tool is configured:
- `/weekly-focus` runs on project context only (CLAUDE.md + Customers/ + Projects/)
- `/weekly-business-review` uses data only, no meeting/email context
- Customer deep dives rely only on files in `Customers/`

---

## How Skills Use This

| Skill | What it pulls |
|-------|--------------|
| `/weekly-focus` | Recent emails, meetings, and discussions from the past 2 weeks — surfaces open action items, decisions, and escalations |
| `/weekly-business-review` | Any customer-related discussions or escalations from the past week — adds context to the named customer section |
| Customer queries ("What's new with Acme?") | Recent meetings or Slack threads mentioning that customer |
| `/hydrate-knowledge` | Can search for product-related documents in your connected workspace |

---

## Privacy Note

Claude only reads communications through the tools you explicitly configure here. No data is sent outside your local session. The productivity tool integration requires you to have already authenticated with the service — Claude does not store credentials.
