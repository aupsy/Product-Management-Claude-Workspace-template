---
name: weekly-focus
description: Generate a prioritized weekly focus areas doc, synthesizing recent emails/meetings and project context
user-invocable: true
argument-hint: (no arguments needed)
allowed-tools: mcp__workiq__ask_work_iq, Read, Glob, Write
---

# Weekly Focus Areas — [YOUR-TEAM-NAME]

Generates a prioritized `Team/Weekly Focus Areas/YYYY-MM-DD-weekly-focus.md` for the start of each week, combining:
- Recent emails, meetings, and Slack threads (via configured productivity tool)
- Current OKR gaps and team priorities (from `CLAUDE.md`)
- Active customer situations (from `Customers/`)
- In-flight initiatives (from `Projects/`)

---

## Step 1: Pull Recent Signals from Productivity Tool

Read `.claude/skills/productivity-tools.md` to determine which tool is active.

### If Microsoft 365 / WorkIQ is active:

Call `mcp__workiq__ask_work_iq` with:

> "What are the most important emails, meetings, and discussions from the past 2 weeks related to [YOUR-PRODUCT-NAME], [YOUR-TEAM-NAME], customer escalations, or key priorities? Summarize key action items, decisions, and open issues — organized by theme. Also surface any meeting recordings or transcripts related to customers."

### If Slack is active:

Search the channels configured in `productivity-tools.md` for the past 2 weeks:
- Messages mentioning customers or product areas by name
- Threads with unresolved questions or pending action items
- Announcements, decisions, or escalations flagged in thread
- Any pinned messages or bookmarks from the past week

Use the Slack MCP tool specified in the config (e.g., `mcp__slack__search_messages`).

### If Google Workspace is active:

Query recent Gmail threads and Calendar events:
- Unresolved email threads with action items
- Meeting notes and follow-ups from customer calls (last 2 weeks)
- Any Google Meet transcripts or recordings if accessible

### If no tool is configured:

Proceed with project context only. Note at the top of the report:
> *Note: No productivity tool configured. Context based on project files only. To include email/meeting context, configure a tool in `.claude/skills/productivity-tools.md`.*

Store any result as `$COMMS_CONTEXT`.

---

## Step 2: Load Project Context

Read in parallel:
- `CLAUDE.md` — OKRs, priority framework, team context
- `Customers/Weekly Feedback Reports/` — latest feedback report (most recently modified `.md`)
- Any `Customers/*/` engagement notes modified in the past 14 days
- `Projects/` — scan for active `INITIATIVE-OVERVIEW*.md` files

Key signals to extract:
- Current OKR numbers vs targets
- Customer escalations or blockers
- Active initiatives that need PM unblocking this week

---

## Step 3: Synthesize and Prioritize

Use the priority framework from `CLAUDE.md`:
- **P0**: Customer escalations, OKR-blocking issues, time-sensitive decisions, unresolved pilot blockers
- **P1**: Advances 90-day deliverables, high-leverage product decisions
- **P2**: Prep for recurring commitments, relationship building

**Formatting rules:**
- Each item must have a concrete **"This week:"** action — not vague direction
- Include the OKR or customer each item ladders up to
- Keep the total to ~5–8 items max (ruthless prioritization is the point)
- End with "The One Number to Watch" anchoring everything to the most at-risk OKR

---

## Step 4: Write the Report

Determine the Monday of the current week as `$WEEK_START`.

Save to: `Team/Weekly Focus Areas/$WEEK_START-weekly-focus.md`

```markdown
# Week of [Month Day] — [YOUR-TEAM-NAME] Focus

*Generated [TODAY] · Sources: recent emails/meetings + project context*

---

## P0: [Item Title]

[1-2 sentence context on why this is urgent this week]

- **This week**: [Concrete action]
- [Supporting bullet if needed]
- **Ladders to**: [OKR or customer]

---

## P1: [Item Title]

...

---

## P2: [Item Title]

...

---

## The One Number to Watch This Week

**[OKR metric]** — currently [X], target [Y] by [date]. [1-2 sentences on what moves it this week.]
```

---

## Step 5: Confirm to User

- File path (as a clickable link)
- 2–3 bullet summary of the top P0s
- Invite edits

---

## Notes

- If WorkIQ is not available, proceed with project context only
- Do not include items with no clear weekly action
- "The One Number to Watch" should rotate to whichever OKR is most at-risk or most in-motion
