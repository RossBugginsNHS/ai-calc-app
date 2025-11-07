# History Folder

This folder contains a chronological record of all interactions during the software development planning process.

## Purpose

Each interaction with the agent is logged in a timestamped file that captures:
- What was discussed
- Questions asked and answered
- Decisions made
- Artifacts created
- Any issues or blockers encountered

## File Naming Convention

Files are named using UTC timestamps: `[yyyyMMdd-HHmm]-[role-number]-[role-name].md`

**Examples:**
- `20251107-1430-00-customer.md` - Customer intake conversation on Nov 7, 2025 at 14:30 UTC
- `20251107-1545-01-business-analyst.md` - Business analysis work on Nov 7, 2025 at 15:45 UTC
- `20251108-0900-02-requirements-engineer.md` - Requirements engineering on Nov 8, 2025 at 09:00 UTC

## File Contents

Each history file typically contains:

```markdown
# [Role Name] - [Date/Time]

**Role**: [Role Number and Name]  
**Timestamp**: [YYYY-MM-DD HH:mm UTC]  
**Session Duration**: [Approximate duration]

## Summary

[Brief overview of what was accomplished]

## Conversation Highlights

[Key points from the conversation with customer]

## Artifacts Created

- `path/to/artifact.md` - Description

## Decisions Made

1. [Decision 1]
2. [Decision 2]

## Questions & Answers

**Q:** [Question]  
**A:** [Answer]

## Next Steps

[What needs to happen next]

## Notes

[Any additional context]
```

## Usage

The agent automatically creates these history files. They serve as:
- An audit trail of the planning process
- Context for understanding why decisions were made
- Reference for future team members
- Documentation of the customer's requirements evolution
