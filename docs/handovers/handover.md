# Handover Document

**From Role**: 04 - Security Architect  
**To Role**: 05 - UX/UI Designer  
**Date**: 2025-11-07  
**Project**: Basic Calculator Web App

---

## Summary

Comprehensive security architecture has been designed for the Basic Calculator Web App, covering threat modeling (STRIDE), authentication & authorization, data protection, API security, infrastructure security, compliance (GDPR, PCI DSS), security monitoring, incident response, and security testing. The security design implements defense-in-depth with multiple layers of protection and targets OWASP ASVS Level 2 compliance.

---

## What Was Completed

### Artifacts Created

- ✅ **Security Architecture** (`docs/artifacts/04-security-architect/security-architecture.md`)
  - Security principles (defense in depth, least privilege, zero trust)
  - Comprehensive threat model using STRIDE methodology
  - Authentication architecture (JWT + refresh tokens, bcrypt password hashing)
  - Data protection (encryption at rest/transit, GDPR compliance, data retention)
  - API security (input validation, rate limiting, security headers, CORS)
  - Infrastructure security (VPC, IAM, secrets management, DDoS protection)
  - Application security (XSS prevention, CSRF protection, dependency security)
  - Compliance (GDPR, PCI DSS via Stripe, OWASP ASVS Level 2)
  - Security monitoring (CloudWatch alarms, audit logging, threat detection)
  - Incident response (detection, containment, recovery, notification)
  - Security testing (SAST, DAST, penetration testing, security code review)
  - Security governance (policies, training, roadmap)

- ✅ **Security Testing Plan** (`docs/artifacts/04-security-architect/security-testing-plan.md`)
  - Testing strategy (automated + manual, risk-based prioritization)
  - SAST (Static Analysis) - ESLint security plugins, Semgrep, TypeScript strict mode
  - DAST (Dynamic Analysis) - OWASP ZAP automated scans, manual testing
  - Dependency security - npm audit, Snyk, Dependabot automation
  - Penetration testing - Quarterly external audits, OWASP Testing Guide methodology
  - Security code review - Checklist for auth, input validation, error handling
  - API security testing - Authentication, authorization, input validation tests
  - Authentication testing - Password security, JWT validation, session management
  - Infrastructure testing - Terraform scanning (Checkov), AWS security checks
  - Test execution schedule - Continuous (CI/CD), daily, weekly, monthly, quarterly

- ✅ **Incident Response Plan** (`docs/artifacts/04-security-architect/incident-response-plan.md`)
  - Incident response team (roles, responsibilities, contact information)
  - Incident classification (P0-P3 severity levels, response times)
  - NIST incident response lifecycle (Prepare → Detect → Contain → Eradicate → Recover → Learn)
  - Detection and analysis (automated alerts, indicators of compromise)
  - Containment strategies (short-term actions, evidence preservation, forensics)
  - Eradication and recovery (root cause analysis, threat removal, staged rollout)
  - Post-incident activities (PIR within 1 week, lessons learned, documentation)
  - Communication plan (internal, external, regulatory notification)
  - Incident response playbooks (data breach, DDoS, account takeover, malicious code)
  - Tools and resources (forensics, communication, containment, recovery)

- ✅ **Security Runbook** (`docs/artifacts/04-security-architect/security-runbook.md`)
  - Daily security operations (morning checks, log reviews, monitoring)
  - Authentication operations (password resets, session invalidation, JWT rotation)
  - Access control operations (grant/revoke access, administrator management)
  - Secrets management (add/rotate/revoke secrets, AWS Secrets Manager)
  - Monitoring and alerting (configure alerts, tune thresholds, review alarms)
  - Incident response procedures (declare, investigate, close incidents)
  - Security hardening (Lambda, RDS, CloudFront security headers)
  - Compliance operations (GDPR data export, right to be forgotten)
  - Emergency procedures (kill switch, rollback deployments)
  - Security maintenance (quarterly reviews, monthly dependency updates)

### Key Security Decisions

1. **Multi-Token Authentication**: Access tokens (15 min) + refresh tokens (7 days) for security + usability balance
2. **bcrypt Password Hashing**: Cost factor 12 (~300ms per hash) balances security and UX
3. **Defense in Depth**: Multiple security layers (network, infrastructure, application, data)
4. **OWASP Top 10 Mitigations**: Input validation, output encoding, authentication, access control
5. **Rate Limiting**: Multi-layer (per-user 100/min, per-IP 1000/min, global 10K/min)
6. **Security Headers**: CSP, HSTS, X-Frame-Options, X-Content-Type-Options
7. **Automated Security Testing**: SAST/DAST in CI/CD pipeline, fail build on critical issues
8. **GDPR Compliance**: Soft delete (30-day grace), data export API, right to be forgotten
9. **Incident Response**: 15-minute response time for P0, 72-hour GDPR notification
10. **Quarterly Penetration Testing**: External security audits before production launch

### Security Architecture Summary

**Authentication & Authorization**:

- JWT with HS256 algorithm, 256-bit secret in AWS Secrets Manager
- Refresh token rotation (invalidation on logout, password change)
- HttpOnly cookies (SameSite=Strict) prevent XSS token theft
- Brute force protection (5 failed attempts = 30-minute lockout)
- Password requirements (8+ chars, uppercase, lowercase, number)

- ✅ **Data Architecture** (`docs/artifacts/03-system-architect/data-architecture.md`)
  - Entity-relationship diagram (users, subscriptions, calculation_history, payment_history)
  - Database schema (PostgreSQL with Prisma)
  - Data storage strategy (localStorage for free, PostgreSQL for premium)
  - Data access patterns and query optimization
  - Database migration strategy (Prisma Migrate, zero-downtime)
  - Data archival and retention policies
  - Backup and recovery procedures
  - GDPR compliance implementation (soft deletes, data export)
  - Performance optimization (indexing, pagination, connection pooling)

- ✅ **API Design** (`docs/artifacts/03-system-architect/api-design.md`)
  - RESTful API design (/auth, /history, /subscription, /webhooks)
  - Authentication flow (JWT with httpOnly cookies)
  - Endpoint specifications (request/response schemas)
  - Error handling (standard error format, HTTP status codes)
  - Rate limiting strategy (per-user and global limits)
  - CORS configuration
  - API versioning strategy (URL path versioning)
  - Security measures (input validation, SQL injection prevention)
  - OpenAPI specification (for interactive docs)

- ✅ **Deployment Architecture** (`docs/artifacts/03-system-architect/deployment-architecture.md`)
  - Infrastructure overview (AWS services, cost breakdown)
  - Environment strategy (dev, staging, production)
  - Infrastructure as Code (Terraform structure and examples)
  - CI/CD pipeline (GitHub Actions workflow)
  - Database migration procedures (zero-downtime strategy)
  - Monitoring and observability (CloudWatch dashboards, alarms)
  - Backup and disaster recovery (RTO/RPO targets)
  - Security (network security, IAM roles, secrets management)
  - Cost optimization strategies
  - Operational procedures (deployment checklist, incident response)

- ✅ **Architecture Decision Records** (`docs/artifacts/03-system-architect/architecture-decision-records.md`)
  - ADR-001: Vanilla TypeScript (vs React/Vue/Svelte)
  - ADR-002: AWS Lambda (vs ECS/EC2)
  - ADR-003: PostgreSQL (vs MongoDB/DynamoDB)
  - ADR-004: RESTful API (vs GraphQL)
  - ADR-005: Monorepo (vs multi-repo)
  - ADR-006: Stripe (vs PayPal/Braintree)
  - ADR-007: JWT Authentication (vs sessions/OAuth)
  - ADR-008: Prisma ORM (vs TypeORM/Knex/raw SQL)
  - ADR-009: Terraform (vs CDK/CloudFormation/Pulumi)
  - ADR-010: localStorage for free tier (vs backend API)

### Key Decisions Made

1. **Frontend: Vanilla TypeScript** - No framework overhead (20-30KB bundle vs 65-80KB for React), meets <50KB requirement
2. **Backend: AWS Lambda** - Serverless for zero cost at low usage, auto-scales to 100K users
3. **Database: PostgreSQL (RDS Aurora Serverless v2)** - ACID transactions for payments, auto-scaling
4. **API: RESTful** - Simple (5 resources), team familiarity, HTTP caching support
5. **Payments: Stripe** - PCI DSS compliant, subscription management, lower fees than PayPal
6. **Authentication: JWT** - Stateless (no Redis), httpOnly cookies for security
7. **ORM: Prisma** - Type-safe queries, auto-complete, automated migrations
8. **IaC: Terraform** - Cloud-agnostic, mature ecosystem, remote state in S3
9. **Free Tier Storage: localStorage** - Zero backend cost, instant access, offline support
10. **Architecture Pattern: Hybrid** - Frontend-first with serverless backend (progressive enhancement)

### Architecture Summary

**Frontend**:

- Vanilla TypeScript with MVC pattern
- Bundle size: 30-40KB gzipped (meets <50KB requirement)
- Vite for build tool (fast HMR, optimized bundling)
- CloudFront CDN + S3 for global delivery

**Data Protection**:

- TLS 1.3 for all connections, HSTS header (1-year max-age)
- Database encryption at rest (AES-256) with AWS KMS
- Application-level encryption for sensitive data (bcrypt for passwords)
- Data retention policies (1 year audit logs, 7 years payment records)
- GDPR compliance (right to access, deletion with 30-day grace, data export API)

**API Security**:

- Input validation with Zod schemas (whitelist approach)
- Prisma ORM prevents SQL injection (no raw SQL)
- Rate limiting (per-user, per-IP, global limits)
- Security headers (CSP, X-Frame-Options, X-Content-Type-Options, HSTS)
- CORS whitelist (production, staging, dev origins only)
- Generic error messages (no stack traces or system internals exposed)

**Infrastructure Security**:

- VPC with private subnets for Lambda and RDS
- Security groups (least privilege, Lambda → RDS only)
- IAM roles with minimal permissions
- Secrets Manager for all credentials (JWT secret, DB passwords, API keys)
- CloudWatch logging for audit trail
- AWS GuardDuty for threat detection
- DDoS protection (AWS Shield + CloudFront)

**Security Testing**:

- SAST in CI/CD (ESLint security, Semgrep, TypeScript strict mode)
- DAST daily on staging (OWASP ZAP automated scans)
- Dependency scanning (npm audit, Snyk, Dependabot)
- Quarterly penetration testing (external firm, OWASP Testing Guide)
- Security code reviews (mandatory for auth/payment changes)

---

## Open Questions for UX/UI Designer

1. **Calculator Interface**: What's the optimal layout for calculator buttons on mobile vs desktop?
2. **Visual Hierarchy**: How to guide users from free tier to premium upgrade?
3. **History Display**: Best way to present calculation history (timeline, grid, list)?
4. **Accessibility**: How to make calculator fully keyboard navigable (WCAG 2.1 AA)?
5. **Error States**: How to display errors (invalid input, network failure) without disrupting flow?
6. **Loading States**: What loading indicators for API calls (history, auth)?
7. **Onboarding**: Do we need a tutorial for first-time users?
8. **Premium Features**: How to visually differentiate free vs premium features?
9. **Mobile-First**: Should we design mobile-first then scale up, or responsive from start?
10. **Dark Mode**: Should we support dark mode for better accessibility?
11. **Branding**: What visual style aligns with "professional yet approachable" brand?
12. **User Feedback**: How to collect user feedback on UI/UX (surveys, analytics)?

---

## Context for UX/UI Designer

You now have complete system architecture and security design. Your task is to:

**Design User Experience**:

- User flows (free user → premium upgrade, calculation → save history)
- Information architecture (navigation, content structure)
- Interaction design (button states, transitions, animations)
- Responsive design (mobile-first, tablet, desktop)

**Create User Interface**:

- Visual design system (colors, typography, spacing, components)
- Calculator interface (buttons, display, operations)
- History interface (list/grid view, search, filters)
- Account management (login, signup, profile, settings)
- Payment flow (upgrade CTA, Stripe integration, success/failure states)

**Ensure Accessibility**:

- WCAG 2.1 AA compliance (contrast ratios, keyboard navigation)
- Screen reader support (ARIA labels, semantic HTML)
- Touch targets (minimum 44x44px for mobile)
- Focus indicators (visible keyboard focus)

**Design for Performance**:

- Optimize assets (images, icons, fonts)
- Critical CSS (inline for first paint)
- Progressive enhancement (works without JavaScript)
- Loading states (skeleton screens, spinners)

**Create Design Documentation**:

- Design system (components, patterns, guidelines)
- User flows and wireframes
- High-fidelity mockups (all key screens)
- Prototypes (interactive, clickable)
- UI specifications (measurements, colors, fonts, spacing)

---

## Reference Materials

All requirements, architecture, and security documents are available in:

- `docs/artifacts/00-customer/project-brief.md` - Original project vision
- `docs/artifacts/01-business-analyst/business-case.md` - Market analysis and monetization
- `docs/artifacts/01-business-analyst/stakeholder-analysis.md` - User personas and stakeholders
- `docs/artifacts/02-requirements-engineer/functional-requirements.md` - User stories and features
- `docs/artifacts/02-requirements-engineer/non-functional-requirements.md` - Performance, security, accessibility
- `docs/artifacts/03-system-architect/system-architecture.md` - Overall system design
- `docs/artifacts/03-system-architect/data-architecture.md` - Database schema and data flow
- `docs/artifacts/03-system-architect/api-design.md` - API endpoints and contracts
- `docs/artifacts/03-system-architect/deployment-architecture.md` - Infrastructure and CI/CD
- `docs/artifacts/03-system-architect/architecture-decision-records.md` - Key architectural decisions
- `docs/artifacts/04-security-architect/security-architecture.md` - Security controls and policies
- `docs/artifacts/04-security-architect/security-testing-plan.md` - SAST, DAST, penetration testing
- `docs/artifacts/04-security-architect/incident-response-plan.md` - Security incident procedures
- `docs/artifacts/04-security-architect/security-runbook.md` - Operational security procedures

---

**Handover Complete**  
**Next Role**: 05 - UX/UI Designer  
**Priority**: Design mobile-first calculator interface and premium upgrade flow

---

## Notes

- Security architecture is comprehensive and ready for implementation
- All 12 open questions from System Architect have been answered
- OWASP ASVS Level 2 compliance targeted
- Quarterly penetration testing recommended before production launch
- Focus on user experience without compromising security (e.g., password requirements balance security + usability)


## Reference Materials

All requirements and architecture documents are available in:

- `docs/artifacts/00-customer/project-brief.md` - Original project vision
- `docs/artifacts/01-business-analyst/business-case.md` - Market analysis and monetization
- `docs/artifacts/02-requirements-engineer/functional-requirements.md` - User stories and features
- `docs/artifacts/02-requirements-engineer/non-functional-requirements.md` - Performance, security, accessibility
- `docs/artifacts/03-system-architect/system-architecture.md` - Overall system design
- `docs/artifacts/03-system-architect/data-architecture.md` - Database schema and data flow
- `docs/artifacts/03-system-architect/api-design.md` - API endpoints and contracts
- `docs/artifacts/03-system-architect/deployment-architecture.md` - Infrastructure and CI/CD
- `docs/artifacts/03-system-architect/architecture-decision-records.md` - Key architectural decisions

---

**Handover Complete**  
**Next Role**: 04 - Security Architect  
**Priority**: Review authentication flow and payment security first


- `docs/artifacts/02-requirements-engineer/functional-requirements.md`
- `docs/artifacts/02-requirements-engineer/non-functional-requirements.md`
