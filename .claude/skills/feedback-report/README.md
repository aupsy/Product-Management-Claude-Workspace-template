# Feedback Report Skill — Setup Guide

## What to Configure

Open `skill.md` and replace these placeholders:

| Placeholder | Example |
|-------------|---------|
| `[YOUR-FEEDBACK-SOURCE]` | SuccessHub, Zendesk, Intercom, Pendo, UserVoice |
| `[YOUR-FEEDBACK-EXPORT-FORMAT]` | Excel (.xlsx), CSV, JSON |
| `[YOUR-FEEDBACK-DIR]` | Path to your default export folder |
| `[YOUR-PRODUCT-SHORT]` | Your product acronym |
| `[YOUR-FEEDBACK-FILENAME-PATTERN]` | How export files are named |

## Usage

```
/feedback-report                          # uses default directory
/feedback-report path/to/feedback.xlsx   # specific file
/feedback-report path/to/folder          # finds latest file in folder
```

## Output

Saves to:
- `[YOUR-FEEDBACK-DIR]/[PRODUCT]_Feedback_[DATE].md`
- `Customers/Weekly Feedback Reports/[PRODUCT]_Feedback_[DATE].md`

## Notes

- Themes are discovered from your data, not hardcoded — this means the skill adapts to any product
- Run weekly for trend analysis
- Use the top themes as input for feature pitches and sprint planning
