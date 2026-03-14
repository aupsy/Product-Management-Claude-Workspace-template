# Team

Weekly focus documents and team ritual artifacts.

## Contents

| Item | What it is |
|------|-----------|
| `Weekly Focus Areas/` | Output folder for `/weekly-focus` skill |
| *(add as needed)* | Team ritual docs, retro notes, offsites, etc. |

## Weekly Focus Areas

Generated every Monday by `/weekly-focus`. Each file is named `YYYY-MM-DD-weekly-focus.md` (the Monday date).

The weekly focus doc synthesizes:
- Your team's OKR gaps (from `CLAUDE.md`)
- Active customer situations (from `Customers/`)
- In-flight initiatives (from `Projects/`)
- Recent emails and meetings (from your productivity tool, if connected)

## Connecting Your Productivity Tool

The `/weekly-focus` skill can pull context from your work communications. See the productivity tools section in `.claude/skills/weekly-focus/skill.md` for setup.

Supported:
- **Microsoft 365** — via WorkIQ MCP server
- **Google Workspace** — via Google Calendar / Gmail API (configure in skill.md)
- **Slack** — via Slack MCP server (configure in skill.md)
