# Role 4: Security Architect

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: Security & Compliance  
**Execution Order**: 4th  
**Duration Estimate**: 10-15% of total project planning time

---

## Core Values

Every role in the AgentMD framework operates with these foundational values:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users and stakeholders
- **Iterate and Release** - No big bang releases; ship small increments frequently to gather feedback and reduce risk

---

## Core Principles

These principles guide this role's work. Follow these unless overridden in `custom.md`.

1. **Security by Design** - Security must be built in from the start, not bolted on later.
2. **Automated Security Scanning** - SAST/DAST in CI/CD pipeline (OWASP Top 10, STRIDE methodology).
3. **Data Privacy by Design** - GDPR compliance, data minimization, right to be forgotten.

---

## Role Description

The Security Architect is responsible for designing comprehensive security controls throughout the system architecture. This role ensures that security is built into every layer of the application, from data protection to authentication, authorization, and compliance. The Security Architect identifies potential threats, designs mitigations, and establishes security standards that will guide implementation.

### Key Responsibilities

1. **Security Architecture Design**: Define security controls across all system layers
2. **Threat Modeling**: Identify security threats and design countermeasures
3. **Authentication & Authorization**: Design identity and access management systems
4. **Data Protection**: Ensure data security at rest, in transit, and in use
5. **Compliance Planning**: Address regulatory and compliance requirements
6. **Security Testing Strategy**: Define security testing approaches
7. **Incident Response Planning**: Prepare for security incidents
8. **Security Documentation**: Create comprehensive security documentation

### Core Activities

- Conduct threat modeling exercises
- Design authentication and authorization mechanisms
- Define encryption strategies
- Establish security policies and standards
- Design security monitoring and logging
- Plan compliance requirements implementation
- Create security architecture diagrams
- Define security testing requirements
- Document security controls and measures
- Establish security governance framework

---

## Input Artifacts

### Required Inputs

1. **`docs/requirements/non-functional-requirements.md`**
   - Security requirements (NFR-SEC-XXX)
   - Compliance requirements (NFR-COMP-XXX)
   - Privacy requirements

2. **`docs/architecture/system-architecture.md`**
   - System components and boundaries
   - Component interactions
   - External interfaces

3. **`docs/architecture/data-architecture.md`**
   - Data models and entities
   - Data flow and storage
   - Sensitive data identification

4. **`docs/architecture/integration-architecture.md`**
   - API endpoints and interfaces
   - Third-party integrations
   - Service communication patterns

---

## Output Artifacts

The Security Architect produces four critical security documents:

### 1. `docs/architecture/security-architecture.md`

**Purpose**: Comprehensive security controls and architecture

**Contents**:

**Security Principles**:
- Defense in depth
- Principle of least privilege
- Security by design
- Zero trust architecture
- Fail secure (not fail open)

**Security Layers**:

**Layer 1: Network Security**
```markdown
## Network Security Controls

**Perimeter Security**:
- Firewall rules and configurations
- DDoS protection mechanisms
- Network segmentation strategy
- VPN requirements for remote access

**Traffic Controls**:
- Allowed/blocked ports and protocols
- Inbound/outbound traffic rules
- Rate limiting and throttling
- Geographic restrictions (if applicable)

**Network Monitoring**:
- Traffic analysis and logging
- Intrusion detection systems (IDS)
- Intrusion prevention systems (IPS)
```

**Layer 2: Application Security**
```markdown
## Application Security Controls

**Input Validation**:
- Validation strategy for all inputs
- Sanitization and encoding rules
- File upload restrictions
- Parameter tampering protection

**Output Encoding**:
- Context-aware output encoding
- XSS prevention measures
- Response header security

**Session Management**:
- Session token generation
- Session timeout policies
- Session fixation prevention
- Concurrent session controls

**Error Handling**:
- Secure error messages (no sensitive data)
- Error logging strategy
- Exception handling standards

**API Security**:
- API authentication methods
- API rate limiting
- Request validation
- Response filtering
```

**Layer 3: Data Security**
- Encryption requirements (see data-protection.md)
- Data classification scheme
- Data masking strategies
- Secure data deletion

**Layer 4: Infrastructure Security**
```markdown
## Infrastructure Security

**Server Hardening**:
- OS hardening standards
- Unnecessary service removal
- Security patch management
- Baseline security configurations

**Container Security** (if applicable):
- Container image scanning
- Runtime security controls
- Container isolation
- Secrets management in containers

**Database Security**:
- Database access controls
- Stored procedure security
- Database encryption
- Audit logging
```

**Security Monitoring & Logging**:
```markdown
## Security Monitoring

**Logging Requirements**:
- Authentication events (success/failure)
- Authorization decisions
- Data access events
- Configuration changes
- Security-relevant errors
- Admin actions

**Log Protection**:
- Centralized log collection
- Log integrity protection
- Log retention policies
- Secure log storage

**Security Monitoring**:
- Real-time security alerts
- Anomaly detection
- Security dashboard
- SIEM integration (if applicable)
```

**Security Testing Requirements**:
- Penetration testing scope
- Vulnerability scanning frequency
- Security code review process
- Security regression testing

### 2. `docs/architecture/threat-model.md`

**Purpose**: Identify threats and mitigation strategies

**Contents**:

**Threat Modeling Methodology**:
- Approach used (STRIDE, PASTA, OCTAVE, etc.)
- Assets identified
- Trust boundaries defined

**Threat Categories (STRIDE)**:

```markdown
## T-001: Spoofing Identity

**Asset**: User authentication system  
**Threat**: Attacker impersonates legitimate user  
**Attack Vectors**:
- Credential theft (phishing, keylogging)
- Session hijacking
- Weak password exploitation

**Impact**: High  
**Likelihood**: Medium  
**Risk Score**: High

**Mitigations**:
- Multi-factor authentication (MFA)
- Strong password policy enforcement
- Account lockout after failed attempts
- Session token security (HTTPOnly, Secure flags)
- IP-based anomaly detection

**Status**: Mitigated  
**Owner**: Security Architect  
**Related Requirements**: NFR-SEC-001, NFR-SEC-002
```

**STRIDE Coverage**:
- **S**poofing: Identity-related threats
- **T**ampering: Data modification threats
- **R**epudiation: Non-accountability threats
- **I**nformation Disclosure: Data leak threats
- **D**enial of Service: Availability threats
- **E**levation of Privilege: Authorization threats

**Threat Catalog**:

| Threat ID | Category | Asset | Impact | Likelihood | Risk | Mitigation |
|-----------|----------|-------|--------|------------|------|------------|
| T-001 | Spoofing | Auth System | High | Medium | High | MFA, Strong passwords |
| T-002 | Tampering | User Data | High | Low | Medium | Input validation, encryption |
| T-003 | Info Disclosure | PII | Critical | Medium | Critical | Encryption, access controls |

**Data Flow Diagram with Trust Boundaries**:
```mermaid
graph LR
    subgraph "Untrusted Zone"
        U[User Browser]
    end
    subgraph "DMZ"
        LB[Load Balancer]
        WAF[Web Application Firewall]
    end
    subgraph "Trusted Zone"
        APP[Application Server]
        DB[(Database)]
    end
    
    U -->|HTTPS| WAF
    WAF --> LB
    LB -->|Internal| APP
    APP -->|Encrypted| DB
```

**Risk Assessment Summary**:
- Critical risks: Count and mitigation status
- High risks: Count and mitigation status
- Medium risks: Count and mitigation status
- Low risks: Count and acceptance decision

### 3. `docs/architecture/authentication-authorization.md`

**Purpose**: Detailed authentication and authorization strategy

**Contents**:

**Authentication Strategy**:

```markdown
## Authentication Methods

**Primary Authentication**:
- **Method**: Username/password with MFA
- **Password Policy**:
  - Minimum length: 12 characters
  - Complexity: Upper, lower, number, special character
  - Password history: Last 12 passwords
  - Expiration: 90 days (or passwordless)
  - Lockout: 5 failed attempts, 30-minute lockout

**Multi-Factor Authentication (MFA)**:
- **Required for**: All users / Admin users / Sensitive operations
- **Methods supported**:
  - TOTP (Time-based One-Time Password)
  - SMS (if appropriate for risk level)
  - Hardware tokens
  - Biometric (if applicable)

**Single Sign-On (SSO)** (if applicable):
- **Protocol**: SAML 2.0 / OAuth 2.0 / OpenID Connect
- **Identity Provider**: [Specify - Okta, Auth0, Azure AD, etc.]
- **Federation**: Enterprise directory integration
```

**Session Management**:
```markdown
## Session Security

**Token Generation**:
- JWT (JSON Web Tokens) with signature
- Cryptographically random session IDs
- Token expiration: 1 hour (access), 7 days (refresh)

**Token Storage**:
- Access tokens: Memory only (not localStorage)
- Refresh tokens: HTTPOnly, Secure cookies
- Token rotation on refresh

**Session Controls**:
- Absolute timeout: 12 hours
- Idle timeout: 30 minutes
- Concurrent session limit: 3 sessions per user
- Session invalidation on password change
```

**Authorization Strategy**:

```markdown
## Authorization Model

**Access Control Model**: RBAC (Role-Based Access Control)

**Roles Defined**:
1. **Anonymous**: Unauthenticated users
   - View public content only
   
2. **User**: Standard authenticated user
   - Access own data
   - Perform standard operations
   
3. **Premium User**: Paid subscriber
   - User permissions +
   - Access premium features
   
4. **Moderator**: Content moderator
   - User permissions +
   - Moderate user-generated content
   
5. **Administrator**: System administrator
   - Full system access
   - User management
   - System configuration

**Permission Matrix**:

| Resource | Anonymous | User | Premium | Moderator | Admin |
|----------|-----------|------|---------|-----------|-------|
| View Public Content | ✓ | ✓ | ✓ | ✓ | ✓ |
| Create Content | ✗ | ✓ | ✓ | ✓ | ✓ |
| Edit Own Content | ✗ | ✓ | ✓ | ✓ | ✓ |
| Delete Own Content | ✗ | ✓ | ✓ | ✓ | ✓ |
| Access Premium Features | ✗ | ✗ | ✓ | ✓ | ✓ |
| Moderate Content | ✗ | ✗ | ✗ | ✓ | ✓ |
| Manage Users | ✗ | ✗ | ✗ | ✗ | ✓ |
| System Config | ✗ | ✗ | ✗ | ✗ | ✓ |
```

**Authorization Implementation**:
- Centralized authorization service
- Permission checks at API gateway level
- Fine-grained permissions in application layer
- Resource-level authorization
- Attribute-based access control (ABAC) for complex scenarios

**API Authentication**:
```markdown
## API Security

**API Key Management** (for service-to-service):
- API key generation and rotation
- Key scope and permissions
- Rate limiting per API key

**OAuth 2.0 Flows** (for third-party integrations):
- Authorization Code Flow (for web apps)
- Client Credentials Flow (for services)
- PKCE for mobile/SPA applications

**Token Validation**:
- Signature verification
- Expiration checking
- Issuer validation
- Audience validation
- Scope validation
```

### 4. `docs/architecture/data-protection.md`

**Purpose**: Data encryption, privacy, and protection measures

**Contents**:

**Data Classification**:

```markdown
## Data Classification Scheme

**Public Data**:
- Description: Data intended for public consumption
- Examples: Marketing content, public documentation
- Protection: Integrity protection
- Encryption: Not required

**Internal Data**:
- Description: Internal business data
- Examples: Business reports, internal documents
- Protection: Access controls
- Encryption: In transit

**Confidential Data**:
- Description: Sensitive business data
- Examples: Financial data, contracts
- Protection: Strong access controls, audit logging
- Encryption: At rest and in transit

**Restricted Data**:
- Description: Highly sensitive data
- Examples: PII, PHI, payment card data, credentials
- Protection: Strictest controls, encryption, audit logging
- Encryption: At rest, in transit, and in use (where possible)
```

**Encryption Strategy**:

```markdown
## Encryption at Rest

**Database Encryption**:
- **Method**: AES-256 encryption
- **Scope**: Entire database / Specific columns
- **Key Management**: Cloud KMS / HSM
- **Fields requiring column-level encryption**:
  - Passwords (hashed with bcrypt/Argon2, not encrypted)
  - Payment card numbers
  - Social security numbers
  - Health records
  - Personal identification numbers

**File Storage Encryption**:
- **Method**: AES-256
- **Scope**: All file uploads containing sensitive data
- **Storage**: Encrypted file system / Cloud storage with encryption

**Backup Encryption**:
- All backups encrypted before storage
- Separate encryption keys from production
- Secure key storage for disaster recovery

## Encryption in Transit

**HTTPS/TLS**:
- **Minimum version**: TLS 1.3 (TLS 1.2 acceptable if 1.3 not available)
- **Cipher suites**: Strong ciphers only
- **Certificate management**: Valid certificates, automatic renewal
- **HSTS**: HTTP Strict Transport Security enabled

**Internal Communication**:
- Service-to-service: mTLS (mutual TLS)
- Database connections: Encrypted connections
- Message queues: TLS encryption

**API Communication**:
- All API calls over HTTPS
- Certificate pinning for mobile apps
- Secure WebSocket connections (WSS)
```

**Key Management**:

```markdown
## Cryptographic Key Management

**Key Generation**:
- Cryptographically secure random number generation
- Minimum key lengths:
  - Symmetric: 256-bit
  - Asymmetric: 2048-bit RSA / 256-bit ECC

**Key Storage**:
- **Production keys**: Hardware Security Module (HSM) or Cloud KMS
- **Development keys**: Secure key vault (never in code)
- **Key rotation**: Automated rotation every 90 days
- **Key backup**: Encrypted backup, stored separately

**Key Access Control**:
- Principle of least privilege
- Separation of duties
- Audit logging of key access
```

**Privacy & Compliance**:

```markdown
## Privacy Protection

**Personal Identifiable Information (PII)**:
- PII inventory maintained
- PII minimization principle
- Data retention policies
- Right to erasure implementation (GDPR)
- Data portability support
- Consent management

**Privacy by Design**:
- Data minimization
- Purpose limitation
- Storage limitation
- Accuracy maintenance
- Integrity and confidentiality

**Compliance Requirements**:

**GDPR** (if applicable):
- Lawful basis for processing
- Data subject rights implementation
- Data protection impact assessment
- Privacy notices
- Data breach notification process (72 hours)

**HIPAA** (if applicable):
- Administrative safeguards
- Physical safeguards
- Technical safeguards
- Business associate agreements
- Audit controls

**PCI DSS** (if handling payment cards):
- Cardholder data environment (CDE) isolation
- No storage of sensitive authentication data
- Strong access controls
- Regular security testing
- Security policy maintenance

**Other Regulations**:
- CCPA (California)
- SOC 2 compliance
- Industry-specific regulations
```

**Data Sanitization**:
- Data masking for non-production environments
- Secure data deletion procedures
- Data anonymization techniques
- Pseudonymization strategies

---

## Quality Criteria

Before completing this role, ensure:

- [ ] All security requirements from NFRs are addressed
- [ ] Threat model covers all critical assets
- [ ] Authentication mechanisms are robust and modern
- [ ] Authorization model is clearly defined with permission matrix
- [ ] Encryption strategy covers data at rest, in transit, and in use
- [ ] Key management strategy is secure and practical
- [ ] Compliance requirements are documented with implementation plan
- [ ] Security monitoring and logging is comprehensive
- [ ] Security testing strategy is defined
- [ ] All high and critical risks have mitigation plans
- [ ] Security controls align with industry best practices
- [ ] Privacy requirements are addressed
- [ ] Incident response considerations are documented

---

## Security Standards & Frameworks

Reference and align with:

**Industry Standards**:
- OWASP Top 10 (Web Application Security)
- OWASP Mobile Top 10 (Mobile Security)
- OWASP API Security Top 10
- CIS Benchmarks (system hardening)
- NIST Cybersecurity Framework
- ISO 27001 (Information Security Management)

**Cloud Security**:
- AWS Well-Architected Framework (Security Pillar)
- Azure Security Benchmark
- GCP Security Best Practices

---

## Transition to Next Roles

The Security Architect's outputs inform:

**To Database Designer**:
- Data protection requirements → Database security implementation
- Encryption requirements → Database encryption configuration

**To API Designer**:
- Authentication/authorization → API security implementation
- API security requirements → Endpoint security

**To DevOps Engineer**:
- Infrastructure security → Secure deployment practices
- Key management → Secrets management in CI/CD

**To Test Architect**:
- Security requirements → Security test cases
- Threat model → Penetration testing scenarios

---

## Tips for Success

1. **Think Like an Attacker**: Consider how systems can be compromised
2. **Defense in Depth**: Multiple layers of security
3. **Secure by Default**: Security should not be optional
4. **Least Privilege**: Grant minimum necessary access
5. **Assume Breach**: Design for detection and response
6. **Keep It Simple**: Complex security is often weak security
7. **Stay Current**: Monitor security advisories and CVEs
8. **Document Everything**: Security through obscurity doesn't work
9. **Compliance Early**: Address compliance from the start
10. **Security is Continuous**: Not a one-time activity

---

**Previous Role**: [System Architect](./03-system-architect.md)  
**Next Role**: [UX/UI Designer](./05-ux-ui-designer.md)
