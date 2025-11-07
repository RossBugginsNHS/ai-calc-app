# Security Runbook

**Project**: Basic Calculator Web App  
**Version**: 1.0  
**Date**: 2025-11-07  
**Author**: Security Architect  
**Status**: Draft

---

## Executive Summary

This Security Runbook provides operational procedures for common security tasks and scenarios. It serves as a practical guide for developers, DevOps engineers, and security team members to perform security-related operations safely and consistently.

**Purpose:**

- **Standardize** security operations
- **Reduce errors** through documented procedures
- **Enable** rapid response to security events
- **Train** new team members on security practices
- **Maintain** security posture through regular tasks

---

## Table of Contents

1. [Daily Security Operations](#1-daily-security-operations)
2. [Authentication Operations](#2-authentication-operations)
3. [Access Control Operations](#3-access-control-operations)
4. [Secrets Management](#4-secrets-management)
5. [Monitoring and Alerting](#5-monitoring-and-alerting)
6. [Incident Response Procedures](#6-incident-response-procedures)
7. [Security Hardening](#7-security-hardening)
8. [Compliance Operations](#8-compliance-operations)
9. [Emergency Procedures](#9-emergency-procedures)
10. [Security Maintenance](#10-security-maintenance)

---

## 1. Daily Security Operations

### 1.1 Morning Security Check

**Frequency:** Daily (start of business day)  
**Owner:** Security Lead / On-call Engineer  
**Duration:** 15 minutes

**Checklist:**

```bash
#!/bin/bash
# daily-security-check.sh

echo "=== Daily Security Check ==="
echo "Date: $(date)"

# 1. Check for CloudWatch alarms
echo "\n1. CloudWatch Alarms:"
aws cloudwatch describe-alarms \
  --state-value ALARM \
  --query 'MetricAlarms[*].[AlarmName,StateReason]' \
  --output table

# 2. Review failed login attempts (last 24 hours)
echo "\n2. Failed Login Attempts:"
aws logs filter-log-events \
  --log-group-name /aws/lambda/auth \
  --filter-pattern '"auth.login.failed"' \
  --start-time $(($(date +%s) - 86400))000 \
  | jq '.events | length'

# 3. Check for new security vulnerabilities (npm audit)
echo "\n3. Dependency Vulnerabilities:"
npm audit --json | jq '{critical: .metadata.vulnerabilities.critical, high: .metadata.vulnerabilities.high}'

# 4. Review WAF blocked requests
echo "\n4. WAF Blocked Requests (last 24 hours):"
aws wafv2 get-sampled-requests \
  --web-acl-id <web-acl-id> \
  --rule-metric-name BlockedRequests \
  --scope CLOUDFRONT \
  --time-window '{"StartTime": <24-hours-ago>, "EndTime": <now>}' \
  | jq '.SampledRequests | length'

# 5. Check AWS GuardDuty findings
echo "\n5. GuardDuty Findings:"
aws guardduty list-findings \
  --detector-id <detector-id> \
  --finding-criteria '{"Criterion":{"severity":{"Gte":4}}}' \
  | jq '.FindingIds | length'

# 6. Review certificate expiry
echo "\n6. SSL Certificate Expiry:"
aws acm describe-certificate \
  --certificate-arn <cert-arn> \
  --query 'Certificate.NotAfter' \
  --output text

echo "\n=== Check Complete ==="
```

**Expected Results:**

- ✅ No active CloudWatch alarms
- ✅ Failed logins < 50 (threshold for investigation)
- ✅ Zero critical/high npm vulnerabilities
- ✅ WAF blocks < 1000 (normal traffic)
- ✅ Zero high-severity GuardDuty findings
- ✅ Certificate expires > 30 days from now

**Escalation:** If any check fails, follow incident response procedures.

### 1.2 Review Security Logs

**Frequency:** Daily  
**Owner:** Security Lead  
**Duration:** 30 minutes

**Procedure:**

```bash
# 1. Open CloudWatch Insights
# Navigate to: AWS Console > CloudWatch > Logs Insights

# 2. Query authentication anomalies
fields @timestamp, userEmail, ipAddress, userAgent, status
| filter event = "auth.login"
| filter status = "failed"
| stats count() by ipAddress
| sort count desc
| limit 20

# 3. Query unusual API patterns
fields @timestamp, userId, endpoint, method, statusCode
| filter statusCode >= 400
| stats count() by userId, endpoint
| sort count desc
| limit 50

# 4. Query data access patterns
fields @timestamp, userId, action, resource
| filter action in ["history.export", "data.delete", "subscription.cancel"]
| sort @timestamp desc
| limit 100
```

**Investigate if:**

- Same IP has >10 failed logins
- User has >50 4xx errors in one hour
- Unusual data export requests (outside business hours)
- Multiple account deletions in short timeframe

---

## 2. Authentication Operations

### 2.1 Reset User Password (Support Request)

**Scenario:** User forgot password or account locked

**Procedure:**

```bash
# Step 1: Verify user identity (via support ticket, email verification)
# NEVER reset password without identity verification!

# Step 2: Generate password reset token
node scripts/generate-reset-token.js <user-email>

# Output: Reset token (32-byte random string)
# Example: a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6

# Step 3: Store token hash in database
prisma studio
# Or SQL:
UPDATE users
SET reset_token = '<bcrypt-hash-of-token>',
    reset_token_expires = NOW() + INTERVAL '1 hour'
WHERE email = '<user-email>';

# Step 4: Send reset email
node scripts/send-reset-email.js <user-email> <reset-token>

# Email contains link: https://calcapp.com/reset-password?token=<reset-token>

# Step 5: Verify token used within 1 hour
# (Automated - token expires after 1 hour)
```

**Security Notes:**

- ⚠️ Always verify user identity before reset
- ⚠️ Token is single-use and expires in 1 hour
- ⚠️ Old passwords cannot be reused (check password history)
- ⚠️ Log all password resets for audit trail

### 2.2 Invalidate User Sessions (Account Compromise)

**Scenario:** User reports unauthorized access

**Procedure:**

```bash
# Step 1: Confirm account compromise indicators
# Check: Unusual login location, multiple failed attempts, user report

# Step 2: Disable account immediately
UPDATE users
SET is_active = false,
    disabled_reason = 'Security: Potential compromise',
    disabled_at = NOW()
WHERE email = '<user-email>';

# Step 3: Invalidate all refresh tokens
UPDATE users
SET refresh_token_version = refresh_token_version + 1
WHERE email = '<user-email>';

# Step 4: Add all user's JWTs to blacklist (if before expiry)
# Get user ID from database
SELECT id FROM users WHERE email = '<user-email>';

# Add to blacklist table
INSERT INTO token_blacklist (user_id, reason, expires_at)
VALUES ('<user-id>', 'Account compromise', NOW() + INTERVAL '15 minutes');

# Step 5: Review recent activity
SELECT
  created_at,
  endpoint,
  ip_address,
  user_agent
FROM audit_logs
WHERE user_id = '<user-id>'
  AND created_at > NOW() - INTERVAL '7 days'
ORDER BY created_at DESC;

# Step 6: Notify user via email and SMS (if available)
node scripts/send-security-alert.js <user-email> "account-compromise"

# Step 7: Force password reset
UPDATE users
SET password_reset_required = true,
    reset_token = '<new-token>',
    reset_token_expires = NOW() + INTERVAL '24 hours'
WHERE email = '<user-email>';

# Step 8: Re-enable account after password reset
# (Automated when user completes password reset flow)
```

### 2.3 Rotate JWT Secret

**Scenario:** Quarterly rotation or suspected compromise

**Frequency:** Quarterly (or emergency)  
**Impact:** All users logged out, must re-authenticate

**Procedure:**

```bash
# Step 1: Generate new secret (256-bit random)
NEW_SECRET=$(openssl rand -base64 32)

# Step 2: Store new secret in AWS Secrets Manager
aws secretsmanager update-secret \
  --secret-id calcapp/jwt-secret \
  --secret-string "{\"current\":\"$NEW_SECRET\",\"previous\":\"$OLD_SECRET\"}"

# Step 3: Deploy Lambda functions with both secrets
# Lambda validates with both "current" and "previous" for 24 hours

# Step 4: Update environment variable
aws lambda update-function-configuration \
  --function-name api-auth \
  --environment 'Variables={JWT_SECRET_VERSION=2}'

# Repeat for all Lambda functions

# Step 5: Wait 24 hours for all old tokens to expire

# Step 6: Remove old secret from Secrets Manager
aws secretsmanager update-secret \
  --secret-id calcapp/jwt-secret \
  --secret-string "{\"current\":\"$NEW_SECRET\"}"

# Step 7: Verify no errors in CloudWatch logs
aws logs filter-log-events \
  --log-group-name /aws/lambda/api-auth \
  --filter-pattern '"jwt.verify.failed"' \
  --start-time $(($(date +%s) - 3600))000

# Step 8: Document rotation in audit log
echo "JWT secret rotated on $(date) by $USER" >> /var/log/security-audit.log
```

**Notification:**

```markdown
Subject: Scheduled Maintenance - Re-authentication Required

Dear CalcApp Users,

As part of our regular security maintenance, we have rotated our
authentication keys. You will need to log in again on all devices.

This is a normal security practice and does not indicate a breach.

Thank you for your understanding.
CalcApp Security Team
```

---

## 3. Access Control Operations

### 3.1 Grant User Access (New Premium User)

**Scenario:** User upgrades to premium tier (via Stripe)

**Procedure:**

```bash
# Automated via Stripe webhook (webhook-handler/subscription-updated.ts)

# Manual verification (if webhook fails):

# Step 1: Verify Stripe subscription
stripe subscriptions retrieve <subscription-id>

# Step 2: Update user tier in database
UPDATE users
SET tier = 'premium',
    subscription_id = '<stripe-subscription-id>',
    subscription_started = NOW()
WHERE email = '<user-email>';

# Step 3: Create audit log entry
INSERT INTO audit_logs (user_id, action, details, created_at)
VALUES ('<user-id>', 'subscription.upgraded', '{"tier":"premium"}', NOW());

# Step 4: Send welcome email
node scripts/send-welcome-email.js <user-email>

# Step 5: Verify access (test API call)
curl -X GET https://api.calcapp.com/v1/history \
  -H "Authorization: Bearer <user-jwt-token>"
# Expected: 200 OK (not 403 Forbidden)
```

### 3.2 Revoke User Access (Subscription Cancelled)

**Scenario:** User cancels subscription or payment fails

**Procedure:**

```bash
# Automated via Stripe webhook (subscription.deleted, invoice.payment_failed)

# Step 1: Update user tier to free
UPDATE users
SET tier = 'free',
    subscription_id = NULL,
    subscription_ended = NOW()
WHERE email = '<user-email>';

# Step 2: Archive calculation history
# History retained but read-only access only
UPDATE calculation_history
SET is_accessible = false
WHERE user_id = '<user-id>';

# Step 3: Notify user
node scripts/send-subscription-cancelled.js <user-email>

# Step 4: Grace period (7 days)
# User can re-subscribe and regain access to history
# After 7 days, history is permanently deleted

# Step 5: (After 7 days) Delete history
DELETE FROM calculation_history
WHERE user_id = '<user-id>'
  AND subscription_ended < NOW() - INTERVAL '7 days';
```

### 3.3 Grant Administrator Access (Internal)

**Scenario:** New team member needs AWS/infrastructure access

**Procedure:**

```bash
# Step 1: Create IAM user
aws iam create-user --user-name <firstname.lastname>

# Step 2: Assign to appropriate group
# Groups: developers, security-team, devops, read-only
aws iam add-user-to-group \
  --user-name <firstname.lastname> \
  --group-name developers

# Step 3: Enable MFA (required for console access)
# User must set up MFA on first login
aws iam enable-mfa-device \
  --user-name <firstname.lastname> \
  --serial-number <mfa-device-arn> \
  --authentication-code1 <code1> \
  --authentication-code2 <code2>

# Step 4: Create access keys (for CLI/API access)
aws iam create-access-key --user-name <firstname.lastname>

# Step 5: Send credentials securely
# Use 1Password or encrypted email (PGP)

# Step 6: Document in team access log
echo "$(date): Granted AWS access to <firstname.lastname> (Group: developers)" \
  >> /var/log/access-grants.log

# Step 7: Review access after 90 days
# Set calendar reminder
```

**Security Notes:**

- ⚠️ All users must enable MFA within 24 hours
- ⚠️ Access keys rotate every 90 days
- ⚠️ Review permissions quarterly (least privilege)
- ⚠️ Revoke access immediately on departure

---

## 4. Secrets Management

### 4.1 Add New Secret

**Scenario:** New API key or credential needed

**Procedure:**

```bash
# Step 1: Generate secret (or receive from third party)
# Example: Stripe API key, SendGrid API key, etc.

# Step 2: Store in AWS Secrets Manager
aws secretsmanager create-secret \
  --name calcapp/stripe-api-key \
  --description "Stripe API key for production" \
  --secret-string "sk_live_abc123..."

# Step 3: Grant Lambda access via IAM policy
aws iam attach-role-policy \
  --role-name lambda-execution-role \
  --policy-arn arn:aws:iam::aws:policy/SecretsManagerReadWrite

# Step 4: Update Lambda environment variable (reference to secret)
aws lambda update-function-configuration \
  --function-name api-payment \
  --environment 'Variables={STRIPE_SECRET_NAME=calcapp/stripe-api-key}'

# Step 5: Update application code to fetch secret
# Example (TypeScript):
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getStripeKey() {
  const secret = await secretsManager.getSecretValue({
    SecretId: process.env.STRIPE_SECRET_NAME
  }).promise();
  return secret.SecretString;
}

# Step 6: Test secret retrieval
aws lambda invoke \
  --function-name api-payment \
  --payload '{"action":"test-secret-access"}' \
  response.json

# Step 7: Document secret in secrets inventory
echo "calcapp/stripe-api-key | Stripe Production API Key | api-payment Lambda | Production" \
  >> docs/secrets-inventory.md
```

### 4.2 Rotate Secret

**Scenario:** Quarterly rotation or suspected compromise

**Procedure:**

```bash
# Step 1: Generate new secret value
# (From provider, e.g., regenerate Stripe API key)

# Step 2: Update secret in AWS Secrets Manager (keep old version)
aws secretsmanager update-secret \
  --secret-id calcapp/stripe-api-key \
  --secret-string "sk_live_new_key_xyz789..."

# Step 3: Deploy updated code (Lambda automatically uses new secret)
# No code changes needed if using dynamic secret retrieval

# Step 4: Monitor for errors (24 hours)
aws logs filter-log-events \
  --log-group-name /aws/lambda/api-payment \
  --filter-pattern 'ERROR' \
  --start-time $(($(date +%s) - 86400))000

# Step 5: Delete old secret version (after 24 hours)
aws secretsmanager delete-secret \
  --secret-id calcapp/stripe-api-key \
  --recovery-window-in-days 7

# Step 6: Update secrets inventory
echo "$(date): Rotated calcapp/stripe-api-key" >> docs/secrets-inventory.md
```

### 4.3 Emergency Secret Revocation

**Scenario:** Secret leaked (e.g., committed to public GitHub)

**IMMEDIATE ACTIONS:**

```bash
# Step 1: Revoke secret at source (within 5 minutes)
# Example for Stripe:
curl https://api.stripe.com/v1/api_keys/<key-id> \
  -u sk_live_compromised_key: \
  -X DELETE

# Step 2: Generate new secret
# (Stripe automatically generates new key on deletion)

# Step 3: Update AWS Secrets Manager
aws secretsmanager update-secret \
  --secret-id calcapp/stripe-api-key \
  --secret-string "sk_live_new_emergency_key..."

# Step 4: Force Lambda function update (immediate deployment)
aws lambda update-function-configuration \
  --function-name api-payment \
  --environment 'Variables={FORCE_RELOAD=true}'

# Step 5: Remove secret from Git history (if committed)
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/file-with-secret" \
  --prune-empty --tag-name-filter cat -- --all

# Step 6: Force push (if private repo) or contact GitHub (if public)
git push origin --force --all

# Step 7: Monitor for unauthorized usage
# Check Stripe dashboard for suspicious charges

# Step 8: Incident report
node scripts/create-incident-report.js \
  --type "secret-leak" \
  --secret "stripe-api-key" \
  --action "revoked-and-rotated"
```

**Post-Incident:**

- Review how secret was leaked
- Update pre-commit hooks (detect secrets)
- Team training on secrets management
- Update CI/CD to scan for secrets

---

## 5. Monitoring and Alerting

### 5.1 Configure New Security Alert

**Scenario:** Add monitoring for new security event

**Procedure:**

```bash
# Example: Alert on excessive failed login attempts

# Step 1: Create CloudWatch metric filter
aws logs put-metric-filter \
  --log-group-name /aws/lambda/auth \
  --filter-name FailedLoginAttempts \
  --filter-pattern '"auth.login.failed"' \
  --metric-transformations \
    metricName=FailedLogins,\
    metricNamespace=Security,\
    metricValue=1,\
    defaultValue=0

# Step 2: Create CloudWatch alarm
aws cloudwatch put-metric-alarm \
  --alarm-name HighFailedLogins \
  --alarm-description "Alert on >50 failed logins in 5 minutes" \
  --metric-name FailedLogins \
  --namespace Security \
  --statistic Sum \
  --period 300 \
  --threshold 50 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1 \
  --alarm-actions arn:aws:sns:us-east-1:123456789:security-alerts

# Step 3: Test alarm
# Trigger failed logins (staging environment)
for i in {1..55}; do
  curl -X POST https://staging-api.calcapp.com/v1/auth/login \
    -d '{"email":"test@example.com","password":"wrong"}';
done

# Step 4: Verify alert received (check email, Slack, PagerDuty)

# Step 5: Document alarm
echo "| HighFailedLogins | >50 failed logins/5min | P1 | Potential brute force |" \
  >> docs/cloudwatch-alarms.md
```

### 5.2 Review and Tune Alerts

**Frequency:** Monthly  
**Owner:** Security Lead

**Procedure:**

```bash
# Step 1: List all security alarms
aws cloudwatch describe-alarms \
  --alarm-name-prefix "Security" \
  --query 'MetricAlarms[*].[AlarmName,Threshold,StateValue]' \
  --output table

# Step 2: Check alarm false positive rate
aws cloudwatch get-metric-statistics \
  --namespace Security \
  --metric-name AlarmFalsePositives \
  --start-time $(date -d '30 days ago' +%s) \
  --end-time $(date +%s) \
  --period 86400 \
  --statistics Sum

# Step 3: Adjust thresholds (if >10% false positive rate)
aws cloudwatch put-metric-alarm \
  --alarm-name HighFailedLogins \
  --threshold 75  # Increase from 50 to reduce false positives

# Step 4: Review alarm response time
# Check PagerDuty metrics: time to acknowledge, time to resolve

# Step 5: Document changes
echo "$(date): Adjusted HighFailedLogins threshold from 50 to 75 (reduce FP)" \
  >> docs/alarm-tuning-log.md
```

---

## 6. Incident Response Procedures

### 6.1 Declare Security Incident

**When to Declare:**

- Confirmed data breach
- Active attack (DDoS, unauthorized access)
- Critical vulnerability exploitation
- System compromise

**Procedure:**

```bash
# Step 1: Create incident record
node scripts/create-incident.js \
  --severity P0 \
  --type data-breach \
  --description "SQL injection detected, user data accessed"

# Output: Incident ID (e.g., INC-2025-001)

# Step 2: Notify incident response team
# Automatic notifications via:
- Slack: #security-incidents
- PagerDuty: Page on-call security lead
- Email: security-team@calcapp.com

# Step 3: Start incident log
cat > incidents/INC-2025-001.md << EOF
# Incident: INC-2025-001

**Severity**: P0 (Critical)
**Type**: Data Breach
**Status**: ACTIVE
**Incident Commander**: [Auto-assigned]

## Timeline

| Time | Event | Action |
|------|-------|--------|
| 14:35 | CloudWatch alarm: SQL injection pattern | Security Lead notified |

## Next Actions

- [ ] Contain breach (revoke access)
- [ ] Preserve evidence (DB snapshot)
- [ ] Assess scope (how many users affected?)
- [ ] Patch vulnerability
EOF

# Step 4: Join incident war room
# Zoom: https://zoom.us/j/security-incident

# Step 5: Follow incident response plan
# See: docs/artifacts/04-security-architect/incident-response-plan.md
```

### 6.2 Close Security Incident

**Procedure:**

```bash
# Step 1: Verify incident resolved
- [ ] Threat eliminated
- [ ] Vulnerability patched
- [ ] Systems recovered
- [ ] Monitoring enhanced

# Step 2: Update incident record
node scripts/update-incident.js \
  --incident-id INC-2025-001 \
  --status RESOLVED \
  --resolution "SQL injection vulnerability patched, affected users notified"

# Step 3: Schedule post-incident review
# Within 1 week of resolution

# Step 4: Notify stakeholders
# Internal: #security-incidents, #engineering
# External: Status page update (if public-facing)

# Step 5: Document lessons learned
cat >> incidents/INC-2025-001.md << EOF

## Resolution

**Resolution Time**: 3 hours 45 minutes
**Users Affected**: 237
**Data Exposed**: Email addresses only
**Patch**: SQL injection fix deployed to production

## Lessons Learned

**What Went Well**:
- Rapid detection (5 minutes)
- Clear escalation process

**What Didn't Go Well**:
- Delayed containment (15 minutes)
- Incomplete input validation

**Action Items**:
- [ ] Implement Zod validation on all inputs (Tech Lead, Nov 14)
- [ ] Add SQL injection detection to WAF (Security, Nov 10)
- [ ] Security training for dev team (CTO, Dec 1)

EOF

# Step 6: Archive incident (move to closed/)
mv incidents/INC-2025-001.md incidents/closed/
```

---

## 7. Security Hardening

### 7.1 Harden Lambda Function

**Procedure:**

```bash
# Step 1: Enable VPC (private subnet, no internet access)
aws lambda update-function-configuration \
  --function-name api-auth \
  --vpc-config SubnetIds=<private-subnet-id>,SecurityGroupIds=<sg-id>

# Step 2: Set minimum memory/timeout (reduce attack surface)
aws lambda update-function-configuration \
  --function-name api-auth \
  --memory-size 512 \
  --timeout 30

# Step 3: Enable encryption at rest (KMS)
aws lambda update-function-configuration \
  --function-name api-auth \
  --kms-key-arn arn:aws:kms:us-east-1:123456789:key/abc-123

# Step 4: Remove unused permissions (least privilege IAM)
aws iam detach-role-policy \
  --role-name lambda-api-auth-role \
  --policy-arn arn:aws:iam::aws:policy/PowerUserAccess

# Attach minimal policy
aws iam attach-role-policy \
  --role-name lambda-api-auth-role \
  --policy-arn arn:aws:iam::123456789:policy/lambda-auth-minimal

# Step 5: Enable X-Ray tracing (security monitoring)
aws lambda update-function-configuration \
  --function-name api-auth \
  --tracing-config Mode=Active

# Step 6: Set reserved concurrency (prevent DoS)
aws lambda put-function-concurrency \
  --function-name api-auth \
  --reserved-concurrent-executions 100
```

### 7.2 Harden RDS Database

**Procedure:**

```bash
# Step 1: Enable encryption at rest (if not already)
# (Must be enabled at creation time, cannot modify existing)

# Step 2: Enable automated backups (retention 7 days)
aws rds modify-db-instance \
  --db-instance-identifier calcapp-prod \
  --backup-retention-period 7 \
  --preferred-backup-window "03:00-04:00"

# Step 3: Enable multi-AZ (high availability)
aws rds modify-db-instance \
  --db-instance-identifier calcapp-prod \
  --multi-az

# Step 4: Enable enhanced monitoring
aws rds modify-db-instance \
  --db-instance-identifier calcapp-prod \
  --monitoring-interval 60 \
  --monitoring-role-arn arn:aws:iam::123456789:role/rds-monitoring

# Step 5: Enable audit logging
aws rds modify-db-instance \
  --db-instance-identifier calcapp-prod \
  --cloudwatch-logs-export-configuration \
    '{"EnableLogTypes":["postgresql"]}'

# Step 6: Rotate master password
aws rds modify-db-instance \
  --db-instance-identifier calcapp-prod \
  --master-user-password $(openssl rand -base64 32) \
  --apply-immediately

# Step 7: Update secrets manager with new password
aws secretsmanager update-secret \
  --secret-id calcapp/db-password \
  --secret-string "$(openssl rand -base64 32)"

# Step 8: Restrict security group (only Lambda access)
aws ec2 modify-security-group-rules \
  --group-id <rds-sg-id> \
  --ingress-rules "IpProtocol=tcp,FromPort=5432,ToPort=5432,SourceSecurityGroupId=<lambda-sg-id>"
```

### 7.3 Enable Security Headers (CloudFront)

**Procedure:**

```javascript
// CloudFront Function: security-headers.js

function handler(event) {
  var response = event.response;
  var headers = response.headers;

  // Security headers
  headers['strict-transport-security'] = {
    value: 'max-age=31536000; includeSubDomains; preload'
  };
  headers['content-security-policy'] = {
    value: "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:;"
  };
  headers['x-content-type-options'] = { value: 'nosniff' };
  headers['x-frame-options'] = { value: 'DENY' };
  headers['x-xss-protection'] = { value: '1; mode=block' };
  headers['referrer-policy'] = { value: 'strict-origin-when-cross-origin' };
  headers['permissions-policy'] = {
    value: 'geolocation=(), microphone=(), camera=()'
  };

  return response;
}
```

```bash
# Deploy CloudFront function
aws cloudfront create-function \
  --name security-headers \
  --function-config '{"Comment":"Add security headers","Runtime":"cloudfront-js-1.0"}' \
  --function-code fileb://security-headers.js

# Associate with CloudFront distribution
aws cloudfront update-distribution \
  --id <distribution-id> \
  --distribution-config file://distribution-config.json
```

---

## 8. Compliance Operations

### 8.1 GDPR Data Export (Right to Access)

**Scenario:** User requests all their data

**Procedure:**

```bash
# Automated via API endpoint: /api/v1/data-export

# Manual process (if API unavailable):

# Step 1: Verify user identity (email verification)

# Step 2: Query all user data
psql -h <db-host> -U admin -d calcapp << EOF
\copy (
  SELECT
    u.id,
    u.email,
    u.display_name,
    u.tier,
    u.created_at,
    json_agg(ch.*) as calculation_history,
    json_agg(s.*) as subscription_info
  FROM users u
  LEFT JOIN calculation_history ch ON u.id = ch.user_id
  LEFT JOIN subscriptions s ON u.id = s.user_id
  WHERE u.email = '<user-email>'
  GROUP BY u.id
) TO '/tmp/user-data-export.json' WITH (FORMAT JSON);
EOF

# Step 3: Format data for user (human-readable)
node scripts/format-data-export.js /tmp/user-data-export.json

# Step 4: Encrypt export file (user's public PGP key, if provided)
gpg --encrypt --recipient <user-email> user-data-export.json

# Step 5: Upload to secure S3 bucket (pre-signed URL, 24-hour expiry)
aws s3 cp user-data-export.json.gpg \
  s3://data-exports/<user-id>/export-$(date +%s).json.gpg

aws s3 presign \
  s3://data-exports/<user-id>/export-$(date +%s).json.gpg \
  --expires-in 86400

# Step 6: Email download link to user
node scripts/send-data-export-email.js <user-email> <presigned-url>

# Step 7: Log export request (audit trail)
INSERT INTO audit_logs (user_id, action, details, created_at)
VALUES ('<user-id>', 'data.exported', '{"format":"json"}', NOW());
```

### 8.2 GDPR Data Deletion (Right to be Forgotten)

**Scenario:** User requests account and data deletion

**Procedure:**

```bash
# Step 1: Verify user identity (email verification + authentication)

# Step 2: Soft delete (30-day grace period)
UPDATE users
SET deleted_at = NOW(),
    is_active = false,
    deletion_reason = 'user_requested'
WHERE email = '<user-email>';

# Step 3: Disable API access immediately
# JWT validation checks deleted_at timestamp

# Step 4: Send confirmation email (with recovery instructions)
node scripts/send-deletion-confirmation.js <user-email>

# Step 5: (After 30 days) Hard delete
# Automated cron job runs daily:

# Delete calculation history
DELETE FROM calculation_history
WHERE user_id IN (
  SELECT id FROM users
  WHERE deleted_at < NOW() - INTERVAL '30 days'
);

# Delete refresh tokens
DELETE FROM refresh_tokens
WHERE user_id IN (
  SELECT id FROM users
  WHERE deleted_at < NOW() - INTERVAL '30 days'
);

# Anonymize payment records (keep for compliance, remove PII)
UPDATE payment_history
SET email = 'deleted@example.com',
    user_id = NULL
WHERE user_id IN (
  SELECT id FROM users
  WHERE deleted_at < NOW() - INTERVAL '30 days'
);

# Delete user record
DELETE FROM users
WHERE deleted_at < NOW() - INTERVAL '30 days';

# Step 6: Verify deletion
SELECT COUNT(*) FROM users WHERE email = '<user-email>';
# Expected: 0

# Step 7: Log deletion (anonymized)
INSERT INTO audit_logs (user_id, action, details, created_at)
VALUES (NULL, 'user.deleted', '{"reason":"gdpr_request"}', NOW());
```

---

## 9. Emergency Procedures

### 9.1 Emergency Shutdown (Kill Switch)

**Scenario:** Active attack, need to stop all services immediately

**DANGER:** This will cause complete service outage!

**Procedure:**

```bash
# Step 1: Get approval from Incident Commander
# This is a last resort!

# Step 2: Disable all Lambda functions
for function in $(aws lambda list-functions --query 'Functions[*].FunctionName' --output text); do
  aws lambda update-function-configuration \
    --function-name $function \
    --environment '{"Variables":{"MAINTENANCE_MODE":"true"}}' &
done
wait

# Step 3: Set API Gateway to maintenance mode
aws apigateway update-rest-api \
  --rest-api-id <api-id> \
  --patch-operations op=replace,path=/description,value="MAINTENANCE"

# Step 4: Update CloudFront to serve static maintenance page
aws cloudfront update-distribution \
  --id <distribution-id> \
  --default-root-object maintenance.html

# Step 5: Notify users (status page)
echo "Service temporarily unavailable for emergency maintenance" \
  > /var/www/status/index.html

# Step 6: Incident log
echo "$(date): EMERGENCY SHUTDOWN activated by $USER" \
  >> /var/log/emergency-procedures.log
```

**Recovery:**

```bash
# After threat contained, restore services

# Step 1: Re-enable Lambda functions
for function in $(aws lambda list-functions --query 'Functions[*].FunctionName' --output text); do
  aws lambda update-function-configuration \
    --function-name $function \
    --environment '{"Variables":{"MAINTENANCE_MODE":"false"}}' &
done

# Step 2: Restore API Gateway
# Step 3: Restore CloudFront
# Step 4: Monitor for issues
# Step 5: Notify users (service restored)
```

### 9.2 Rollback Deployment (Security Issue)

**Scenario:** Security vulnerability introduced in recent deployment

**Procedure:**

```bash
# Step 1: Identify problematic deployment
git log --oneline -10

# Step 2: Rollback Lambda functions to previous version
aws lambda update-function-code \
  --function-name api-auth \
  --s3-bucket lambda-deployments \
  --s3-key deployments/api-auth-v1.2.3.zip

# Step 3: Verify rollback successful
curl https://api.calcapp.com/v1/health
# Expected: 200 OK

# Step 4: Monitor error rates (5 minutes)
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Errors \
  --dimensions Name=FunctionName,Value=api-auth \
  --start-time $(date -d '5 minutes ago' --iso-8601=seconds) \
  --end-time $(date --iso-8601=seconds) \
  --period 60 \
  --statistics Sum

# Step 5: Notify team
echo "Security rollback completed: api-auth v1.2.3 -> v1.2.2" | \
  slack-cli post #engineering

# Step 6: Create hotfix branch
git checkout -b hotfix/security-issue-fix

# Step 7: Document incident
echo "$(date): Rolled back api-auth due to security vulnerability" \
  >> /var/log/deployments.log
```

---

## 10. Security Maintenance

### 10.1 Quarterly Security Review

**Frequency:** Every 3 months  
**Owner:** Security Lead + CTO

**Checklist:**

- [ ] Review IAM users and permissions (remove unused)
- [ ] Rotate all secrets (JWT, database passwords, API keys)
- [ ] Update dependencies (npm audit fix)
- [ ] Review CloudWatch alarms (tune thresholds)
- [ ] Penetration test (external firm)
- [ ] Review audit logs (sample for anomalies)
- [ ] Update security documentation
- [ ] Security training for new team members
- [ ] Review incident response plan
- [ ] Test disaster recovery procedures

### 10.2 Monthly Dependency Updates

**Frequency:** Monthly (first Monday)  
**Owner:** Development Team

**Procedure:**

```bash
# Step 1: Check for updates
npm outdated

# Step 2: Review security advisories
npm audit

# Step 3: Update non-breaking changes
npm update

# Step 4: Review major version updates (manual)
npm install package-name@latest

# Step 5: Run tests
npm run test
npm run test:e2e
npm run test:security

# Step 6: Deploy to staging
npm run deploy:staging

# Step 7: Monitor staging for 24 hours
# Check error rates, performance

# Step 8: Deploy to production
npm run deploy:production

# Step 9: Document updates
echo "$(date): Updated dependencies (npm update)" >> CHANGELOG.md
```

---

## Summary

This Security Runbook provides operational procedures for:

1. **Daily Operations**: Security checks, log reviews, monitoring
2. **Authentication**: Password resets, session management, JWT rotation
3. **Access Control**: Grant/revoke access, administrator management
4. **Secrets**: Add, rotate, emergency revocation
5. **Monitoring**: Configure alerts, tune thresholds
6. **Incidents**: Declare, respond, close incidents
7. **Hardening**: Lambda, RDS, security headers
8. **Compliance**: GDPR data export, deletion
9. **Emergency**: Shutdown, rollback
10. **Maintenance**: Quarterly reviews, dependency updates

**Best Practices:**

- ✅ Always verify identity before granting access
- ✅ Document all security operations in audit logs
- ✅ Test procedures in staging before production
- ✅ Follow principle of least privilege
- ✅ Preserve evidence before making changes

---

**Document Approval:**

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Security Architect | [TBD] | 2025-11-07 | [Pending] |
| Technical Lead | [TBD] | [Pending] | [Pending] |
| DevOps Engineer | [TBD] | [Pending] | [Pending] |

---

**Change Log:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-07 | Security Architect | Initial draft |
