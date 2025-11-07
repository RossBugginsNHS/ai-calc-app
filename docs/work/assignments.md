# Current Assignments

> **Last Updated:** [YYYY-MM-DD HH:MM]
>
> This file tracks who is currently working on what. Update whenever assignments change.

## Active Assignments

<!-- Format: - **@username** - [Feature 00000] [Story 00000] - [story-title] (status) -->

### In Progress

_No assignments currently in progress_

### Blocked

_No blocked assignments_

---

## Instructions

### Assigning Work

```bash
# 1. Move story to in-progress status folder
git mv docs/work/features/[status]/00001-feature-name/user-stories/todo/00042-story-name.md \
       docs/work/features/[status]/00001-feature-name/user-stories/in-progress/00042-story-name.md

# 2. Update the story file's audit log
# Add entry to "Assignment Changes" table:
# | [YYYY-MM-DD HH:MM] | Unassigned | @yourname | Picked up from backlog |

# 3. Update this assignments.md file
# Add: - **@yourname** - [Feature 00001] [Story 00042] - story-title (in-progress)

# 4. Update recently-changed.md
# See recently-changed.md for format

# 5. Commit everything together
git add docs/work/
git commit -m "Assign @yourname to story 00042"
```

### Completing Work

```bash
# 1. Move story to done status folder
git mv docs/work/features/[status]/00001-feature-name/user-stories/in-progress/00042-story-name.md \
       docs/work/features/[status]/00001-feature-name/user-stories/done/00042-story-name.md

# 2. Update story audit log
# Add to "Status Changes": | [YYYY-MM-DD HH:MM] | in-progress | done | @yourname | Completed |
# Add to "Updates": | [YYYY-MM-DD HH:MM] | Story completed | @yourname |

# 3. Remove from this assignments.md file

# 4. Update recently-changed.md

# 5. Commit
git add docs/work/
git commit -m "Complete story 00042: story-title"
```

---

## Tips

- **One story at a time** - Focus on completing before starting new work
- **Update immediately** - When status changes, update files immediately
- **Communicate blockers** - Move to blocked/ status and document in story audit log
- **Reference by ID** - Always use numeric IDs (00001, 00042) for clarity
