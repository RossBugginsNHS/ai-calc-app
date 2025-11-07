# Security Testing Plan

**Project**: Basic Calculator Web App  
**Version**: 1.0  
**Date**: 2025-11-07  
**Author**: Security Architect  
**Status**: Draft

---

## Executive Summary

This document outlines the comprehensive security testing strategy for the Basic Calculator Web App. The testing approach combines automated and manual security testing throughout the development lifecycle, following industry best practices (OWASP, NIST) and targeting OWASP ASVS Level 2 compliance.

**Testing Approach:**

- **Shift-Left Security**: Security testing integrated into CI/CD pipeline
- **Continuous Testing**: Automated scans on every commit
- **Defense in Depth**: Multiple testing tools and methodologies
- **Risk-Based**: Prioritize testing based on threat model
- **Comprehensive Coverage**: SAST, DAST, dependency scanning, penetration testing

---

## Table of Contents

1. [Testing Strategy](#1-testing-strategy)
2. [Static Application Security Testing (SAST)](#2-static-application-security-testing-sast)
3. [Dynamic Application Security Testing (DAST)](#3-dynamic-application-security-testing-dast)
4. [Dependency Security Testing](#4-dependency-security-testing)
5. [Penetration Testing](#5-penetration-testing)
6. [Security Code Review](#6-security-code-review)
7. [API Security Testing](#7-api-security-testing)
8. [Authentication Testing](#8-authentication-testing)
9. [Infrastructure Security Testing](#9-infrastructure-security-testing)
10. [Test Execution Schedule](#10-test-execution-schedule)

---

## 1. Testing Strategy

### 1.1 Testing Principles

**1. Automated First**: Automate repetitive security tests in CI/CD  
**2. Continuous Testing**: Run security tests on every code change  
**3. Fast Feedback**: Fail fast when critical vulnerabilities detected  
**4. Comprehensive Coverage**: Test all layers (frontend, API, infrastructure)  
**5. Risk-Based Prioritization**: Focus on high-risk areas first

### 1.2 Testing Pyramid

```
            ┌─────────────────────┐
            │ Penetration Testing │ Quarterly
            │   (Manual, External)│
            └─────────────────────┘
                     ▲
            ┌─────────────────────┐
            │ DAST (Dynamic Scan) │ Daily (Staging)
            │  OWASP ZAP, Burp    │
            └─────────────────────┘
                     ▲
        ┌────────────────────────────┐
        │ SAST (Static Analysis)     │ Every Commit
        │ ESLint, Semgrep, npm audit │
        └────────────────────────────┘
                     ▲
    ┌────────────────────────────────────┐
    │ Unit Security Tests               │ Every Commit
    │ Input validation, auth logic tests│
    └────────────────────────────────────┘
```

### 1.3 Testing Scope

**In Scope:**

- Frontend application (TypeScript, vanilla JS)
- Backend API (AWS Lambda, Node.js)
- Authentication mechanisms (JWT)
- Database queries (Prisma ORM)
- Third-party integrations (Stripe)
- Infrastructure configuration (Terraform)

**Out of Scope:**

- AWS managed services internal security (AWS responsibility)
- Stripe payment processing internals (Stripe responsibility)
- Physical security (datacenter security)

### 1.4 Success Criteria

**Pre-Production Requirements:**

- ✅ Zero critical or high vulnerabilities in SAST
- ✅ Zero critical vulnerabilities in DAST
- ✅ All dependencies up-to-date (no known CVEs)
- ✅ Penetration test completed with all findings remediated
- ✅ Security code review completed
- ✅ Security testing documentation complete

**Ongoing Requirements:**

- ✅ SAST passes on every commit (no new vulnerabilities)
- ✅ Weekly DAST scan shows no regressions
- ✅ Monthly dependency updates applied
- ✅ Quarterly penetration testing completed

---

## 2. Static Application Security Testing (SAST)

### 2.1 SAST Tools

**Primary Tools:**

1. **ESLint** (JavaScript/TypeScript)
   - `eslint-plugin-security` - Detect security issues
   - `eslint-plugin-no-secrets` - Find hardcoded secrets
   - Custom rules for project-specific patterns

2. **Semgrep** (Multi-language)
   - OWASP Top 10 rules
   - Custom security patterns
   - Open-source security rules from Semgrep Registry

3. **TypeScript Compiler**
   - Strict mode enabled
   - No `any` types allowed
   - Null safety checks

### 2.2 SAST Test Cases

**Category 1: Injection Vulnerabilities**

| Test Case | Tool | Expected Result |
|-----------|------|-----------------|
| SQL injection patterns | Semgrep | No raw SQL queries, Prisma only |
| Command injection | ESLint | No `child_process.exec()` or `eval()` |
| XSS vulnerabilities | ESLint | No `innerHTML`, use `textContent` |
| NoSQL injection | Semgrep | Input validation on all queries |

**Category 2: Authentication & Session Management**

| Test Case | Tool | Expected Result |
|-----------|------|-----------------|
| Hardcoded credentials | ESLint no-secrets | No passwords in code |
| Weak crypto algorithms | Semgrep | No MD5, SHA1 for passwords |
| JWT secret management | Code review | Secrets in AWS Secrets Manager |
| Session fixation | Code review | JWT regeneration on privilege change |

**Category 3: Sensitive Data Exposure**

| Test Case | Tool | Expected Result |
|-----------|------|-----------------|
| Logging sensitive data | Semgrep | No passwords, tokens in logs |
| Insecure randomness | ESLint | Use `crypto.randomBytes()`, not `Math.random()` |
| Sensitive data in URLs | Code review | No PII in query parameters |

**Category 4: Security Misconfiguration**

| Test Case | Tool | Expected Result |
|-----------|------|-----------------|
| CORS misconfiguration | Code review | Whitelist origins only |
| Debug mode in production | Code review | `NODE_ENV=production` |
| Error stack traces | Code review | Generic errors in production |

### 2.3 SAST Configuration

**ESLint Security Rules (.eslintrc.js):**

```javascript
module.exports = {
  plugins: ['security', 'no-secrets'],
  extends: ['plugin:security/recommended'],
  rules: {
    'security/detect-object-injection': 'error',
    'security/detect-non-literal-regexp': 'error',
    'security/detect-unsafe-regex': 'error',
    'security/detect-buffer-noassert': 'error',
    'security/detect-child-process': 'error',
    'security/detect-disable-mustache-escape': 'error',
    'security/detect-eval-with-expression': 'error',
    'security/detect-no-csrf-before-method-override': 'error',
    'security/detect-non-literal-fs-filename': 'error',
    'security/detect-non-literal-require': 'error',
    'security/detect-possible-timing-attacks': 'error',
    'security/detect-pseudoRandomBytes': 'error',
    'no-secrets/no-secrets': 'error',
  }
};
```

**Semgrep Rules (semgrep.yml):**

```yaml
rules:
  - id: hardcoded-password
    pattern: password = "..."
    message: Hardcoded password detected
    severity: ERROR
    languages: [javascript, typescript]

  - id: sql-injection
    pattern: db.raw($QUERY)
    message: Raw SQL query detected, use ORM
    severity: ERROR
    languages: [javascript, typescript]

  - id: jwt-no-verify
    pattern: jwt.decode($TOKEN)
    message: JWT token not verified
    severity: ERROR
    languages: [javascript, typescript]
```

### 2.4 CI/CD Integration

**GitHub Actions Workflow (.github/workflows/security-scan.yml):**

```yaml
name: Security Scan

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  sast:
    name: Static Application Security Testing
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: ESLint Security Scan
        run: npm run lint:security
      
      - name: Semgrep Scan
        uses: returntocorp/semgrep-action@v1
        with:
          config: auto
      
      - name: TypeScript Type Check
        run: npm run type-check
      
      - name: Upload SARIF results
        uses: github/codeql-action/upload-sarif@v2
        if: always()
        with:
          sarif_file: semgrep-results.sarif
```

---

## 3. Dynamic Application Security Testing (DAST)

### 3.1 DAST Tools

**Primary Tool: OWASP ZAP (Zed Attack Proxy)**

**Alternative: Burp Suite Professional** (manual testing)

### 3.2 DAST Test Cases

**OWASP Top 10 Coverage:**

| OWASP Risk | Test Method | Expected Result |
|------------|-------------|-----------------|
| **A01: Broken Access Control** | Forced browsing, parameter manipulation | 401/403 for unauthorized access |
| **A02: Cryptographic Failures** | TLS scan, cookie security | TLS 1.3, Secure/HttpOnly cookies |
| **A03: Injection** | SQL injection, XSS payloads | Input validation blocks all payloads |
| **A04: Insecure Design** | Business logic testing | Rate limits, proper auth flows |
| **A05: Security Misconfiguration** | Header analysis, error messages | Security headers present, generic errors |
| **A06: Vulnerable Components** | Dependency scan | No known CVEs in dependencies |
| **A07: Authentication Failures** | Brute force, session management | Account lockout, JWT expiry enforced |
| **A08: Data Integrity Failures** | JWT tampering, webhook verification | Invalid signatures rejected |
| **A09: Logging Failures** | Log analysis | Security events logged |
| **A10: SSRF** | URL manipulation | No internal network access |

### 3.3 DAST Scan Configuration

**OWASP ZAP Automated Scan:**

```bash
#!/bin/bash
# dast-scan.sh

# Start ZAP in daemon mode
docker run -d --name zap \
  -p 8090:8090 \
  -v $(pwd)/zap-reports:/zap/wrk \
  owasp/zap2docker-stable \
  zap.sh -daemon -host 0.0.0.0 -port 8090 -config api.disablekey=true

# Wait for ZAP to start
sleep 30

# Spider the application (discover endpoints)
curl "http://localhost:8090/JSON/spider/action/scan/?url=https://staging.calcapp.com"

# Wait for spider to complete
while [ $(curl -s "http://localhost:8090/JSON/spider/view/status/" | jq -r '.status') != "100" ]; do
  sleep 5
done

# Active scan (inject attack payloads)
curl "http://localhost:8090/JSON/ascan/action/scan/?url=https://staging.calcapp.com"

# Wait for active scan to complete
while [ $(curl -s "http://localhost:8090/JSON/ascan/view/status/" | jq -r '.status') != "100" ]; do
  sleep 10
done

# Generate HTML report
curl "http://localhost:8090/OTHER/core/other/htmlreport/" > zap-reports/report.html

# Generate JSON report
curl "http://localhost:8090/JSON/core/view/alerts/" > zap-reports/alerts.json

# Check for high-risk vulnerabilities
HIGH_RISK=$(curl -s "http://localhost:8090/JSON/core/view/alerts/" | jq '[.alerts[] | select(.risk == "High")] | length')

if [ "$HIGH_RISK" -gt 0 ]; then
  echo "FAIL: $HIGH_RISK high-risk vulnerabilities found"
  exit 1
else
  echo "PASS: No high-risk vulnerabilities found"
fi

# Stop ZAP
docker stop zap && docker rm zap
```

### 3.4 Manual DAST Testing

**Authentication Testing:**

```bash
# Test 1: Login with invalid credentials
curl -X POST https://api.calcapp.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"wrongpassword"}'
# Expected: 401 Unauthorized, generic error message

# Test 2: Brute force protection
for i in {1..10}; do
  curl -X POST https://api.calcapp.com/v1/auth/login \
    -H "Content-Type: application/json" \
    -d '{"email":"test@example.com","password":"wrong'$i'"}'
done
# Expected: Account lockout after 5 attempts

# Test 3: JWT tampering
curl -X GET https://api.calcapp.com/v1/history \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyXzk5OTk5OSIsImVtYWlsIjoiYWxpY2VAZXhhbXBsZS5jb20iLCJ0aWVyIjoicHJlbWl1bSJ9.invalid_signature"
# Expected: 401 Unauthorized

# Test 4: Expired JWT
curl -X GET https://api.calcapp.com/v1/history \
  -H "Authorization: Bearer <expired_token>"
# Expected: 401 Unauthorized
```

**Authorization Testing:**

```bash
# Test 1: Access other user's data
curl -X GET https://api.calcapp.com/v1/history?userId=other_user_123 \
  -H "Authorization: Bearer <valid_token_user_456>"
# Expected: 403 Forbidden (can only access own data)

# Test 2: Free user accessing premium endpoint
curl -X GET https://api.calcapp.com/v1/history \
  -H "Authorization: Bearer <free_tier_token>"
# Expected: 403 Forbidden (premium feature)

# Test 3: Delete other user's history item
curl -X DELETE https://api.calcapp.com/v1/history/other_user_item_id \
  -H "Authorization: Bearer <valid_token>"
# Expected: 403 Forbidden
```

---

## 4. Dependency Security Testing

### 4.1 Tools

**1. npm audit** (built-in)  
**2. Snyk** (enhanced vulnerability detection)  
**3. Dependabot** (automated PR for updates)

### 4.2 Test Cases

| Test Case | Tool | Frequency | Threshold |
|-----------|------|-----------|-----------|
| Known CVEs in dependencies | npm audit | Every commit | Zero high/critical |
| Outdated packages | npm outdated | Weekly | Update within 7 days |
| License compliance | license-checker | Monthly | Only approved licenses |
| Malicious packages | Snyk | Every commit | Zero |

### 4.3 Dependency Scan Configuration

**npm audit in CI/CD:**

```yaml
- name: Dependency Security Scan
  run: |
    npm audit --audit-level=moderate
    if [ $? -ne 0 ]; then
      echo "FAIL: Vulnerabilities found in dependencies"
      exit 1
    fi
```

**Snyk Integration:**

```yaml
- name: Snyk Security Scan
  uses: snyk/actions/node@master
  env:
    SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
  with:
    args: --severity-threshold=high
```

**Dependabot Configuration (.github/dependabot.yml):**

```yaml
version: 2
updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: weekly
    open-pull-requests-limit: 10
    reviewers:
      - security-team
    labels:
      - security
      - dependencies
```

### 4.4 Dependency Review Process

**Before Adding New Dependency:**

1. ✅ Check package popularity (npm downloads, GitHub stars)
2. ✅ Review package maintenance (last update, open issues)
3. ✅ Check for known vulnerabilities (Snyk, npm audit)
4. ✅ Review package code (look for suspicious behavior)
5. ✅ Check license compatibility (MIT, Apache 2.0, ISC)
6. ✅ Evaluate alternatives (do we really need this package?)

**Approved Dependency List:**

- **Allowed Licenses**: MIT, Apache 2.0, ISC, BSD-3-Clause
- **Forbidden Licenses**: GPL, AGPL (copyleft concerns)

---

## 5. Penetration Testing

### 5.1 Penetration Testing Scope

**Quarterly External Penetration Test**

**Test Scope:**

- Web application (https://calcapp.com)
- API endpoints (https://api.calcapp.com/v1/*)
- Authentication mechanisms
- Payment flow (Stripe integration)
- Session management
- Data protection

**Out of Scope:**

- AWS infrastructure (managed by AWS)
- Stripe payment processing (managed by Stripe)
- Physical security
- Social engineering attacks

### 5.2 Penetration Testing Methodology

**Methodology: OWASP Testing Guide v4.2**

**Phases:**

1. **Reconnaissance** (Passive)
   - DNS enumeration
   - Subdomain discovery
   - Technology stack identification
   - Public information gathering

2. **Scanning** (Active)
   - Port scanning
   - Service enumeration
   - Vulnerability scanning
   - SSL/TLS analysis

3. **Exploitation**
   - Authentication bypass attempts
   - Authorization flaws
   - Injection attacks (SQL, XSS, command)
   - Business logic vulnerabilities
   - Session management issues

4. **Post-Exploitation**
   - Privilege escalation attempts
   - Data exfiltration attempts
   - Persistence mechanisms

5. **Reporting**
   - Executive summary
   - Vulnerability details (severity, CVSS, PoC)
   - Remediation recommendations
   - Retesting results

### 5.3 Penetration Testing Checklist

**Authentication Testing:**

- [ ] Weak password policy
- [ ] Brute force protection
- [ ] Account enumeration
- [ ] Password reset vulnerabilities
- [ ] Session fixation
- [ ] JWT token security
- [ ] Multi-factor authentication bypass (future)

**Authorization Testing:**

- [ ] Insecure direct object references (IDOR)
- [ ] Missing function-level access control
- [ ] Privilege escalation (vertical/horizontal)
- [ ] Forced browsing

**Input Validation:**

- [ ] SQL injection
- [ ] Cross-site scripting (XSS)
- [ ] Command injection
- [ ] Path traversal
- [ ] XML injection
- [ ] LDAP injection

**Session Management:**

- [ ] Session token entropy
- [ ] Session fixation
- [ ] Session timeout
- [ ] Logout functionality
- [ ] Concurrent sessions

**Business Logic:**

- [ ] Payment bypass
- [ ] Subscription manipulation
- [ ] Race conditions
- [ ] Rate limiting bypass

**Information Disclosure:**

- [ ] Error messages
- [ ] Directory listing
- [ ] Source code disclosure
- [ ] Sensitive data in URLs
- [ ] PII exposure

**Configuration:**

- [ ] Security headers (CSP, HSTS, etc.)
- [ ] CORS configuration
- [ ] TLS/SSL configuration
- [ ] Default credentials
- [ ] Debug mode enabled

### 5.4 Penetration Testing Report

**Report Structure:**

1. **Executive Summary**
   - Overall risk rating
   - Key findings
   - Business impact

2. **Vulnerability Details**
   - Description
   - Severity (Critical, High, Medium, Low)
   - CVSS score
   - Proof of concept
   - Affected components
   - Business impact
   - Remediation steps

3. **Technical Details**
   - HTTP requests/responses
   - Screenshots
   - Exploitation steps

4. **Remediation Recommendations**
   - Short-term fixes
   - Long-term improvements
   - Prioritization

5. **Retest Results**
   - Verified fixes
   - Remaining issues

**Vulnerability Severity Matrix:**

| CVSS Score | Severity | Response Time | Example |
|------------|----------|---------------|---------|
| 9.0 - 10.0 | **Critical** | < 24 hours | SQL injection, RCE |
| 7.0 - 8.9 | **High** | < 7 days | XSS, authentication bypass |
| 4.0 - 6.9 | **Medium** | < 30 days | CSRF, information disclosure |
| 0.1 - 3.9 | **Low** | < 90 days | Security headers missing |

---

## 6. Security Code Review

### 6.1 Code Review Checklist

**Authentication & Authorization:**

- [ ] Passwords stored with bcrypt (cost 12+)
- [ ] JWT tokens properly validated (signature, expiry, claims)
- [ ] Authorization checks on all endpoints
- [ ] No hardcoded credentials
- [ ] Secrets in environment variables/AWS Secrets Manager

**Input Validation:**

- [ ] All user inputs validated server-side
- [ ] Whitelist validation (allow known-good)
- [ ] Input length limits enforced
- [ ] No `eval()` or `Function()` constructors
- [ ] Prisma ORM used (no raw SQL)

**Output Encoding:**

- [ ] No `innerHTML` (use `textContent`)
- [ ] User data sanitized before display
- [ ] Content Security Policy configured
- [ ] Error messages generic (no stack traces)

**Session Management:**

- [ ] JWT tokens short-lived (15 minutes)
- [ ] Refresh tokens stored in httpOnly cookies
- [ ] SameSite=Strict cookie attribute
- [ ] Logout invalidates refresh tokens

**Data Protection:**

- [ ] HTTPS enforced (HSTS header)
- [ ] Sensitive data encrypted at rest
- [ ] No PII in logs
- [ ] Database connection strings in secrets

**Error Handling:**

- [ ] Try-catch blocks around async operations
- [ ] Generic error messages to users
- [ ] Detailed errors logged server-side
- [ ] No sensitive data in error messages

**Logging & Monitoring:**

- [ ] Security events logged (login, logout, failures)
- [ ] Audit trail for sensitive operations
- [ ] No passwords or tokens in logs
- [ ] Log retention policy enforced

### 6.2 Peer Review Process

**All Code Changes Require:**

1. ✅ Peer review by another developer
2. ✅ Security review for auth/payment changes
3. ✅ Automated tests pass
4. ✅ SAST scan passes
5. ✅ Approval before merge

**Security-Critical Files** (require security team review):

- Authentication logic (`src/auth/*`)
- Payment processing (`src/payment/*`)
- Database migrations (`prisma/migrations/*`)
- Infrastructure code (`terraform/*`)
- Security configurations (`.github/workflows/security-*.yml`)

---

## 7. API Security Testing

### 7.1 API Security Test Cases

**Test Suite:**

```typescript
// tests/security/api-security.test.ts

describe('API Security', () => {
  describe('Authentication', () => {
    it('should reject requests without JWT token', async () => {
      const res = await request(app).get('/v1/history');
      expect(res.status).toBe(401);
      expect(res.body.error.code).toBe('AUTH_REQUIRED');
    });

    it('should reject requests with invalid JWT token', async () => {
      const res = await request(app)
        .get('/v1/history')
        .set('Authorization', 'Bearer invalid_token');
      expect(res.status).toBe(401);
    });

    it('should reject requests with expired JWT token', async () => {
      const expiredToken = generateExpiredToken();
      const res = await request(app)
        .get('/v1/history')
        .set('Authorization', `Bearer ${expiredToken}`);
      expect(res.status).toBe(401);
    });
  });

  describe('Authorization', () => {
    it('should prevent access to other users data', async () => {
      const userAToken = await loginAsUser('alice@example.com');
      const userBItem = await createHistoryItem('bob@example.com');
      
      const res = await request(app)
        .delete(`/v1/history/${userBItem.id}`)
        .set('Authorization', `Bearer ${userAToken}`);
      
      expect(res.status).toBe(403);
    });

    it('should prevent free users from accessing premium features', async () => {
      const freeToken = await loginAsFreeUser();
      const res = await request(app)
        .get('/v1/history')
        .set('Authorization', `Bearer ${freeToken}`);
      
      expect(res.status).toBe(403);
      expect(res.body.error.code).toBe('PREMIUM_REQUIRED');
    });
  });

  describe('Input Validation', () => {
    it('should reject SQL injection attempts', async () => {
      const res = await request(app)
        .post('/v1/auth/login')
        .send({
          email: "admin'--",
          password: 'anything'
        });
      
      expect(res.status).toBe(400);
    });

    it('should reject XSS payloads', async () => {
      const token = await loginAsUser('alice@example.com');
      const res = await request(app)
        .post('/v1/history')
        .set('Authorization', `Bearer ${token}`)
        .send({
          expression: '<script>alert("XSS")</script>',
          result: '0'
        });
      
      expect(res.status).toBe(400);
    });

    it('should enforce maximum input length', async () => {
      const token = await loginAsUser('alice@example.com');
      const longExpression = 'a'.repeat(10000);
      const res = await request(app)
        .post('/v1/history')
        .set('Authorization', `Bearer ${token}`)
        .send({
          expression: longExpression,
          result: '0'
        });
      
      expect(res.status).toBe(400);
    });
  });

  describe('Rate Limiting', () => {
    it('should enforce rate limits', async () => {
      const token = await loginAsUser('alice@example.com');
      
      // Make 101 requests (limit is 100/minute)
      const requests = Array(101).fill(null).map(() =>
        request(app)
          .get('/v1/history')
          .set('Authorization', `Bearer ${token}`)
      );
      
      const responses = await Promise.all(requests);
      const rateLimitedResponses = responses.filter(r => r.status === 429);
      
      expect(rateLimitedResponses.length).toBeGreaterThan(0);
    });
  });

  describe('Security Headers', () => {
    it('should include security headers', async () => {
      const res = await request(app).get('/v1/health');
      
      expect(res.headers['x-content-type-options']).toBe('nosniff');
      expect(res.headers['x-frame-options']).toBe('DENY');
      expect(res.headers['strict-transport-security']).toContain('max-age=31536000');
    });
  });
});
```

---

## 8. Authentication Testing

### 8.1 Authentication Test Cases

**Test Matrix:**

| Test Case | Expected Result | Priority |
|-----------|-----------------|----------|
| Login with valid credentials | 200 OK, JWT returned | Critical |
| Login with invalid password | 401 Unauthorized | Critical |
| Login with non-existent email | 401 Unauthorized (generic message) | Critical |
| Login with weak password | 400 Bad Request (on registration) | High |
| Brute force protection | Account lockout after 5 attempts | Critical |
| JWT token expiry | 401 after 15 minutes | Critical |
| Refresh token rotation | New access token issued | High |
| Logout invalidates refresh token | Cannot refresh after logout | High |
| Password reset token expiry | Invalid after 1 hour | Medium |
| Multiple concurrent sessions | Allowed on different devices | Medium |

### 8.2 Password Security Tests

```typescript
describe('Password Security', () => {
  it('should enforce minimum password length', async () => {
    const res = await request(app)
      .post('/v1/auth/register')
      .send({
        email: 'test@example.com',
        password: 'short'
      });
    
    expect(res.status).toBe(400);
    expect(res.body.error.message).toContain('at least 8 characters');
  });

  it('should require password complexity', async () => {
    const weakPasswords = ['password123', 'ALLCAPS123', '12345678'];
    
    for (const password of weakPasswords) {
      const res = await request(app)
        .post('/v1/auth/register')
        .send({
          email: 'test@example.com',
          password: password
        });
      
      expect(res.status).toBe(400);
    }
  });

  it('should hash passwords with bcrypt', async () => {
    await registerUser('test@example.com', 'SecurePass123!');
    const user = await db.user.findUnique({
      where: { email: 'test@example.com' }
    });
    
    expect(user.password).not.toBe('SecurePass123!');
    expect(user.password).toMatch(/^\$2[aby]\$\d+\$/); // bcrypt pattern
  });
});
```

---

## 9. Infrastructure Security Testing

### 9.1 Infrastructure Test Cases

**Terraform Security Scanning:**

```bash
# Checkov - Infrastructure as Code security scanner
checkov -d terraform/ --framework terraform

# TFLint - Terraform linter
tflint terraform/
```

**AWS Security Checks:**

| Check | Tool | Expected Result |
|-------|------|-----------------|
| Public S3 buckets | AWS Config | Only website bucket public |
| IAM policies | IAM Policy Simulator | Least privilege enforced |
| Security groups | AWS Inspector | No unrestricted inbound rules |
| RDS encryption | AWS Config | Encryption enabled |
| CloudWatch logging | AWS Config | All services logging |

### 9.2 Infrastructure Security Checklist

**AWS Security Best Practices:**

- [ ] MFA enabled on root account
- [ ] IAM users have individual accounts (no shared)
- [ ] Least privilege IAM policies
- [ ] S3 buckets not publicly accessible (except website)
- [ ] RDS database in private subnet
- [ ] RDS encryption at rest enabled
- [ ] VPC flow logs enabled
- [ ] CloudTrail enabled (audit log)
- [ ] AWS Config enabled (compliance monitoring)
- [ ] Security groups follow least privilege
- [ ] No hardcoded credentials in Terraform
- [ ] KMS keys for encryption
- [ ] Secrets Manager for sensitive data

---

## 10. Test Execution Schedule

### 10.1 Continuous Testing (Automated)

**Every Code Commit:**

- ✅ SAST (ESLint, Semgrep)
- ✅ TypeScript type checking
- ✅ Unit security tests
- ✅ Dependency scan (npm audit, Snyk)

**Daily (Staging Environment):**

- ✅ DAST (OWASP ZAP full scan)
- ✅ API security tests
- ✅ Integration security tests

**Weekly:**

- ✅ Dependency updates check
- ✅ Infrastructure security scan (Checkov, TFLint)
- ✅ Manual API security testing

**Monthly:**

- ✅ Security code review (random sample)
- ✅ Dependency update sprint
- ✅ Security metrics review

**Quarterly:**

- ✅ External penetration testing
- ✅ Security architecture review
- ✅ Incident response drill
- ✅ Security training for team

### 10.2 Pre-Production Testing

**Before Production Deploy:**

1. ✅ All SAST checks pass (zero critical/high)
2. ✅ DAST scan completed (zero critical vulnerabilities)
3. ✅ Penetration test completed (all findings remediated)
4. ✅ Security code review approved
5. ✅ Dependency audit clean
6. ✅ Infrastructure security review passed
7. ✅ Security documentation complete

### 10.3 Testing Metrics

**Track and Report:**

| Metric | Target | Measurement |
|--------|--------|-------------|
| Time to fix critical vulnerabilities | < 24 hours | Jira ticket resolution time |
| SAST false positive rate | < 10% | Manual review results |
| Code coverage (security tests) | > 80% | Jest coverage report |
| Dependency vulnerabilities | 0 high/critical | npm audit, Snyk dashboard |
| DAST scan duration | < 2 hours | ZAP scan logs |
| Penetration test findings (quarterly) | Trend down | Pentest reports |

---

## Summary

This security testing plan provides comprehensive coverage of the Basic Calculator Web App through:

1. **Automated Testing**: SAST, DAST, dependency scans in CI/CD
2. **Manual Testing**: Code reviews, penetration testing, API security testing
3. **Continuous Monitoring**: Daily scans, weekly reviews, quarterly audits
4. **Risk-Based Approach**: Prioritize critical vulnerabilities, fail fast
5. **Comprehensive Coverage**: All layers (frontend, API, infrastructure, dependencies)

**Next Steps:**

1. Implement SAST tools in CI/CD pipeline
2. Set up weekly DAST scans
3. Schedule first penetration test (pre-production)
4. Establish security testing metrics dashboard
5. Train development team on security testing practices

---

**Document Approval:**

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Security Architect | [TBD] | 2025-11-07 | [Pending] |
| Technical Lead | [TBD] | [Pending] | [Pending] |
| QA Lead | [TBD] | [Pending] | [Pending] |

---

**Change Log:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-07 | Security Architect | Initial draft |
