# Work Tracking

This folder contains the ConceptShipAI work tracking system - a lightweight, file-based approach to managing features, user stories, and assignments for CLIENT PROJECTS.

> **⚠️ IMPORTANT:** This is for tracking work on CLIENT PROJECTS using ConceptShipAI, NOT for ConceptShipAI's own development.  
> **📖 READ FIRST:** See [`/docs/work-tracking-instructions.md`](../work-tracking-instructions.md) for comprehensive guidance on when and how to use this system.

## Philosophy

- **Be Agile** - Embrace change, adapt quickly
- **Deliver Value Early and Often** - Focus on outcomes that matter
- **Iterate and Release** - No big bang; ship small increments frequently
- **Transparency** - All work visible in version control
- **Simplicity** - No external tools required; just files and folders

## Structure

```text
docs/work/
├── README.md               # This file - overview and guidance
├── assignments.md          # Flat index of current assignments (who's doing what)
├── recently-changed.md     # Last 30 days of activity across all work items
├── backlog/                # Unrefined ideas and items (anyone can add)
│   └── [idea-files].md
├── features/               # Refined features ready for work
│   ├── todo/               # Features not yet started
│   │   └── 00001-feature-name/
│   │       ├── feature.md
│   │       └── user-stories/
│   │           ├── todo/
│   │           ├── in-progress/
│   │           ├── done/
│   │           └── blocked/
│   ├── in-progress/        # Features actively being worked on
│   ├── done/               # Completed features
│   └── blocked/            # Features with blockers
└── templates/
    ├── feature.md          # Feature template
    └── user-story.md       # User story template
```

## Workflow

### 1. Capture Ideas (Backlog)

Anyone can add rough ideas to `backlog/`:

```bash
# Add a new backlog item
echo "# My Idea\nSome quick thoughts..." > docs/work/backlog/my-idea.md
git add docs/work/backlog/my-idea.md
git commit -m "Add backlog item: my-idea"
```

Items in backlog don't need to be refined. They're just placeholders to capture thoughts.

### 2. Refine Features

The **Project Manager (Role 12)** refines backlog items into features with unique numeric IDs:

```bash
# Find next feature ID (check existing features for highest number)
ls -1 docs/work/features/*/ | grep -Eo '^[0-9]{5}' | sort -n | tail -1
# Add 1 for next ID

# Create a new feature (e.g., ID 00001)
mkdir -p docs/work/features/todo/00001-authentication/user-stories/{todo,in-progress,done,blocked}
cp docs/work/templates/feature.md docs/work/features/todo/00001-authentication/feature.md
# Edit feature.md with details, update ID to 00001
git add docs/work/features/todo/00001-authentication/
git commit -m "Create feature 00001: authentication"
```

### 3. Create User Stories

The **Project Manager** or **Business Analyst (Role 1)** breaks features into user stories with unique IDs:

```bash
# Find next story ID (stories are globally unique across all features)
find docs/work/features -name "[0-9]*-*.md" | grep -Eo '[0-9]{5}' | sort -n | tail -1
# Add 1 for next ID

# Add a user story (e.g., ID 00042)
cp docs/work/templates/user-story.md \
   docs/work/features/todo/00001-authentication/user-stories/todo/00042-login-with-email.md
# Edit user-story.md with acceptance criteria, update ID to 00042, Feature ID to 00001
git add docs/work/features/todo/00001-authentication/
git commit -m "Add story 00042 to feature 00001: login-with-email"
```

### 4. Assign Work

Update `assignments.md` to track who's working on what:

```markdown
## Current Assignments

- **@alice** - [Feature 00001] [Story 00042] - login-with-email (in-progress)
- **@bob** - [Feature 00002] [Story 00043] - user-profile-display (in-progress)
```

### 5. Move Work Through Statuses

As work progresses, move files between status folders and update audit logs:

```bash
# Start working on a feature
git mv docs/work/features/todo/00001-authentication docs/work/features/in-progress/00001-authentication
# Update feature.md audit log: add status change entry

# Start working on a user story
git mv docs/work/features/in-progress/00001-authentication/user-stories/todo/00042-login-with-email.md \
       docs/work/features/in-progress/00001-authentication/user-stories/in-progress/00042-login-with-email.md
# Update story audit log: add status change and assignment entries
# Update assignments.md
# Update recently-changed.md

# Complete a user story
git mv docs/work/features/in-progress/00001-authentication/user-stories/in-progress/00042-login-with-email.md \
       docs/work/features/in-progress/00001-authentication/user-stories/done/00042-login-with-email.md
# Update story audit log: add completion entries
# Remove from assignments.md
# Update recently-changed.md

# Complete a feature (when all stories done)
git mv docs/work/features/in-progress/00001-authentication docs/work/features/done/00001-authentication
# Update feature audit log
# Update recently-changed.md
```

### 6. Handle Blockers

If work is blocked:

```bash
# Move to blocked status
git mv docs/work/features/in-progress/00001-authentication/user-stories/in-progress/00042-login-with-email.md \
       docs/work/features/in-progress/00001-authentication/user-stories/blocked/00042-login-with-email.md

# Update story with blocker details in audit log
# Update assignments.md (move to Blocked section)
# Update recently-changed.md

# When unblocked, move back to in-progress
git mv docs/work/features/in-progress/00001-authentication/user-stories/blocked/00042-login-with-email.md \
       docs/work/features/in-progress/00001-authentication/user-stories/in-progress/00042-login-with-email.md
# Update audit log with unblock entry
```

## Templates

See `docs/work/templates/` for:

- `feature.md` - Feature template with ID, description, goals, stories, acceptance criteria, append-only audit log
- `user-story.md` - User story template with ID, role, goal, benefit, acceptance criteria, tasks, append-only audit log

## Key Concepts

### Unique Numeric IDs

- **Features** and **User Stories** have globally unique 5-digit IDs (00001, 00042, etc.)
- **Format:** `00001-short-kebab-case-name`
- **Never reuse IDs** - even if work items are deleted
- Use IDs for references in handovers, assignments, and communication

### Append-Only Audit Logs

Every work item has an audit log section at the bottom:
- **Status Changes** - Every status transition with timestamp
- **Assignment Changes** - Every assignment/re-assignment
- **Blocker Log** (stories only) - Blockers added and resolved
- **Updates** - Significant changes to work item content

**CRITICAL:** Never edit or delete audit log entries. Always append new entries at the bottom.

### Recently Changed Tracking

`recently-changed.md` provides a quick view of the last 30 days of activity:
- Updated whenever work items change
- Shows feature/story IDs, change type, and who made the change
- Replaces traditional "history" folder - work items ARE the history

## Roles Involved

- **Customer (Role 0)** - Provides vision, priorities, feedback
- **Business Analyst (Role 1)** - Helps refine requirements
- **Project Manager (Role 12)** - Manages backlog, refines features, assigns work, tracks progress
- **All Delivery Roles** - Pick up user stories, move through statuses, update progress

## Tips

- **Keep features small** - Aim for features that can be completed in 1-2 weeks
- **Keep user stories smaller** - Each story should be completable in 1-3 days
- **Update assignments.md daily** - Keep the team informed
- **Use git commits** - Every move through statuses should be a commit with a meaningful message
- **Review blocked items weekly** - Don't let blockers accumulate
- **Archive done features quarterly** - Keep the done/ folder manageable

## Example Feature Lifecycle

1. Idea captured in `backlog/better-search.md`
2. Project Manager refines into `features/todo/00001-search-improvements/feature.md`
3. Feature broken into 3 user stories in `features/todo/00001-search-improvements/user-stories/todo/` (IDs: 00042, 00043, 00044)
4. Feature moved to `features/in-progress/00001-search-improvements/`
5. Stories worked through: todo → in-progress → done (with audit log entries for each transition)
6. When all stories done, feature moved to `features/done/00001-search-improvements/`
7. All history preserved in work item audit logs and `recently-changed.md`

## Getting Started

1. **Read the comprehensive instructions:** [`/docs/work-tracking-instructions.md`](../work-tracking-instructions.md)
2. Check `assignments.md` for current work
3. Review `recently-changed.md` for recent activity
4. Review `features/todo/` for available features
5. Review `features/in-progress/` to see what's being worked on
6. Add ideas to `backlog/` anytime
7. Coordinate with Project Manager for assignments

---

**📖 For complete instructions, see [`/docs/work-tracking-instructions.md`](../work-tracking-instructions.md)**

**Remember: Iterate and release. Ship small increments frequently to gather feedback and reduce risk.**
