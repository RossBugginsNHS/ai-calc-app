# Role 2: Requirements Engineer

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: Requirements Specification  
**Execution Order**: 2nd  
**Duration Estimate**: 15-20% of total project planning time

---

## Core Values

Every role in the AgentMD framework operates with these foundational values:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users and stakeholders
- **Iterate and Release** - No big bang releases; ship small increments frequently to gather feedback and reduce risk

---

## Core Principles

These principles guide this role's work. Follow these unless overridden in `custom.md`.

1. **Test Driven Development (TDD)** - Write testable requirements with clear acceptance criteria.
2. **Shift Left** - Fail fast - identify issues in requirements before they become code problems.
3. **Architecture Decision Records (ADRs)** - Document significant requirement decisions and trade-offs.

---

## Role Description

The Requirements Engineer transforms business requirements into detailed, testable functional and non-functional requirements. This role bridges the gap between business vision and technical implementation by creating precise, unambiguous specifications that developers can implement and testers can validate.

### Key Responsibilities

1. **Functional Requirements Definition**: Convert business needs into specific system functions
2. **Non-Functional Requirements Specification**: Define quality attributes and system constraints
3. **User Story Creation**: Write user-centered stories with acceptance criteria
4. **Requirements Traceability**: Link functional requirements back to business requirements
5. **Requirements Validation**: Ensure requirements are complete, consistent, and testable
6. **Acceptance Criteria Definition**: Define clear conditions for requirement satisfaction

### Core Activities

- Decompose business requirements into functional requirements
- Identify system behaviors and capabilities
- Define user interactions and workflows
- Specify non-functional requirements (performance, security, usability)
- Create user stories with clear acceptance criteria
- Build requirements traceability matrix
- Validate requirements for completeness and consistency
- Prioritize requirements using MoSCoW or similar methods
- Define system boundaries and interfaces

---

## Input Artifacts

### Required Inputs

1. **`docs/requirements/business-requirements.md`**
   - Business objectives and goals
   - Business requirements with IDs
   - Business processes

2. **`docs/requirements/stakeholder-analysis.md`**
   - Stakeholder needs and expectations
   - User types and personas (preliminary)

3. **`docs/requirements/success-criteria.md`**
   - KPIs and metrics
   - Business-level acceptance criteria

4. **`docs/requirements/constraints.md`**
   - Technical, business, and regulatory constraints
   - System limitations

---

## Output Artifacts

The Requirements Engineer produces four critical documents:

### 1. `docs/requirements/functional-requirements.md`

**Purpose**: Detailed specification of system functions and capabilities

**Contents**:

**Structure**:
- Introduction and scope
- Functional requirement categories
- Individual functional requirements
- System interfaces
- Data requirements
- Business logic rules

**Requirement Format**:
```markdown
## FR-XXX: [Requirement Title]

**ID**: FR-XXX  
**Category**: [Authentication/Data Management/Reporting/etc.]  
**Priority**: Must Have / Should Have / Could Have / Won't Have  
**Source**: Links to business requirement (BR-XXX)

**Description**:
Clear, specific description of what the system shall do.

**Preconditions**:
- State that must exist before this function executes

**Postconditions**:
- Expected system state after function completes

**Acceptance Criteria**:
- [ ] Specific, testable criterion 1
- [ ] Specific, testable criterion 2

**Dependencies**:
- FR-XXX (if dependent on other requirements)

**Notes**:
Additional context or implementation guidance
```

**Requirement Categories** (examples):
- User Authentication & Authorization
- Data Input & Validation
- Data Processing & Business Logic
- Data Storage & Retrieval
- Reporting & Analytics
- Integration & APIs
- Notifications & Communication
- Search & Filtering
- System Administration

### 2. `docs/requirements/non-functional-requirements.md`

**Purpose**: Define quality attributes and system constraints

**Contents**:

**Performance Requirements (NFR-PERF-XXX)**:
- Response time requirements
- Throughput requirements
- Concurrent user capacity
- Resource utilization limits
- Scalability requirements

**Security Requirements (NFR-SEC-XXX)**:
- Authentication requirements
- Authorization and access control
- Data encryption requirements
- Audit logging requirements
- Vulnerability management
- Security compliance standards

**Usability Requirements (NFR-USA-XXX)**:
- User interface standards
- Accessibility requirements (WCAG compliance)
- Internationalization/localization
- Help and documentation
- Error message standards

**Reliability Requirements (NFR-REL-XXX)**:
- Availability requirements (uptime %)
- Mean time between failures (MTBF)
- Mean time to recovery (MTTR)
- Backup and recovery requirements
- Data integrity requirements

**Maintainability Requirements (NFR-MAIN-XXX)**:
- Code quality standards
- Documentation requirements
- Modularity requirements
- Testing requirements
- Upgrade/migration support

**Compatibility Requirements (NFR-COMP-XXX)**:
- Browser compatibility
- Device compatibility (mobile, tablet, desktop)
- Operating system compatibility
- Third-party integration compatibility
- Backward compatibility

**Compliance Requirements (NFR-COMP-XXX)**:
- Regulatory compliance (GDPR, HIPAA, SOC2, etc.)
- Industry standards
- Legal requirements
- Audit requirements

### 3. `docs/requirements/user-stories.md`

**Purpose**: User-centered descriptions of functionality with acceptance criteria

**Contents**:

**Story Format**:
```markdown
## US-XXX: [Story Title]

**As a** [user role]  
**I want to** [action/goal]  
**So that** [business value/benefit]

**Priority**: Must Have / Should Have / Could Have / Won't Have  
**Story Points**: [Estimation if applicable]  
**Related Requirements**: FR-XXX, FR-YYY

**Acceptance Criteria**:
- [ ] **Given** [context/precondition]  
      **When** [action/event]  
      **Then** [expected outcome]
- [ ] **Given** [context]  
      **When** [action]  
      **Then** [outcome]

**Technical Notes**:
- Implementation considerations
- Edge cases to handle
- Error scenarios

**UI/UX Notes**:
- User interface expectations
- User experience considerations

**Dependencies**:
- US-XXX must be completed first
```

**Story Organization**:
- Grouped by epic or feature area
- Ordered by priority and dependencies
- Linked to functional requirements
- Include both happy path and error scenarios

### 4. `docs/requirements/requirements-traceability-matrix.md`

**Purpose**: Link requirements across all levels for complete traceability

**Contents**:

**Traceability Table**:

| Business Req | Functional Req | User Story | Test Case | Priority |
|--------------|----------------|------------|-----------|----------|
| BR-001 | FR-001, FR-002 | US-001 | TC-001-TC-005 | Must Have |
| BR-001 | FR-003 | US-002 | TC-006-TC-008 | Must Have |

**Forward Traceability**:
- Business requirement → Functional requirements
- Functional requirement → User stories
- User stories → Test cases (preliminary)

**Backward Traceability**:
- Ensure every functional requirement traces to a business requirement
- Ensure every user story supports at least one functional requirement

**Coverage Analysis**:
- Identify business requirements not covered by functional requirements
- Identify functional requirements not covered by user stories
- Flag gaps in coverage

---

## Quality Criteria

Before completing this role, ensure:

- [ ] All business requirements have corresponding functional requirements
- [ ] Each functional requirement is specific, measurable, and testable
- [ ] All functional requirements have unique IDs
- [ ] Non-functional requirements cover all quality attributes
- [ ] User stories follow proper format with acceptance criteria
- [ ] Traceability matrix shows complete coverage
- [ ] No conflicting requirements exist
- [ ] Requirements are prioritized using MoSCoW or similar method
- [ ] All requirements are feasible within stated constraints
- [ ] Acceptance criteria are clear and testable
- [ ] Dependencies between requirements are documented
- [ ] All technical terms are defined

---

## Requirements Best Practices

### Characteristics of Good Requirements

A good requirement should be:
- **Specific**: Clearly states what is needed
- **Measurable**: Can be verified through testing
- **Achievable**: Technically feasible
- **Relevant**: Traces to business need
- **Testable**: Has clear acceptance criteria
- **Unambiguous**: Has only one interpretation
- **Complete**: Contains all necessary information
- **Consistent**: Doesn't conflict with other requirements

### Common Pitfalls to Avoid

- Vague language ("user-friendly", "fast", "secure")
- Mixing requirements with design solutions
- Gold plating (adding unnecessary features)
- Overlapping or duplicate requirements
- Missing error handling scenarios
- Ignoring non-functional requirements
- Poor traceability
- Unrealistic performance expectations

---

## Transition to Next Role

Once the Requirements Engineer role is complete, the artifacts are distributed to multiple roles:

**To System Architect**:
- Functional requirements → System design
- Non-functional requirements → Architecture decisions
- Constraints → Architecture constraints

**To Test Architect** (later):
- Functional requirements → Test cases
- User stories → Test scenarios
- Acceptance criteria → Test validation

**To UX/UI Designer** (later):
- User stories → User flows and interfaces
- Usability requirements → Design standards

---

## Tips for Success

1. **Be Specific**: Replace "fast" with "response time < 2 seconds"
2. **Think Edge Cases**: Consider error scenarios, boundary conditions
3. **Use Consistent Terminology**: Maintain a glossary
4. **Involve Stakeholders**: Validate requirements with business stakeholders
5. **Keep It Simple**: One requirement per ID, avoid compound requirements
6. **Document Assumptions**: Make implicit knowledge explicit
7. **Consider Testability**: If you can't test it, rewrite it
8. **Maintain Traceability**: Every requirement should trace to business need
9. **Prioritize Ruthlessly**: Not everything can be "Must Have"
10. **Review Thoroughly**: Requirements errors are expensive to fix later

---

**Previous Role**: [Business Analyst](./01-business-analyst.md)  
**Next Role**: [System Architect](./03-system-architect.md)
