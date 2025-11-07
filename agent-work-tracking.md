# Agent Instructions: Work Tracking System

> **⚠️ READ-ONLY FILE**: This file contains instructions for the work tracking system.  
> **All customizations go in `/agent-custom.md`**

This file is automatically loaded by AI agents when working with features, stories, and assignments during the delivery phase.

---

## Overview

The work tracking system provides **structured feature and story management** for implementation work. It is designed for iterative delivery with clear traceability and audit trails.

**Key Capabilities:**
- Unique numeric IDs for all work items
- Append-only audit logs
- Clear assignment tracking
- Lean handovers (reference by ID, not duplication)
- Recent activity tracking

---

## When to Use Work Tracking

### ✅ USE for Client Projects - Starting from Planning Phase

**Use work tracking when:**
- Helping a customer plan/build THEIR software project using ConceptShipAI
- The customer wants to track features, user stories, and assignments
- Working on ongoing delivery that needs progress tracking
- Managing multiple implementation roles (20-22)
- Tracking work across multiple sprints/iterations

**🔴 CRITICAL: Work tracking starts DURING PLANNING, not just during delivery**

**Planning roles (1-11) should create features and stories as they work:**
- **Role 1 (Business Analyst)**: Identify initial features from business requirements
- **Role 2 (Requirements Engineer)**: Break features into user stories with acceptance criteria
- **Role 3-11 (Specialist roles)**: Refine features/stories, add technical details
- **Role 12 (Delivery Manager)**: Prioritize and assign stories for implementation

**By the time planning is complete, `docs/work/` should contain:**
- Features in `docs/work/features/todo/` ready for implementation
- User stories with acceptance criteria
- Clear priorities and dependencies

### ❌ DO NOT USE for ConceptShipAI Development

**Do NOT use work tracking when:**
- Working on the ConceptShipAI template itself
- Building/improving this framework
- Meta-development tasks

**For ConceptShipAI development**, use:
- `DEVELOPMENT-TRACKER.md` - Template development tracking
- Direct file editing and git commits
- Issue tracking in the repository

---

## System Structure

### File Organization

```
docs/work/
├── README.md                        # Work tracking overview
├── assignments.md                   # Current active assignments
├── recently-changed.md              # Last 30 days of activity
├── templates/                       # Feature and story templates
│   ├── feature-template.md
│   └── story-template.md
├── backlog/                         # Future work items
│   └── [id]-[name].md
└── features/                        # Active and completed features
    ├── todo/                        # Not started
    │   └── [id]-[name]/
    │       ├── feature.md
    │       └── stories/
    │           ├── [id]-story.md
    │           └── ...
    ├── in-progress/                 # Currently being worked on
    │   └── [id]-[name]/
    └── done/                        # Completed
        └── [id]-[name]/
```

### ID Format

**All work items use unique numeric IDs:**

- Format: `00001`, `00042`, `00123`
- 5 digits, zero-padded
- Sequential (though gaps are okay)
- Globally unique across features and stories

**File naming:**
- Features: `[id]-[kebab-case-name]/`
- Stories: `[id]-story.md`

**Examples:**
- `00001-user-authentication/` - Feature folder
- `00042-story.md` - Story file
- `00123-shopping-cart/` - Another feature

---

## Work Item Lifecycle

### Feature Lifecycle

```
backlog/ → features/todo/ → features/in-progress/ → features/done/
```

**Transitions:**
1. **Created in backlog** - Initial idea, needs refinement
2. **Moved to features/todo** - Ready for work, stories defined
3. **Moved to features/in-progress** - At least one story in progress
4. **Moved to features/done** - All stories complete

### Story Lifecycle

**Stories live inside feature folders:**

```
features/[status]/[feature-id]/stories/[story-id]-story.md
```

**Status tracked in story file audit log:**
- `backlog` - Not ready for implementation
- `todo` - Ready to be assigned
- `assigned` - Assigned to implementation role
- `in-progress` - Being worked on
- `in-review` - Code review, QA
- `done` - Complete, deployed, validated
- `blocked` - Cannot proceed (waiting on something)

---

## Key Files

### `docs/work/assignments.md`

**Current active assignments for implementation roles.**

**Format:**
```markdown
# Active Assignments

Last Updated: [YYYY-MM-DD HH:mm UTC]

## Current Sprint/Iteration

| Story ID | Story Title | Assigned To | Status | Started | Notes |
|----------|-------------|-------------|--------|---------|-------|
| 00042 | User profile page | Frontend Developer (Role 20) | in-progress | 2025-11-07 | PR #123 |
| 00043 | Auth API endpoint | Backend Developer (Role 21) | in-progress | 2025-11-07 | |
| 00044 | Database migration | Backend Developer (Role 21) | assigned | 2025-11-07 | Waiting on 00043 |

## Completed This Sprint

| Story ID | Story Title | Assigned To | Completed | Notes |
|----------|-------------|-------------|-----------|-------|
| 00041 | Login form component | Frontend Developer (Role 20) | 2025-11-06 | Deployed to test |
```

**Updated by:**
- Delivery Manager (Role 12) when assigning work
- Implementation roles (20-22) when updating status

### `docs/work/recently-changed.md`

**Last 30 days of activity across all work items.**

**Format:**
```markdown
# Recently Changed Work Items

Generated: [YYYY-MM-DD HH:mm UTC]
Shows work items with activity in the last 30 days.

## [YYYY-MM-DD]

### Feature 00001: User Authentication
- Story 00042: STATUS changed to `done` by Frontend Developer
- Story 00043: BLOCKER added - Waiting on API specs

### Feature 00005: Shopping Cart
- Story 00067: ASSIGNED to Backend Developer
- Story 00068: STATUS changed to `in-progress`
```

**Auto-generated by:**
- Delivery Manager when updating assignments
- Can be manually maintained if preferred

---

## Role Responsibilities

### Planning Roles (1-11) - Creating Features and Stories

**🔴 CRITICAL: Planning roles should populate `docs/work/` during the planning phase**

#### Role 1: Business Analyst

**Work tracking responsibilities:**

1. **Identify Initial Features**
   - Review business requirements and identify high-level features
   - Create feature files in `docs/work/backlog/` or `docs/work/features/todo/`
   - Format: `[id]-[kebab-case-name]/feature.md`
   - Link features to business requirements (BR-xxx)

2. **Feature Content**
   - Feature title and description
   - Business value and objectives
   - Affected stakeholders
   - Success criteria
   - Initial priority estimate

**Example**: After creating business requirements, create:
- `docs/work/features/todo/00001-user-authentication/feature.md`
- `docs/work/features/todo/00002-user-profile-management/feature.md`

#### Role 2: Requirements Engineer

**Work tracking responsibilities:**

1. **Break Features into User Stories**
   - For each feature in `docs/work/features/todo/`, create user stories
   - Location: `docs/work/features/todo/[feature-id]/stories/[story-id]-story.md`
   - Link stories to functional requirements (FR-xxx)

2. **Story Content**
   - User story format: "As a [user], I want [goal], so that [benefit]"
   - Acceptance criteria (testable)
   - Dependencies on other stories
   - Priority and estimated complexity

**Example**: For feature 00001, create:
- `docs/work/features/todo/00001-user-authentication/stories/00042-story.md` (login form)
- `docs/work/features/todo/00001-user-authentication/stories/00043-story.md` (password reset)

#### Roles 3-11: Specialist Roles

**Work tracking responsibilities:**

1. **Refine Existing Features/Stories**
   - Add specialist perspective to stories
   - Add technical details, security considerations, API specs, etc.
   - Update audit logs with refinements

2. **Create Additional Stories**
   - If specialist work reveals new stories, create them
   - Example: Security Architect adds "implement OAuth 2.0" story

**All specialist roles should reference work items in their artifacts:**
- System Architect: Link architecture decisions to features
- Security Architect: Link security controls to stories
- API Designer: Link API endpoints to stories
- UX Designer: Link wireframes/mockups to stories

### Delivery Manager (Role 12)

**Primary work tracking responsibilities:**

1. **Backlog Management**
   - Review and refine features created by planning roles
   - Move features between backlog/todo/in-progress/done
   - Prioritize features

2. **Story Refinement**
   - Review stories created by Requirements Engineer
   - Add estimates, priorities, dependencies
   - Ensure stories have clear acceptance criteria

3. **Assignment Management**
   - Update `docs/work/assignments.md`
   - Assign stories to implementation roles (20-22)
   - Track progress
   - Remove blockers

4. **Activity Tracking**
   - Maintain `docs/work/recently-changed.md`
   - Review story audit logs
   - Report progress to customer

5. **Feature Transitions**
   - Move features between todo/in-progress/done folders
   - Close completed features
   - Archive as needed

### Implementation Roles (20-22)

**Work tracking responsibilities:**

1. **Check Assignments**
   - Read `docs/work/assignments.md` for assigned stories
   - If "carry on" command given, check for assigned work

2. **Read Story Details**
   - Open assigned story file: `docs/work/features/[status]/[feature-id]/stories/[story-id]-story.md`
   - Review acceptance criteria
   - Check dependencies and blockers

3. **Update Audit Log**
   - Add entries to story audit log as work progresses
   - Format: `[YYYY-MM-DD HH:mm UTC] STATUS: [status] - [description]`
   - Include file paths, PR numbers, test results

4. **Report Blockers**
   - Add BLOCKER entries to audit log
   - Notify Delivery Manager
   - Provide details on what's blocking progress

5. **Complete Stories**
   - Add final audit log entry with completion details
   - Notify Delivery Manager
   - Update any related documentation

### All Roles

**Can contribute to work tracking:**

1. **Create Backlog Items**
   - Any role can add ideas to `docs/work/backlog/`
   - Use format: `[next-id]-[descriptive-name].md`
   - Add basic description and rationale

2. **Reference Work Items**
   - Use work item IDs in artifacts, handovers, history
   - Link planning artifacts to features/stories
   - Maintain traceability

3. **Review Work Items**
   - Read work items to understand context
   - Provide input on requirements, design, security, etc.

---

## Role Assignment During Delivery

### Assignment Process

**1. Delivery Manager Creates/Refines Stories**

In `docs/work/features/todo/[feature-id]/stories/`:

```markdown
# Story 00042: Implement User Profile Page

**Feature**: 00015-user-management
**Status**: todo
**Assigned To**: Unassigned
**Estimate**: Medium (4-8 hours)
**Priority**: High

## Description
Create a user profile page where users can view and edit their profile information.

## Acceptance Criteria
- [ ] Profile page displays user name, email, bio
- [ ] User can edit name and bio
- [ ] Changes are saved to backend API
- [ ] Form validation shows clear error messages
- [ ] Page is responsive on mobile/tablet/desktop

## Dependencies
- Story 00041 (Login) must be complete
- API endpoint /api/users/:id from Story 00043

## Technical Notes
- Use ProfilePage component in projects/frontend/src/pages/
- Connect to /api/users/:id endpoint
- Follow design from docs/artifacts/05-ux-ui-designer/

## Audit Log
[2025-11-07 10:00 UTC] CREATED by Delivery Manager
```

**2. Delivery Manager Assigns Story**

Update `docs/work/assignments.md`:

```markdown
| Story ID | Story Title | Assigned To | Status | Started | Notes |
|----------|-------------|-------------|--------|---------|-------|
| 00042 | User profile page | Frontend Developer (Role 20) | assigned | 2025-11-07 | Dependencies met |
```

Add to story audit log:

```markdown
[2025-11-07 14:00 UTC] ASSIGNMENT: Assigned to Frontend Developer (Role 20)
```

**3. Implementation Role Reads Assignment**

When Frontend Developer assumes role:
1. Read `docs/work/assignments.md`
2. See story 00042 is assigned
3. Open `docs/work/features/todo/00015-user-management/stories/00042-story.md`
4. Review acceptance criteria and technical notes

**4. Implementation Role Works on Story**

Add audit log entries as work progresses:

```markdown
[2025-11-07 14:15 UTC] STATUS: in-progress - Started implementation
[2025-11-07 14:30 UTC] STATUS: in-progress - Tests created at projects/frontend/tests/ProfilePage.test.jsx (TDD RED)
[2025-11-07 14:45 UTC] STATUS: in-progress - Component created at projects/frontend/src/pages/ProfilePage.jsx (TDD GREEN)
[2025-11-07 15:00 UTC] STATUS: in-progress - API integration complete
[2025-11-07 15:15 UTC] STATUS: in-review - PR #123 created, all tests passing
```

**5. Implementation Role Completes Work**

Add final audit log entry:

```markdown
[2025-11-07 16:00 UTC] STATUS: done - PR merged, deployed to test environment
Files created:
- projects/frontend/src/pages/ProfilePage.jsx
- projects/frontend/tests/ProfilePage.test.jsx
- projects/frontend/src/api/users.js
Test coverage: 92%
Deployed to: https://test.app.com
```

Notify Delivery Manager (in chat or update assignments.md):

```markdown
Story 00042 complete. Ready for QA. Moving to story 00043?
```

**6. Delivery Manager Assigns Next Story**

Update `docs/work/assignments.md`:

```markdown
## Completed This Sprint

| Story ID | Story Title | Assigned To | Completed | Notes |
|----------|-------------|-------------|-----------|-------|
| 00042 | User profile page | Frontend Developer (Role 20) | 2025-11-07 | Deployed to test |

## Current Sprint/Iteration

| Story ID | Story Title | Assigned To | Status | Started | Notes |
|----------|-------------|-------------|--------|---------|-------|
| 00045 | User settings page | Frontend Developer (Role 20) | assigned | 2025-11-07 | Next priority |
```

---

## Multi-Role Collaboration

### Scenario: Implementation Role Needs Help

**Example: Frontend Developer blocked on API clarification**

**1. Frontend Developer adds BLOCKER**

In story audit log:

```markdown
[2025-11-07 15:30 UTC] BLOCKER: API endpoint /api/users/:id returns 404 even for valid user IDs. Need clarification from API Designer (Role 7) on endpoint path and authentication.
```

**2. Frontend Developer requests help**

In chat or handover:

```
I'm blocked on story 00042. The API endpoint isn't working as expected. 
Can we bring in the API Designer (Role 7) to clarify the endpoint specification?
```

**3. AI switches to API Designer role**

```
I'm now assuming the API Designer role. Let me review the API specifications 
and clarify the /api/users/:id endpoint...

[Reads docs/artifacts/07-api-designer/api-specifications.md]
[Provides clarification or updates specification]
```

**4. Frontend Developer continues**

```
Thank you! I'm switching back to Frontend Developer role.

[Adds audit log entry]
[2025-11-07 15:45 UTC] BLOCKER RESOLVED: API Designer clarified endpoint requires Bearer token in Authorization header. Proceeding with implementation.
```

### Scenario: Multiple Implementation Roles Coordinating

**Example: Frontend and Backend working on same feature**

`docs/work/assignments.md`:

```markdown
| Story ID | Story Title | Assigned To | Status | Started | Notes |
|----------|-------------|-------------|--------|---------|-------|
| 00042 | Profile UI | Frontend Developer | in-progress | 2025-11-07 | Waiting on API |
| 00043 | Profile API | Backend Developer | in-progress | 2025-11-07 | API complete, testing |
```

**Backend Developer completes Story 00043:**

```markdown
[2025-11-07 14:30 UTC] STATUS: done - API endpoint /api/users/:id complete
```

**Backend Developer notifies Frontend Developer:**

```
Story 00043 (Profile API) is complete. API is deployed to test environment.
Frontend Developer can now integrate with story 00042.
```

**Frontend Developer proceeds:**

```markdown
[2025-11-07 14:35 UTC] NOTE: Story 00043 unblocked. Proceeding with API integration.
```

---

## Lean Handovers with Work Tracking

### Problem: Handovers Can Get Large

**Without work tracking:**
```markdown
# Handover: Business Analyst → Requirements Engineer

## Context
The customer wants a user authentication system with login, registration, 
password reset, email verification, and social login.

## User Stories Created
1. As a user, I want to register with email/password...
   - Acceptance criteria: ...
   - Technical notes: ...
   
2. As a user, I want to login with email/password...
   - Acceptance criteria: ...
   - Technical notes: ...

[... 3 more stories with full details ...]
```

**This duplicates content and makes handovers large.**

### Solution: Reference by ID

**With work tracking:**
```markdown
# Handover: Business Analyst → Requirements Engineer

## Context
Feature 00001 (user-authentication) has been refined and is ready for detailed requirements.

## Completed Work
- Feature 00001 created with 5 user stories
- Story IDs: 00042, 00043, 00044, 00045, 00046
- All stories in `docs/work/features/todo/00001-user-authentication/stories/`

## Next Steps
Requirements Engineer should:
1. Review feature 00001 acceptance criteria
2. Create detailed functional requirements for stories 00042-00046
3. Link requirements to stories in audit logs
```

**Much cleaner!** All details are in the work tracking system, handover just references them.

### Handover Template with Work Tracking

```markdown
# Handover: [From Role] → [To Role]

**Created**: [YYYY-MM-DD HH:mm UTC]
**From**: [Role Name] ([Role Number])
**To**: [Role Name] ([Role Number])

---

## Context
[Brief context - what phase are we in, what's the current focus]

## Work Items
[Reference work items by ID, don't duplicate content]

**Features:**
- Feature [id]: [name] - [status/location]

**Stories:**
- Story [id]: [name] - [status/location]

## Completed Work
[What was accomplished - reference IDs]
- Feature [id]: [what was done]
- Story [id]: [what was done]

## Next Steps
[What the next role should do - reference IDs]
1. Review feature/story [id]
2. Create [artifact/deliverable]
3. Update story [id] audit log

## Questions/Issues
[Any blockers or clarifications needed - can reference work item IDs]

## Notes
[Any additional context]
```

---

## Audit Log Standards

### Format

**All audit log entries follow this format:**

```markdown
[YYYY-MM-DD HH:mm UTC] [TYPE]: [Details]
```

### Entry Types

| Type | When to Use | Example |
|------|-------------|---------|
| `CREATED` | Work item is first created | `CREATED by Delivery Manager` |
| `ASSIGNMENT` | Story assigned to implementation role | `ASSIGNMENT: Assigned to Frontend Developer (Role 20)` |
| `STATUS` | Status changes | `STATUS: in-progress - Started implementation` |
| `BLOCKER` | Work is blocked | `BLOCKER: Waiting on API specs from Role 7` |
| `BLOCKER RESOLVED` | Blocker is cleared | `BLOCKER RESOLVED: API specs received, proceeding` |
| `NOTE` | General information | `NOTE: Updated acceptance criteria based on customer feedback` |
| `DEPENDENCY` | Dependency information | `DEPENDENCY: Requires story 00041 to be complete` |

### Examples

**Feature lifecycle:**
```markdown
## Audit Log

[2025-11-05 09:00 UTC] CREATED by Customer (Role 0) - Initial concept
[2025-11-05 10:30 UTC] NOTE: Refined by Business Analyst (Role 1)
[2025-11-05 14:00 UTC] STATUS: moved to features/todo/ - Ready for implementation
[2025-11-06 08:00 UTC] STATUS: moved to features/in-progress/ - Story 00042 started
[2025-11-07 16:00 UTC] STATUS: moved to features/done/ - All stories complete
```

**Story lifecycle:**
```markdown
## Audit Log

[2025-11-06 08:00 UTC] CREATED by Delivery Manager - From feature 00001
[2025-11-06 08:15 UTC] ASSIGNMENT: Assigned to Frontend Developer (Role 20)
[2025-11-06 09:00 UTC] STATUS: in-progress - Started implementation
[2025-11-06 09:30 UTC] STATUS: in-progress - Tests created (TDD RED)
[2025-11-06 10:00 UTC] STATUS: in-progress - Implementation complete (TDD GREEN)
[2025-11-06 10:30 UTC] STATUS: in-review - PR #123 created
[2025-11-06 11:00 UTC] BLOCKER: PR review shows accessibility issues
[2025-11-06 14:00 UTC] BLOCKER RESOLVED: Accessibility fixes applied, re-requested review
[2025-11-06 15:00 UTC] STATUS: done - PR merged, deployed to test
Files created:
- projects/frontend/src/components/LoginForm.jsx
- projects/frontend/tests/LoginForm.test.jsx
Test coverage: 95%
```

---

## Integration with Artifacts

### Linking Work Items to Planning Artifacts

**In work item files**, reference planning artifacts:

```markdown
# Story 00042: Implement User Profile Page

## Related Artifacts
- **Requirements**: FR-042 in `docs/artifacts/02-requirements-engineer/functional-requirements.md`
- **Design**: Profile page wireframe in `docs/artifacts/05-ux-ui-designer/wireframes.md`
- **API**: GET /api/users/:id in `docs/artifacts/07-api-designer/api-specifications.md`
- **Security**: Authentication requirement SEC-007 in `docs/artifacts/04-security-architect/security-requirements.md`
```

**In planning artifacts**, reference work items:

```markdown
## FR-042: User Profile Management

**ID**: FR-042
**Category**: User Management
**Priority**: Must Have

**Description**:
The system shall allow authenticated users to view and edit their profile information.

**Implementation**:
- Feature: 00001-user-authentication
- Story: 00042-user-profile-page

**Acceptance Criteria**:
- [ ] User can view profile information
- [ ] User can edit name and bio
- [ ] Changes are saved persistently
```

### Traceability Matrix

**In Requirements Traceability Matrix** (`docs/requirements/requirements-traceability-matrix.md`):

```markdown
| Business Req | Functional Req | Work Item | Test Cases | Priority |
|--------------|----------------|-----------|------------|----------|
| BR-001 | FR-042 | Feature 00001, Story 00042 | TC-042-001 to TC-042-005 | Must Have |
```

---

## See Also

- **[agent.md](./agent.md)** - Core orchestration and role system
- **[agent-project-structure.md](./agent-project-structure.md)** - Project structure and code implementation
- **[agent-handovers.md](./agent-handovers.md)** - Handover process and templates
- **[docs/work-tracking-instructions.md](./docs/work-tracking-instructions.md)** - Detailed work tracking instructions

---

**Remember**: Work tracking is for CLIENT PROJECTS during delivery phase. It provides structure for managing features and stories during iterative implementation.
