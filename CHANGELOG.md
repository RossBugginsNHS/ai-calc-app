# AgentMD v2.0 - Conversational Workflow System

## Overview of Changes

This document summarizes the restructuring of AgentMD from a file-based input system to a conversational, context-aware workflow system.

## Major Changes

### 1. Conversational Customer Intake

**Before**: Customer had to write `docs/input/project-brief.md` manually

**After**: Agent engages in conversation, asks questions, and creates the project brief

**Benefits**:
- Lower barrier to entry
- Agent can ask clarifying questions
- More natural interaction
- Better quality briefs through guided conversation

### 2. New Folder Structure

**Before**:
```
docs/
├── input/
├── requirements/
├── architecture/
├── design/
├── planning/
└── implementation/
```

**After**:
```
outputs/              # Role-organized artifacts
├── 00-customer/
├── 01-business-analyst/
├── 02-requirements-engineer/
├── ...
└── 12-project-manager/

history/              # Interaction logs
└── [timestamp]-[role].md

handovers/            # Context management
├── handover.md
└── handover-histories/
```

**Benefits**:
- Clear organization by role
- Easy to find artifacts
- Better audit trail
- Context preservation

### 3. History Tracking

**New**: Every interaction is logged

**File Format**: `[yyyyMMdd-HHmm]-[role-number]-[role-name].md`

**Contents**:
- What was discussed
- Decisions made
- Artifacts created
- Questions and answers
- Next steps

**Benefits**:
- Complete audit trail
- Understanding of decision rationale
- Reference for future team members
- Project timeline documentation

### 4. Handover System

**New**: Ability to pause and resume across chat contexts

**Files**:
- `handovers/handover.md` - Current handover (if any)
- `handovers/handover-histories/[timestamp]-handover.md` - Archived handovers

**Workflow**:
1. Agent completes role
2. Customer requests handover
3. Agent creates handover summary
4. Agent commits all changes
5. Customer creates new chat
6. Agent resumes from handover

**Benefits**:
- Long projects can span multiple days/sessions
- No context loss
- Clear stopping points
- Flexible work schedule

### 5. UTC Timestamps

**Requirement**: All times must be UTC

**Formats**:
- Filenames: `yyyyMMdd-HHmm`
- Display: `YYYY-MM-DD HH:mm UTC`

**Benefits**:
- Global consistency
- No timezone confusion
- Clear ordering
- International team support

### 6. Git Integration

**New**: Automatic commits at role transitions

**Format**: `Completed [Role Name] - [yyyyMMdd-HHmm]`

**When**: After each role when handover is created

**Benefits**:
- Version control of progress
- Rollback capability
- Clear project history
- Collaboration support

### 7. Role 0: Customer

**New**: Explicit customer role (Role 0)

**Purpose**: Formalize the customer intake process

**Output**: `outputs/00-customer/project-brief.md`

**Benefits**:
- Clear workflow start
- Structured information gathering
- Customer feels involved
- Better requirements quality

## File Mapping

### Artifacts Migration

Old artifacts in `docs/` subfolders now go to role-specific `outputs/` folders:

| Old Location | New Location |
|--------------|--------------|
| `docs/requirements/business-requirements.md` | `outputs/01-business-analyst/business-requirements.md` |
| `docs/requirements/functional-requirements.md` | `outputs/02-requirements-engineer/functional-requirements.md` |
| `docs/architecture/system-architecture.md` | `outputs/03-system-architect/system-architecture.md` |
| `docs/architecture/security-architecture.md` | `outputs/04-security-architect/security-architecture.md` |
| `docs/design/wireframes.md` | `outputs/05-ux-ui-designer/wireframes.md` |
| `docs/architecture/database-schema.md` | `outputs/06-database-designer/database-schema.md` |
| `docs/design/api-specification.md` | `outputs/07-api-designer/api-specification.md` |
| `docs/planning/infrastructure-as-code.md` | `outputs/08-devops-engineer/infrastructure-as-code.md` |
| `docs/planning/test-strategy.md` | `outputs/09-test-architect/test-strategy.md` |
| `docs/implementation/implementation-roadmap.md` | `outputs/10-technical-lead/implementation-roadmap.md` |
| `docs/implementation/user-guide.md` | `outputs/11-documentation-writer/user-guide.md` |
| `docs/planning/project-plan.md` | `outputs/12-project-manager/project-plan.md` |

## Updated Documentation

### New Files

1. **`agent.md`** - Completely rewritten with conversational approach
2. **`README.md`** - Comprehensive user guide
3. **`AGENT-QUICK-REFERENCE.md`** - Quick reference for the agent
4. **`outputs/README.md`** - Explains outputs folder
5. **`history/README.md`** - Explains history tracking
6. **`handovers/README.md`** - Explains handover system
7. **`docs/roles/00-customer.md`** - Customer role definition

### Updated Files

1. **`docs/roles-and-artifacts.md`** - Added Role 0, updated workflow diagram
2. All role files (01-12) - Still valid, now reference new folder structure

## Workflow Comparison

### Old Workflow

```
1. Customer writes project-brief.md
2. Customer tells agent to start
3. Agent works through roles 1-12
4. All done in one session
```

### New Workflow

```
1. Customer says "Hi"
2. Agent asks about project
3. Agent creates project-brief.md (Role 0)
4. Agent works through roles 1-12
5. Handovers at natural break points
6. Can span multiple sessions
7. Complete history logged
```

## Usage Examples

### Example 1: One Session (Simple Project)

```
Customer: Hi, I need a simple contact form
Agent: [asks questions]
Agent: [creates brief]
Agent: [completes roles 0-12]
Agent: Project complete!
Time: 2-3 hours
```

### Example 2: Multi-Session (Complex Project)

```
Day 1:
- Customer intake
- Roles 1-4 (Business, Requirements, Architecture, Security)
- Handover created

Day 2:
- Resume from handover
- Roles 5-8 (UX, Database, API, DevOps)
- Handover created

Day 3:
- Resume from handover
- Roles 9-12 (Testing, Tech Lead, Docs, PM)
- Project complete!
```

### Example 3: With Revisions

```
Session 1: Roles 0-5
Handover

Session 2: Roles 6-8
Customer: "Wait, we need to change the database design"
Agent: Returns to Role 6, updates artifacts, logs revision
Agent: Continues with Roles 7-8
Handover

Session 3: Roles 9-12
Complete!
```

## Benefits Summary

✅ **Conversational**: Natural interaction vs. file writing
✅ **Flexible**: Break work into manageable sessions
✅ **Traceable**: Complete history of all decisions
✅ **Resumable**: Pick up where you left off
✅ **Organized**: Clear folder structure
✅ **Versioned**: Git commits at milestones
✅ **Global**: UTC timestamps everywhere
✅ **Professional**: Comprehensive documentation

## Migration Guide

If you have an existing project using the old structure:

1. Create new folder structure (outputs, history, handovers)
2. Move artifacts from `docs/` to `outputs/[role]/`
3. Create initial history file documenting the migration
4. Optional: Create handover if resuming work
5. Continue with next role

## Technical Details

### Timestamp Format

**For Filenames**: `yyyyMMdd-HHmm`
- Year: 4 digits
- Month: 2 digits (01-12)
- Day: 2 digits (01-31)
- Hour: 2 digits (00-23)
- Minute: 2 digits (00-59)

Example: `20251107-1430` = November 7, 2025 at 2:30 PM UTC

**For Display**: `YYYY-MM-DD HH:mm UTC`

Example: `2025-11-07 14:30 UTC`

### Git Commit Format

```
Completed [Role Name] - [timestamp]
```

Examples:
- `Completed Customer Intake - 20251107-1400`
- `Completed Business Analyst - 20251107-1545`
- `Completed Project Manager - 20251108-1600`

### Handover Trigger Points

Recommended times to create handovers:

- After Role 4 (Security Architect) - Foundation complete
- After Role 8 (DevOps Engineer) - Design complete
- After Role 10 (Technical Lead) - Implementation plan ready
- Any time session runs >1 hour
- Any time 2+ roles completed
- End of day
- Before weekend
- When switching team members

## Future Enhancements

Potential additions to consider:

1. **Milestone markers** - Formal checkpoints with customer approval
2. **Parallel role execution** - Run roles 5-8 concurrently
3. **Custom role templates** - Easy addition of project-specific roles
4. **Automated reports** - Generate status reports from history
5. **Role dependencies** - Explicit dependency management
6. **Artifact versioning** - Track artifact changes over time
7. **Customer checkpoints** - Formal approval gates
8. **AI role summaries** - Auto-generate role summaries

## Support

- **Agent Instructions**: `agent.md`
- **Quick Reference**: `AGENT-QUICK-REFERENCE.md`
- **User Guide**: `README.md`
- **Role Details**: `docs/roles/[role].md`
- **Workflow**: `docs/roles-and-artifacts.md`

## Version History

- **v1.0**: Original file-based system
- **v2.0**: Conversational workflow with history and handovers (current)

---

**Last Updated**: 2025-11-07  
**Status**: Active Development
