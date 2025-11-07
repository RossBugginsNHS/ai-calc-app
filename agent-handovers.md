# Agent Instructions: Handover Process

> **⚠️ READ-ONLY FILE**: This file contains instructions for handovers between roles and chat sessions.  
> **All customizations go in `/agent-custom.md`**

This file is automatically loaded by AI agents when creating or processing handovers between roles or chat sessions.

---

## Purpose

Handovers enable **continuity across chat windows** and **role transitions**. They solve the problem of context loss when:
- Human creates a new chat session (VS Code window closes, conversation limit reached)
- Large projects span multiple sessions
- Different roles need to coordinate

**Key Concept:** The file system is the source of truth, not conversation history.

---

## When to Create Handovers

### ✅ Create a handover when:

1. **Ending a chat session** before project completion
2. **Completing a planning role** and transitioning to another role
3. **Reaching conversation limits** in the current chat window
4. **Human requests** "create a handover" or "prepare for handover"
5. **Complex work needs pause** and will resume later
6. **Multiple roles** need to coordinate across sessions

### ❌ Do NOT create handover when:

1. **Simple role switch** in same conversation (just switch roles directly)
2. **Quick questions** to specialist roles (collaborate in same session)
3. **Work is complete** (final deliverables are done)
4. **Within same chat session** and continuing immediately

---

## State Persistence

### What Persists Across Chat Windows

**✅ These are preserved in the file system:**

| What | Where | Purpose |
|------|-------|---------|
| Handover instructions | `docs/handovers/handover.md` | Tells next chat which role to assume |
| Work assignments | `docs/work/assignments.md` | Shows active work for implementation roles |
| Work items | `docs/work/features/`, `docs/work/backlog/` | All features and stories with audit logs |
| Planning artifacts | `docs/artifacts/[role]/` | All outputs from planning roles |
| Project code | `projects/` | All implementation code and tests |
| History | `docs/history/` | Record of all interactions |
| Git commits | `.git/` | Complete change history |

**❌ These are NOT preserved:**

| What | Why | Solution |
|------|-----|----------|
| Conversation context | Chat window closes | Read handover file |
| Working memory | AI doesn't remember previous chat | Check audit logs |
| Unwritten decisions | Not documented | Write to handover |
| Draft ideas | Not committed | Commit before handover |

### How to Maintain Continuity

**When starting a new chat session:**

1. **Read handover file** (`docs/handovers/handover.md`) - contains essential context
2. **Check work assignments** (`docs/work/assignments.md`) - shows active work
3. **Review recent audit logs** in work items - see what's been happening
4. **Check git log** if needed: `git log --oneline -10`
5. **Read referenced artifacts** - mentioned in handover

---

## Handover File Location

### Active Handover

**Location:** `docs/handovers/handover.md`

- **Only ONE handover file exists at a time**
- This is the "current" handover for the next chat session
- Must be processed before creating a new one

### Handover History

**Location:** `docs/handovers/handover-histories/[yyyyMMdd-HHmm]-handover.md`

- Archive of all previous handovers
- Timestamped in UTC format
- Useful for understanding project timeline
- Never deleted (part of project history)

---

## Creating a Handover

### Step 1: Determine Next Role

**Identify what role should continue the work:**

- If in planning phase: Which planning role is next?
- If in delivery phase: Which implementation role should work on the assigned story?
- If blocked: Which specialist role can unblock?

**Examples:**
- Business Analyst → Requirements Engineer
- Requirements Engineer → System Architect
- Delivery Manager → Frontend Developer (with story assignment)
- Frontend Developer → Frontend Developer (continue work in new session)

### Step 2: Create Handover File

**Use `create_file` tool** to create `docs/handovers/handover.md`:

```markdown
# Handover: [Current Role] → [Next Role]

**Date**: [YYYY-MM-DD HH:mm UTC]  
**Current Role**: [Role Number and Name]  
**Next Role**: [Next Role Number and Name]

---

## Work Completed

[Concise summary of what was accomplished - 2-3 paragraphs]

### Artifacts Created

- `docs/artifacts/[role]/[artifact].md` - [Brief description of content and purpose]
- `docs/artifacts/[role]/[artifact2].md` - [Brief description]

[List all new or updated files]

## Key Decisions Made

1. **[Decision Topic]**: [What was decided and why]
2. **[Decision Topic]**: [What was decided and why]
3. [Continue for all significant decisions]

[Include technology choices, architectural decisions, scope changes]

## Next Steps

[Clear action items for the next role - specific and actionable]

### Inputs Available for Next Role

[List artifacts the next role should read]

- `docs/artifacts/[role]/[file].md` - [Why this is relevant]
- `docs/requirements/[file].md` - [Why this is relevant]

### Expected Outputs from Next Role

[What the next role should create]

- **[Artifact Name]** - [Purpose and key contents]
- **[Artifact Name]** - [Purpose and key contents]

## Work Items

[Reference any features or stories - use IDs if work tracking is active]

**Features:**
- Feature 00001: User Authentication - Status: todo, Location: docs/work/features/todo/00001-user-authentication/

**Stories:**
- Story 00042: Login form - Status: assigned to Frontend Developer

## Questions/Issues to Address

[Any open questions, blockers, or concerns]

1. **[Question/Issue]**: [Description and context]
2. **[Question/Issue]**: [Description and needed from whom]

## Notes

[Any additional context, reminders, or information]

- [Important note 1]
- [Important note 2]

## Technical Context

[If relevant - technology choices, frameworks, versions]

- Framework: [React 18, Python 3.11, etc.]
- Database: [PostgreSQL 15, MongoDB, etc.]
- Hosting: [AWS, Azure, Netlify, etc.]

---

**⚠️ CRITICAL**: The **Next Role** field at the top is essential - it tells the AI which role to assume when "carry on" is said.
```

### Step 3: Commit All Changes

**Before creating handover, ensure all work is committed:**

```bash
# Check status
git status

# Stage all changes
git add .

# Commit with clear message
git commit -m "Completed [Role Name] - [UTC timestamp yyyyMMdd-HHmm]

[Optional: Brief summary of what was completed]
"
```

**Why this matters:**
- Next chat session sees committed changes
- Git log provides audit trail
- No work is lost between sessions

### Step 4: Inform Human

**Announce handover is ready:**

```
I've completed the [Role Name] work and prepared a handover to [Next Role].

Handover file created: docs/handovers/handover.md

All changes have been committed to git.

To continue:
1. Create a new chat window
2. Say "carry on" or "continue"
3. I'll read the handover and assume the [Next Role] role

Would you like to continue now, or take a break?
```

---

## Processing a Handover

### When Human Says "Carry On"

**Follow this process EXACTLY:**

#### Step 1: Check for Handover File

```
Does `docs/handovers/handover.md` exist?
```

**If YES:**
- Proceed to Step 2

**If NO:**
- Check `docs/work/assignments.md` for active assignments
- If assignments exist, assume that implementation role
- If no assignments, ask human: "What would you like me to work on?"
- If starting fresh, assume Role 0 (Customer Intake)

#### Step 2: Read Handover File

```
Read docs/handovers/handover.md completely
```

**Extract:**
- **Next Role** field (this is critical!)
- Work completed summary
- Artifacts created
- Next steps
- Any questions or blockers

#### Step 3: Archive Handover

**Move handover to history:**

```bash
# Create timestamp
TIMESTAMP=$(date -u +"%Y%m%d-%H%M")

# Move handover to archive
mv docs/handovers/handover.md docs/handovers/handover-histories/${TIMESTAMP}-handover.md

# Commit the move
git add docs/handovers/
git commit -m "Archive handover - ${TIMESTAMP}"
```

**Why archive?**
- Only one active handover exists at a time
- History preserved for audit trail
- Prevents confusion about which handover is current

#### Step 4: Read Next Role's Files

**Load role context:**

1. Read `docs/roles/[role-number]-[role-name]/default.md`
2. Read `docs/roles/[role-number]-[role-name]/custom.md`
3. Note persona name if defined

#### Step 5: Introduce Yourself

**Greet the human as the new role:**

```
Hello! I'm now the [Role Name] ([Persona Name if defined]).

I've read the handover from [Previous Role]. Here's where we are:

[2-3 sentence summary of handover context]

Key points:
- [Point 1 from handover]
- [Point 2 from handover]

Next steps:
- [Action 1]
- [Action 2]

Ready to continue with [Role Name]? Do you have any questions before I proceed?
```

#### Step 6: Await Confirmation

**Wait for human to confirm before proceeding:**
- Human may have questions
- Human may want to adjust the plan
- Human may want to switch to a different role

#### Step 7: Proceed with Role's Work

**Begin executing the role's responsibilities:**
- Follow the role's default.md workflow
- Create artifacts as specified
- Reference handover's "Next Steps" section

---

## Handover Templates

### Planning Phase Handover

**For transitions between planning roles (0-19):**

```markdown
# Handover: [Current Role] → [Next Role]

**Date**: [YYYY-MM-DD HH:mm UTC]  
**Current Role**: [Role Number] - [Role Name]  
**Next Role**: [Role Number] - [Role Name]

---

## Work Completed

[Summary of planning work completed]

### Artifacts Created

- `docs/artifacts/[role]/[artifact].md` - [Description]

### Artifacts Updated

- `docs/requirements/[file].md` - [What changed]

## Key Decisions Made

1. **[Decision]**: [Details and rationale]

## Next Steps

[What the next planning role should do]

### Required Inputs

The next role should read:
- [Artifact 1] - [Why]
- [Artifact 2] - [Why]

### Expected Outputs

The next role should create:
- **[Artifact]** - [Purpose]

## Open Questions

1. [Question for next role to address]

## Notes

[Additional context]
```

### Delivery Phase Handover

**For implementation work handovers:**

```markdown
# Handover: Delivery Manager → [Implementation Role]

**Date**: [YYYY-MM-DD HH:mm UTC]  
**Current Role**: 12 - Delivery Manager  
**Next Role**: [20/21/22] - [Frontend/Backend/Full-Stack] Developer

---

## Work Completed

Sprint/iteration planning complete. Ready to begin implementation.

### Sprint Goals

- [Goal 1]
- [Goal 2]

## Work Items Assigned

**Story Assignments** (see docs/work/assignments.md):

| Story ID | Title | Priority | Assigned To |
|----------|-------|----------|-------------|
| 00042 | [Title] | High | [Implementation Role] |
| 00043 | [Title] | Medium | [Implementation Role] |

## Technical Context

- **Project Structure**: projects/[name]/
- **Tech Stack**: [React/Python/Go/etc.]
- **Initialized**: [Yes/No]

## Next Steps

1. Review assigned story 00042 in docs/work/features/[feature-id]/stories/00042-story.md
2. Follow TDD workflow: Write tests first, implement code, refactor
3. Update story audit log as work progresses
4. Create PR when story complete

## Resources

- **Requirements**: docs/artifacts/02-requirements-engineer/
- **Design**: docs/artifacts/05-ux-ui-designer/
- **API Specs**: docs/artifacts/07-api-designer/
- **Test Strategy**: docs/artifacts/09-test-architect/

## Notes

[Any specific guidance for implementation]
```

### Mid-Implementation Handover

**For continuing work across chat sessions:**

```markdown
# Handover: Frontend Developer → Frontend Developer

**Date**: [YYYY-MM-DD HH:mm UTC]  
**Current Role**: 20 - Frontend Developer  
**Next Role**: 20 - Frontend Developer

---

## Work Completed

Story 00042 in progress.

### Progress

- [x] Tests created (TDD RED)
- [x] Basic component implementation (TDD GREEN)
- [ ] API integration (in progress)
- [ ] Styling and responsive design
- [ ] E2E tests

### Files Created/Modified

- `projects/frontend/src/components/LoginForm.jsx`
- `projects/frontend/tests/LoginForm.test.jsx`

## Current Status

Working on API integration for LoginForm component. Authentication endpoint `/api/auth/login` is ready (tested with curl).

### Recent Audit Log Entries

```
[2025-11-07 14:30 UTC] STATUS: in-progress - Tests created (TDD RED)
[2025-11-07 14:45 UTC] STATUS: in-progress - Component implementation (TDD GREEN)
[2025-11-07 15:00 UTC] STATUS: in-progress - Starting API integration
```

## Next Steps

1. Complete API integration in LoginForm.jsx
2. Test with actual API endpoint (not mocked)
3. Add error handling for network failures
4. Add loading states
5. Create E2E test for full login flow
6. Update story audit log when complete

## Technical Notes

- API endpoint requires Bearer token (see docs/artifacts/07-api-designer/api-specifications.md)
- Use axios for HTTP requests (already installed)
- Error messages should match UX specs (docs/artifacts/05-ux-ui-designer/error-handling.md)

## Blockers

None currently.

## Notes

Component is working well in tests. Should be able to complete today once API integration is done.
```

---

## Handovers with Work Tracking

### Keep Handovers Lean

**Instead of duplicating work item details in handovers, reference them by ID:**

**❌ BAD (duplicates content):**
```markdown
## User Stories

1. As a user, I want to login with email/password so I can access my account
   - Acceptance Criteria:
     - Email validation works
     - Password is required
     - Error messages are clear
   [... full story details ...]
```

**✅ GOOD (references by ID):**
```markdown
## Work Items

**Features:**
- Feature 00001: User Authentication - Status: in-progress

**Stories:**
- Story 00042: Login form UI - Assigned to Frontend Developer
- Story 00043: Auth API endpoint - Assigned to Backend Developer

See docs/work/features/in-progress/00001-user-authentication/ for full details.
```

### Example: Lean Handover with Work Tracking

```markdown
# Handover: Business Analyst → Requirements Engineer

**Date**: 2025-11-07 14:30 UTC  
**Current Role**: 1 - Business Analyst  
**Next Role**: 2 - Requirements Engineer

---

## Work Completed

Feature 00001 (user-authentication) has been refined and broken into 5 user stories.

### Work Items Created

- **Feature 00001**: User Authentication System
  - Location: docs/work/features/todo/00001-user-authentication/
  - Stories: 00042, 00043, 00044, 00045, 00046
  - Status: Ready for detailed requirements

## Key Decisions Made

1. **Authentication Method**: Email/password primary, social login secondary (future)
2. **Password Reset**: Email-based reset flow
3. **Session Management**: JWT tokens, 24-hour expiry

## Next Steps

Requirements Engineer should:

1. Review feature 00001 in docs/work/features/todo/00001-user-authentication/feature.md
2. Review all 5 user stories (00042-00046)
3. Create detailed functional requirements based on stories
4. Link requirements to story IDs in requirements traceability matrix
5. Update story audit logs when requirements are complete

### Expected Outputs

- Functional requirements for authentication (FR-040 series)
- Non-functional requirements (NFR-SEC series for security)
- Updated requirements traceability matrix

## Notes

Stories are well-defined with clear acceptance criteria. Should be straightforward to convert to detailed requirements.
```

---

## Common Scenarios

### Scenario 1: Planning Role Transition

**Context:** Business Analyst finishing, Requirements Engineer next

**Process:**
1. Business Analyst creates handover with Next Role: Requirements Engineer
2. Commits all artifacts
3. Informs human
4. Human creates new chat, says "carry on"
5. AI reads handover, archives it
6. AI assumes Requirements Engineer role
7. AI introduces itself and summarizes handover
8. AI begins requirements engineering work

### Scenario 2: Starting Implementation Phase

**Context:** Planning complete, ready for code

**Process:**
1. Delivery Manager creates handover with Next Role: Frontend Developer
2. Handover references story 00042 in assignments.md
3. Commits everything
4. Human creates new chat, says "carry on"
5. AI reads handover, sees Frontend Developer assignment
6. AI assumes Frontend Developer role
7. AI reads story 00042 from work tracking
8. AI begins TDD workflow

### Scenario 3: Mid-Story Continuation

**Context:** Frontend Developer in middle of story, needs new chat

**Process:**
1. Frontend Developer updates story audit log with current progress
2. Commits code and tests created so far
3. Creates handover to Frontend Developer (same role)
4. Human creates new chat, says "carry on"
5. AI reads handover, sees Frontend Developer continuing
6. AI reviews recent audit log entries
7. AI checks what files were created
8. AI continues from where it left off

### Scenario 4: Blocker Needs Specialist

**Context:** Frontend Developer blocked, needs API Designer

**Process:**
1. Frontend Developer adds BLOCKER to story audit log
2. Creates handover to API Designer with specific question
3. Human creates new chat (or same chat), says "carry on" or "switch to API Designer"
4. AI assumes API Designer role
5. API Designer addresses blocker
6. API Designer creates handover back to Frontend Developer
7. Frontend Developer continues

---

## Best Practices

### Do's ✅

1. **Always specify Next Role** - Critical for "carry on" command
2. **Commit before handover** - All changes must be in git
3. **Reference work item IDs** - Keep handovers lean
4. **Be specific about next steps** - Clear action items
5. **Archive after reading** - Move to handover-histories/
6. **Include technical context** - Versions, frameworks, decisions
7. **Note any blockers** - Surface issues early

### Don'ts ❌

1. **Don't duplicate work item content** - Reference by ID instead
2. **Don't create handover for simple role switches** - Just switch directly in same chat
3. **Don't leave uncommitted changes** - Commit before handover
4. **Don't assume context carries over** - New chat = clean slate, must read files
5. **Don't skip archive step** - Always move to handover-histories/
6. **Don't forget timestamps** - Always use UTC in yyyyMMdd-HHmm format

---

## Troubleshooting

### "I said carry on but AI didn't read handover"

**Solution:** AI should check for `docs/handovers/handover.md` first. If it exists, read it. If AI didn't, the handover file might not exist or AI skipped the check.

### "Handover has wrong next role"

**Solution:** Human can override. Say "Actually, switch to [Role Name] instead" and AI will assume that role.

### "AI doesn't remember what we discussed"

**Expected:** Conversation context doesn't persist. **Solution:** All important information must be in files (handover, audit logs, artifacts). AI reads files to understand context.

### "Multiple handovers exist"

**Problem:** Only one `handovers/handover.md` should exist at a time.

**Solution:** 
1. Determine which is most recent (check Date field)
2. Archive old ones to handover-histories/
3. Process the current one

### "Handover missing critical information"

**Solution:** Add notes to story audit logs or create a new artifact. Don't rely solely on handover for all context.

---

## See Also

- **[agent.md](./agent.md)** - Core orchestration and role system
- **[agent-project-structure.md](./agent-project-structure.md)** - Project structure and code implementation
- **[agent-work-tracking.md](./agent-work-tracking.md)** - Work tracking, features, stories, assignments
- **[docs/handovers/README.md](./docs/handovers/README.md)** - Additional handover documentation

---

**Remember**: Handovers are the bridge between chat sessions. They preserve continuity when conversation context is lost. The file system is the source of truth.
