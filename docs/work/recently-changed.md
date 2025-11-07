# Recently Changed

> **Purpose:** Quick view of recent activity across all work items.  
> **Update:** Every time a work item changes status or has significant updates.  
> **Retention:** Keep last 30 days of changes.

## Recent Changes

<!-- Format: | [YYYY-MM-DD HH:MM] | [Feature/Story ID] | [Type] | [Change] | [By] | -->
<!-- Type: Feature | Story | Assignment | Blocker | -->

| Timestamp | ID | Type | Change | By |
|-----------|-----|------|--------|-----|
| _No recent changes_ | - | - | - | - |

---

## Instructions

### Recording Changes

Add new entries at the TOP of the table (most recent first):

```markdown
| 2025-11-07 14:30 | 00042 | Story | Moved to done | @alice |
| 2025-11-07 10:15 | 00042 | Assignment | Assigned to @alice | @pm |
| 2025-11-07 09:00 | 00001 | Feature | Moved to in-progress | @pm |
```

### Change Types

- **Feature** - Feature status change (todo → in-progress → done → blocked)
- **Story** - User story status change
- **Assignment** - Assignment or re-assignment of work
- **Blocker** - Blocker added or resolved
- **Update** - Significant content update (scope change, AC change, etc.)

### What to Record

**DO record:**
- Status transitions
- Assignment changes
- Blockers added/resolved
- Significant scope changes
- Completion of work

**DON'T record:**
- Minor edits or typo fixes
- Routine updates to audit logs (those are in the work items themselves)
- Discussion or comments (use work item audit log)

### Maintenance

At the start of each month:
1. Archive changes older than 30 days
2. Keep the file manageable (~ 50-100 entries max)
3. Full history is always in git commits and work item audit logs

---

## Tips

- **Update immediately** - When you change a work item, update this file in the same commit
- **Be concise** - One line per change, details are in the work item itself
- **Use IDs** - Always reference numeric IDs (00001, 00042) not names
- **Timestamp accurately** - Helps correlate with git commits and team activity
