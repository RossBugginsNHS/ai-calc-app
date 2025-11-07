# Handovers Folder

This folder manages context handovers between chat sessions during the software development planning process.

## Purpose

Long planning sessions may span multiple chat contexts. The handover system ensures continuity by:
- Summarizing completed work
- Documenting what's next
- Preserving key decisions and context
- Enabling seamless continuation in a new chat

## Files

### `handover.md`
The current active handover file (if one exists). This file contains:
- Summary of the most recently completed role
- Artifacts created
- Key decisions made
- Next steps for the upcoming role
- Any open questions or issues

**This file is created when you request a handover and cleared when starting a new session.**

### `handover-histories/`
Archive of all previous handovers. Each handover is moved here when a new session begins.

Files are named: `[yyyyMMdd-HHmm]-handover.md`

**Examples:**
- `handover-histories/20251107-1630-handover.md`
- `handover-histories/20251108-1015-handover.md`

## Workflow

### Completing a Role

1. Agent announces role completion
2. Agent asks: "Would you like me to prepare for a handover?"
3. If yes:
   - Agent creates `handover.md` with summary
   - Agent stages and commits all changes
   - Customer creates new chat context

### Starting a New Session

1. Agent checks for `handover.md`
2. If found:
   - Agent moves it to `handover-histories/[timestamp]-handover.md`
   - Agent summarizes the handover to customer
   - Agent asks: "Ready to continue?"
3. Agent proceeds with next role

## Handover Template

```markdown
# Handover: [Current Role] → [Next Role]

**Date**: [YYYY-MM-DD HH:mm UTC]  
**Current Role**: [Role Number and Name]  
**Next Role**: [Next Role Number and Name]

## Work Completed

[Summary]

### Artifacts Created
- `path/to/artifact.md` - Description

## Key Decisions Made

1. [Decision]

## Next Steps

[What's next]

### Inputs Available
- List of input artifacts

### Expected Outputs
- List of expected outputs

## Questions/Issues

[Open items]
```

## Benefits

- **Long-running projects**: Break work into manageable sessions
- **Context preservation**: No loss of information between sessions
- **Clear transitions**: Always know where you left off
- **Audit trail**: Complete history of all handovers
- **Flexibility**: Work on project across multiple days/sessions
