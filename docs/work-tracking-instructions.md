# Work Tracking Instructions for AI Agents

> **Critical:** Read this BEFORE using the work tracking system.

---

## When to Use Work Tracking

### ✅ USE Work Tracking When:

1. **Working on a CLIENT PROJECT** using the ConceptShipAI framework
   - You're acting as a ConceptShipAI role (0-19) to plan/design/build a software system for a customer
   - The customer has agreed to use the work tracking system
   - You're managing features and user stories for THEIR project
   
2. **Managing Ongoing Delivery Work**
   - Breaking down features into implementable user stories
   - Tracking progress across a team
   - Coordinating assignments and status
   - Managing backlogs and refinement

3. **When Explicitly Instructed**
   - Customer says "let's use the work tracking system"
   - Project Manager role asks you to create/update work items
   - You're asked to "track this in the work folder"

### ❌ DO NOT Use Work Tracking When:

1. **Developing ConceptShipAI Itself** (Meta-Development)
   - Working on ConceptShipAI templates, roles, documentation
   - Adding features to the ConceptShipAI framework
   - This repository's own development
   - Use handovers/ folder and CHANGELOG.md instead

2. **Quick Consultations or Planning**
   - Single-session conversations
   - Exploratory discussions
   - Proof-of-concepts or demos
   - Architecture reviews without implementation tracking

3. **Documentation-Only Work**
   - Writing docs, guides, READMEs
   - Creating role definitions
   - Planning artifacts without implementation

4. **When Not Explicitly Requested**
   - Customer hasn't mentioned work tracking
   - You're just brainstorming ideas
   - Doing research or discovery

---

## How to Use Work Tracking

### 1. Create Features

When the Project Manager (Role 12) or Customer (Role 0) identifies a feature:

```bash
# 1. Determine next feature ID
# Check docs/work/features/*/ for highest ID, add 1

# 2. Create feature folder with ID
mkdir -p docs/work/features/todo/00001-feature-name/user-stories/{todo,in-progress,done,blocked}

# 3. Create feature.md from template
cp docs/work/templates/feature.md docs/work/features/todo/00001-feature-name/feature.md

# 4. Fill out feature details
# - Update ID, name, description, business value, goals, acceptance criteria
# - Initialize audit log with creation entry

# 5. Commit
git add docs/work/features/todo/00001-feature-name/
git commit -m "Create feature 00001: feature-name"

# 6. Update recently-changed.md
# Add entry: | [YYYY-MM-DD HH:MM] | 00001 | Feature | Created | @username |
```

### 2. Create User Stories

When breaking down a feature into implementable work:

```bash
# 1. Determine next story ID
# Stories have unique IDs across ALL features, not per-feature
# Check all user-stories folders for highest ID, add 1

# 2. Create story from template
cp docs/work/templates/user-story.md \
   docs/work/features/todo/00001-feature-name/user-stories/todo/00042-story-name.md

# 3. Fill out story details
# - Update ID, Feature ID (parent), story description, acceptance criteria
# - Initialize audit log with creation entry

# 4. Update feature.md
# Add story to "Story List" section: 42. [Story Name] - todo

# 5. Commit
git add docs/work/features/todo/00001-feature-name/
git commit -m "Add story 00042 to feature 00001: story-name"

# 6. Update recently-changed.md
```

### 3. Assign Work

When assigning a story to someone (or yourself):

```bash
# 1. Update story file
# - Change "Assigned To" metadata
# - Add entry to "Assignment Changes" audit log

# 2. Update assignments.md
# Add: - **@username** - [Feature 00001] [Story 00042] - story-title (todo)

# 3. Commit
git add docs/work/assignments.md docs/work/features/.../00042-story-name.md
git commit -m "Assign story 00042 to @username"

# 4. Update recently-changed.md
```

### 4. Start Work

When beginning work on an assigned story:

```bash
# 1. Move story to in-progress
git mv docs/work/features/[status]/00001-feature-name/user-stories/todo/00042-story-name.md \
       docs/work/features/[status]/00001-feature-name/user-stories/in-progress/00042-story-name.md

# 2. Update story audit log
# Add to "Status Changes": | [timestamp] | todo | in-progress | @username | Starting work |

# 3. Update assignments.md
# Change status from (todo) to (in-progress)

# 4. If feature not yet in-progress, move feature too
git mv docs/work/features/todo/00001-feature-name docs/work/features/in-progress/00001-feature-name

# 5. Update feature audit log if moved

# 6. Commit
git add docs/work/
git commit -m "Start work on story 00042"

# 7. Update recently-changed.md
```

### 5. Complete Work

When finishing a story:

```bash
# 1. Move story to done
git mv docs/work/features/[status]/00001-feature-name/user-stories/in-progress/00042-story-name.md \
       docs/work/features/[status]/00001-feature-name/user-stories/done/00042-story-name.md

# 2. Update story audit log
# Add to "Status Changes": | [timestamp] | in-progress | done | @username | Completed |
# Add to "Updates": | [timestamp] | All acceptance criteria met | @username |

# 3. Update assignments.md
# Remove the assignment (no longer active work)

# 4. Update feature.md
# Update story list status: 42. [Story Name] - done
# Update summary counts

# 5. Check if feature is complete
# If all stories done, move feature to done
git mv docs/work/features/in-progress/00001-feature-name docs/work/features/done/00001-feature-name

# 6. Commit
git add docs/work/
git commit -m "Complete story 00042: story-name"

# 7. Update recently-changed.md
```

### 6. Handle Blockers

When work gets blocked:

```bash
# 1. Move to blocked status
git mv docs/work/features/[status]/00001-feature-name/user-stories/in-progress/00042-story-name.md \
       docs/work/features/[status]/00001-feature-name/user-stories/blocked/00042-story-name.md

# 2. Update story audit log
# Add to "Status Changes": | [timestamp] | in-progress | blocked | @username | Blocked by... |
# Add to "Blocker Log": | [timestamp] | Blocked | [description] | High | @owner | [details] |

# 3. Update "Blockers" section in story
# Fill out blocker table with details

# 4. Update assignments.md
# Move to "Blocked" section

# 5. Commit
git add docs/work/
git commit -m "Block story 00042: blocker description"

# 6. Update recently-changed.md

# When unblocked:
# Follow same process but: blocked → in-progress
# Add "Unblocked" entry to blocker log
```

### 7. Update Work Items

When making significant changes to work items:

```bash
# 1. Edit the work item (feature.md or story.md)
# Make your changes to description, acceptance criteria, etc.

# 2. Add entry to audit log "Updates" section
# | [timestamp] | [description of change] | @username |

# 3. Commit
git add docs/work/features/.../story.md
git commit -m "Update story 00042: description of change"

# 4. Update recently-changed.md
# | [timestamp] | 00042 | Update | [brief change] | @username |
```

---

## Unique ID System

### Format

- **Features:** `00001-short-kebab-case-name`
- **Stories:** `00042-short-kebab-case-name`

### Rules

1. **Globally Unique** - IDs are unique across ALL features and ALL stories
2. **Sequential** - Increment from highest existing ID
3. **Never Reuse** - Even if deleted, never reuse an ID
4. **5 Digits** - Pad with zeros (00001, 00042, 00123, 01234)
5. **Kebab Case Name** - Short descriptive name (3-5 words max)

### Finding Next ID

```bash
# Find highest feature ID
ls -1 docs/work/features/*/ | grep -Eo '^[0-9]{5}' | sort -n | tail -1
# Add 1 for next feature ID

# Find highest story ID (across all features)
find docs/work/features -name "*.md" -type f | grep -Eo '/[0-9]{5}-' | grep -Eo '[0-9]{5}' | sort -n | tail -1
# Add 1 for next story ID
```

### Referencing Work Items

Always use numeric ID when referencing:
- ✅ "Story 00042"
- ✅ "Feature 00001"  
- ✅ "See 00042 for details"
- ❌ "The authentication story"
- ❌ "The login feature"

---

## Audit Logs (Append-Only)

### Critical Rules

1. **NEVER edit or delete audit log entries**
2. **ALWAYS add new entries at the bottom** of each table
3. **Include timestamp** in format `YYYY-MM-DD HH:MM`
4. **Be specific** about what changed and why
5. **Record EVERY status change, assignment change, and blocker**

### Audit Log Sections

Every work item has these audit log sections:

#### Status Changes
Records every status transition:
```markdown
| Timestamp | From | To | By | Notes |
|-----------|------|-----|-----|-------|
| 2025-11-07 09:00 | - | todo | @pm | Feature created |
| 2025-11-07 10:30 | todo | in-progress | @pm | Starting work |
| 2025-11-08 16:00 | in-progress | done | @alice | Completed |
```

#### Assignment Changes
Records every assignment or re-assignment:
```markdown
| Timestamp | From | To | Reason |
|-----------|------|-----|--------|
| 2025-11-07 10:00 | Unassigned | @alice | Initial assignment |
| 2025-11-08 09:00 | @alice | @bob | Alice out sick |
```

#### Blocker Log (Stories Only)
Records blockers added and resolved:
```markdown
| Timestamp | Action | Blocker | Impact | Owner | Notes |
|-----------|--------|---------|--------|-------|-------|
| 2025-11-07 14:00 | Blocked | API not ready | High | @api-team | Waiting for endpoint |
| 2025-11-08 10:00 | Unblocked | API not ready | - | @api-team | Endpoint deployed |
```

#### Updates
Records significant changes to work item:
```markdown
| Timestamp | Change | By |
|-----------|--------|-----|
| 2025-11-07 09:00 | Story created | @pm |
| 2025-11-07 15:00 | Added acceptance criteria for mobile | @ux |
| 2025-11-08 11:00 | Story completed | @alice |
```

---

## File Management

### Locations

```
docs/work/
├── README.md                    # Overview and workflow
├── assignments.md               # Current active assignments
├── recently-changed.md          # Last 30 days of activity
├── backlog/                     # Unrefined ideas
│   ├── README.md
│   └── [rough-ideas].md
├── features/
│   ├── todo/                    # Features not started
│   │   └── 00001-feature-name/
│   │       ├── feature.md
│   │       └── user-stories/
│   │           ├── todo/
│   │           ├── in-progress/
│   │           ├── done/
│   │           └── blocked/
│   ├── in-progress/             # Active features
│   ├── done/                    # Completed features
│   └── blocked/                 # Blocked features
└── templates/
    ├── feature.md
    └── user-story.md
```

### Naming Conventions

- **Feature folders:** `00001-short-descriptive-name/`
- **Story files:** `00042-short-descriptive-name.md`
- **Use kebab-case** (hyphens between words)
- **Be concise** (3-5 words in name)
- **Be descriptive** (name should convey purpose)

---

## Handover Integration

### Lean Handovers

When creating handover files, **reference work items instead of duplicating content**:

```markdown
# Handover: Business Analyst → Requirements Engineer

## Context
Feature 00001 (user-authentication) has been refined and is ready for detailed requirements.

## Completed Work
- Feature 00001 created with 4 user stories (see docs/work/features/todo/00001-user-authentication/)
- Story 00042: Login with email
- Story 00043: Login with social providers
- Story 00044: Password reset flow
- Story 00045: Remember me functionality

## Key Decisions
See feature 00001 audit log for decision history.

## Next Steps
Requirements Engineer should:
1. Review feature 00001 acceptance criteria
2. Create detailed requirements for stories 00042-00045
3. Update story acceptance criteria as needed

## Questions/Blockers
None - all stories are well-defined and ready for requirements engineering.
```

### What NOT to Put in Handovers

- ❌ Full feature descriptions (link to feature ID)
- ❌ Complete story lists (link to feature folder)
- ❌ Change history (it's in work item audit logs)
- ❌ Assignment details (it's in assignments.md)

### What to Put in Handovers

- ✅ Context for this specific handover
- ✅ Feature/story IDs being handed over
- ✅ Key decisions or constraints for next role
- ✅ Questions or blockers specific to handover
- ✅ What the next role should do first

---

## Integration with Other AgentMD Files

### History Folder

- **Do NOT duplicate work tracking in history/**
- History folder is for AgentMD's OWN development
- Client projects use work items as their history

### Handovers Folder

- **Reference work items** from handovers
- Keep handovers lean and focused
- Full details live in work items

### Outputs Folder

- Work items are separate from outputs (artifacts)
- Outputs contain deliverables (specs, designs, docs)
- Work items track the process of creating outputs

---

## Quick Reference

### Common Commands

```bash
# Create feature
mkdir -p docs/work/features/todo/00001-name/user-stories/{todo,in-progress,done,blocked}
cp docs/work/templates/feature.md docs/work/features/todo/00001-name/feature.md

# Create story
cp docs/work/templates/user-story.md docs/work/features/.../user-stories/todo/00042-name.md

# Move to in-progress
git mv docs/work/features/todo/00001-name docs/work/features/in-progress/00001-name

# Complete story
git mv docs/work/features/.../user-stories/in-progress/00042-name.md \
       docs/work/features/.../user-stories/done/00042-name.md

# Find next ID
find docs/work/features -name "[0-9]*-*.md" | grep -Eo '[0-9]{5}' | sort -n | tail -1
```

### Decision Tree

```
Is this ConceptShipAI's own development?
├─ YES → Use handovers/, CHANGELOG.md, NOT work tracking
└─ NO → Is this a client project using ConceptShipAI?
    ├─ YES → Is customer using work tracking?
    │   ├─ YES → Use docs/work/ system
    │   └─ NO → Just create artifacts in outputs/
    └─ NO → Don't use work tracking
```

---

## Remember

1. **Work tracking is FOR client projects, NOT ConceptShipAI development itself**
2. **Always use unique numeric IDs** for features and stories
3. **Audit logs are append-only** - never edit history
4. **Update recently-changed.md** with every work item change
5. **Keep handovers lean** - reference work items, don't duplicate
6. **Commit frequently** - every status change is a commit
7. **One story at a time** - focus and complete before starting new work

---

**When in doubt, ask yourself: "Am I working on a CLIENT project that needs delivery tracking, or am I working on ConceptShipAI itself?"**
