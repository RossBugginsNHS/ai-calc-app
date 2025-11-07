# Development Tracker

> **⚠️ THIS FILE IS FOR TEMPLATE DEVELOPMENT ONLY**  
> This file tracks the development of the AgentMD template repository itself.  
> **Delete this file when the template is complete and ready for use.**

---

## Current Status

**Phase**: Building the AgentMD Framework Template  
**Started**: 2025-11-07  
**Last Updated**: 2025-11-07

---

## 📋 Todo List

### High Priority
- [ ] Create 7 new enabling roles (13-19) with default.md and custom.md
- [ ] Document Team Topologies classification in relevant role files
- [ ] Document all 42 architecture principles in agent-custom.md template  
- [ ] Create comprehensive examples for each role's outputs

### New Roles to Add (Enabling Teams)
- [ ] Role 13: User Researcher - User needs analysis, research validation
- [ ] Role 14: Information Governance Officer - Data classification, retention, compliance
- [ ] Role 15: Safety Officer - Domain-specific safety (clinical, financial, operational)
- [ ] Role 16: Accessibility Specialist - WCAG compliance, inclusive design
- [ ] Role 17: Performance Engineer - Performance requirements, optimization, SLAs
- [ ] Role 18: Compliance Officer - Regulatory compliance, audits, certifications
- [ ] Role 19: FinOps Analyst - Cloud cost optimization, TCO analysis, budget forecasting

### Core Framework
- [ ] Complete all role default.md files with detailed instructions
- [ ] Add examples to each role custom.md file
- [ ] Create workflow diagrams (Mermaid) for the process
- [ ] Document handover process thoroughly
- [ ] Create troubleshooting guide

### Documentation
- [ ] Enhance README with more examples
- [ ] Create video walkthrough guide
- [ ] Add FAQ section
- [ ] Create contributor guidelines
- [ ] Add license information

### Project Template
- [ ] Complete the projects/_template structure
- [ ] Add language-specific templates (Python, TypeScript, Go, etc.)
- [ ] Create CI/CD workflow templates for GitHub Actions
- [ ] Add Kubernetes manifest templates
- [ ] Add Terraform/IaC templates

### Testing & Validation
- [ ] Test the workflow end-to-end with a sample project
- [ ] Validate all artifacts generate correctly
- [ ] Test handover process across multiple sessions
- [ ] Verify git commit process works
- [ ] Test with different project types

---

## ✅ Done

### 2025-11-07
- [x] Created basic repository structure
- [x] Created 13 role folders with default.md and custom.md
- [x] Added READ-ONLY warnings to all default.md files
- [x] Created docs/artifacts/ structure with .gitkeep files
- [x] Created docs/handovers/ and docs/history/ structures
- [x] Renamed outputs → artifacts throughout
- [x] Implemented collaborative colleague model for Customer role
- [x] Created agent-custom.md for global customizations
- [x] Created .github/copilot-instructions.md
- [x] Added customization system to README
- [x] Started projects/ folder with _template structure
- [x] Defined 42 architecture principles and practices
- [x] Added Core Principles sections to all 13 role default.md files
- [x] Mapped and distributed all 42 principles across relevant roles

---

## 📝 Notes & Decisions

### Architecture & Design Principles (2025-11-07)

Comprehensive set of 42 principles to guide all projects:

#### Core Methodologies
1. **Test Driven Development (TDD)** - Tests before code, always
2. **Domain Driven Design (DDD)** - Model the business domain
3. **API-First Design** - Design APIs before implementation
4. **Security by Design** - Security from the start, not bolted on later
5. **Shift Left** - Fail fast, catch issues early
6. **12-Factor App Principles** - For portable, cloud-agnostic applications

#### System Architecture
7. **Event Driven Architecture** - Asynchronous, decoupled communication
8. **Microservices** - Clear service boundaries
9. **Clear Separation of Concerns** - Each component has one responsibility
10. **Idempotency** - Operations can be safely retried
11. **Eventual Consistency** - Embrace it in distributed systems
12. **Backward Compatibility** - Versioning strategy for APIs and events

#### Infrastructure & Deployment
13. **CI/CD Pipeline** - Integrated with GitHub
14. **Infrastructure as Code** - Everything versioned and reproducible
15. **Cloud Provider Agnostic** - Design for portability (ask: AWS/Azure/GCP, but ensure Docker Compose/local alternatives)
16. **Local Development Parity** - Must be able to test and deploy locally
17. **Dev/Prod Parity** - Environments should be as similar as possible
18. **Publishable Artifacts** - Build once, deploy many times
19. **Container Orchestration** - Ask preference (Kubernetes, Docker Swarm, Docker Compose)

#### Quality & Security
20. **Automated Security Scanning** - SAST/DAST in CI/CD (OWASP, STRIDE)
21. **Code Quality Gates** - Linting, formatting, complexity checks in CI/CD
22. **Dependency Management** - Regular updates, vulnerability scanning
23. **Observability** - Logging, metrics, tracing from day one

#### Data & State Management
24. **Database Migrations** - Versioned, reversible, testable
25. **Data Privacy by Design** - GDPR compliance, data minimization
26. **Message Broker Strategy** - Ask preference (Kafka, RabbitMQ, cloud-native)
27. **API Gateway Strategy** - Ask preference (centralized gateway, service mesh, none)
28. **Configuration Management** - Ask how to handle secrets/config across environments

#### Documentation Standards
29. **Use Mermaid for Diagrams** - All diagrams in Mermaid format
30. **Use Markdown** - All documentation in Markdown
31. **Code Blocks Must Specify Language** - Always include language identifier
32. **Architecture Decision Records (ADRs)** - Document why decisions were made
33. **README-Driven Development** - Document before building
34. **OpenAPI/AsyncAPI Specifications** - For REST and event-driven APIs
35. **Changelog Maintenance** - Keep CHANGELOG.md up to date

#### Technology Choices
36. **Always Ask About Tech Stack** - Language, frameworks, specific versions
37. **Never Assume** - If unsure, ask; then suggest if needed
38. **Ask About Hosting** - AWS, Azure, GCP, on-premise, or hybrid

#### Development Practices
39. **Monorepo Structure** - All projects in `/projects` with consistent structure
40. **Trunk-Based Development** - Or short-lived feature branches
41. **Semantic Versioning** - Clear version numbering
42. **Feature Flags** - Toggle features without deployment

### Team Topologies Framework (2025-11-07)

AgentMD roles are organized using Team Topologies principles. This helps understand role relationships and cognitive load management.

#### Four Team Types

1. **Stream-Aligned Teams** - Core value delivery, aligned to business domain or user journey
2. **Enabling Teams** - Help Stream-Aligned teams by removing blockers and building capabilities
3. **Complicated-Subsystem Teams** - Handle complex subsystems requiring specialized expertise
4. **Platform Teams** - Provide internal services to reduce Stream-Aligned team cognitive load

#### Role Classifications (20 Total Roles)

##### Stream-Aligned Teams (5 roles)
Core delivery roles aligned to project value streams:
- **Role 0: Customer** - Initiates projects, provides business context, validates outcomes
- **Role 1: Business Analyst** - Analyzes business needs, defines requirements
- **Role 2: Requirements Engineer** - Formalizes functional/non-functional requirements
- **Role 10: Technical Lead** - Leads implementation decisions, code quality
- **Role 12: Project Manager** - Coordinates delivery, manages timelines and risks

##### Enabling Teams (11 roles)
Specialist roles that help others succeed by reducing cognitive load:
- **Role 4: Security Architect** - Enables secure development through threat modeling, SAST/DAST
- **Role 5: UX/UI Designer** - Enables user-centered design and usability
- **Role 9: Test Architect** - Enables quality through testing strategy and automation
- **Role 11: Documentation Writer** - Enables knowledge sharing and onboarding
- **Role 13: User Researcher** - Enables evidence-based design through user research
- **Role 14: Information Governance Officer** - Enables data compliance and privacy
- **Role 15: Safety Officer** - Enables domain-specific safety (clinical, financial, operational)
- **Role 16: Accessibility Specialist** - Enables inclusive design and WCAG compliance
- **Role 17: Performance Engineer** - Enables optimization through performance requirements and SLAs
- **Role 18: Compliance Officer** - Enables regulatory compliance and audit readiness
- **Role 19: FinOps Analyst** - Enables cost optimization and financial accountability

##### Complicated-Subsystem Teams (3 roles)
Specialized technical roles handling complex subsystems:
- **Role 3: System Architect** - Complex system design, architectural patterns, service boundaries
- **Role 6: Database Designer** - Complex data modeling, optimization, migrations
- **Role 7: API Designer** - Complex API design, contracts, event schemas

##### Platform Team (1 role - hybrid)
Internal platform services:
- **Role 8: DevOps Engineer** - Platform services (CI/CD, IaC, observability) + Complicated-Subsystem expertise

#### Design Rationale

**Why These Enabling Teams?**
- **User Researcher**: Critical for evidence-based design; every project should validate with real users
- **Information Governance**: Data classification, retention, privacy are cross-cutting concerns
- **Safety Officer**: Domain-specific safety (clinical safety, financial risk) can't be an afterthought
- **Accessibility**: WCAG compliance and inclusive design are legal requirements and ethical imperatives
- **Performance Engineer**: Performance requirements often missed until production; needs explicit focus
- **Compliance Officer**: Regulatory landscape (GDPR, DORA, industry-specific) requires dedicated expertise
- **FinOps**: Cloud costs spiral without dedicated cost optimization and budget forecasting

**Why Not Stream-Aligned?**
These are cross-cutting concerns that would create excessive cognitive load if every Stream-Aligned team had to maintain expertise. Better to enable others than to duplicate effort.

**Why Not Complicated-Subsystem?**
While these roles require expertise, they're not building complex subsystems. They're providing guidance, standards, and validation to enable others.

### Customization System (2025-11-07)
Three-tier hierarchy:
1. Core behavior: agent.md + role default.md (READ-ONLY)
2. Global customizations: agent-custom.md (EDITABLE)
3. Role customizations: role custom.md (EDITABLE)

### Collaborative Colleague Model (2025-11-07)
Customer role (Role 0) operates as peer colleague, not interviewer.
Natural conversation, working together to scope projects.

---

## 📝 Notes & Decisions (Historical)

### Architecture Principles (2025-11-07)
Decided on 42 core principles covering:
- TDD, DDD, API-First, Security by Design
- Event-driven architecture, microservices
- Cloud-agnostic with local development support
- CI/CD, IaC, observability
- Monorepo structure in /projects

### Customization System (2025-11-07)
Three-tier hierarchy:
1. Core behavior: agent.md + role default.md (READ-ONLY)
2. Global customizations: agent-custom.md (EDITABLE)
3. Role customizations: role custom.md (EDITABLE)

### Collaborative Colleague Model (2025-11-07)
Customer role (Role 0) operates as peer colleague, not interviewer.
Natural conversation, working together to scope projects.

---

## 🎯 Next Steps

1. **Integrate Architecture Principles**: Update agent.md and relevant role files with the 42 principles
2. **Complete Project Template**: Finish creating the template files that were interrupted
3. **Role Enhancement**: Add specific guidance to each role about principles they should emphasize
4. **Examples**: Create at least one complete example walkthrough

---

## 🐛 Known Issues

None currently.

---

## 💡 Ideas for Future

- Integration with project management tools
- Support for multiple programming languages
- Template variations for different project types
- Plugin system for custom roles
- Integration with code generation tools

---

## 🤝 Contributors

- rb (Human) - Framework design and requirements
- GitHub Copilot (Agent) - Implementation and documentation
