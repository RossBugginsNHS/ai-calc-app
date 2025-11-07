# Deployment Architecture

**Project**: Basic Calculator Web App  
**Version**: 1.0  
**Date**: 2025-11-07  
**Author**: System Architect  
**Status**: Draft

---

## Executive Summary

This document defines the deployment architecture, infrastructure components, CI/CD pipelines, and operational procedures for the Basic Calculator Web App. The architecture uses AWS services (Lambda, RDS Aurora, CloudFront) with Infrastructure as Code (Terraform) and automated deployment via GitHub Actions.

**Key Design Decisions:**

- **AWS Cloud** (us-east-1 primary region)
- **Serverless compute** (AWS Lambda + API Gateway)
- **Managed database** (RDS Aurora Serverless v2)
- **Global CDN** (CloudFront with S3 origin)
- **Infrastructure as Code** (Terraform)
- **CI/CD** (GitHub Actions)
- **Monitoring** (CloudWatch + Sentry)

---

## 1. Infrastructure Overview

### 1.1 High-Level Architecture

```mermaid
graph TB
    subgraph "Client"
        Browser[Web Browser]
    end
    
    subgraph "AWS Cloud - us-east-1"
        subgraph "Edge Network"
            CF[CloudFront CDN]
            S3[S3 Static Assets]
        end
        
        subgraph "Application Layer"
            APIGW[API Gateway]
            WAF[AWS WAF]
            Lambda1[Auth Lambda]
            Lambda2[History Lambda]
            Lambda3[Payment Lambda]
        end
        
        subgraph "Data Layer"
            Aurora[(RDS Aurora<br/>PostgreSQL)]
            Redis[(ElastiCache<br/>Redis)]
        end
        
        subgraph "Infrastructure"
            SM[Secrets Manager]
            CW[CloudWatch Logs]
            SNS[SNS Alerts]
        end
    end
    
    subgraph "External Services"
        Stripe[Stripe API]
        Sentry[Sentry]
        GitHub[GitHub]
    end
    
    Browser --> CF
    CF --> S3
    Browser --> APIGW
    APIGW --> WAF
    WAF --> Lambda1
    WAF --> Lambda2
    WAF --> Lambda3
    Lambda1 --> Aurora
    Lambda1 --> Redis
    Lambda2 --> Aurora
    Lambda3 --> Stripe
    Lambda3 --> Aurora
    Lambda1 --> CW
    Lambda2 --> CW
    Lambda3 --> CW
    Lambda1 --> SM
    CW --> SNS
    Browser --> Sentry
    Lambda1 --> Sentry
    GitHub --> |CI/CD| APIGW
```

### 1.2 AWS Services Used

| Service | Purpose | Pricing Model |
|---------|---------|---------------|
| **CloudFront** | CDN for static assets | $0.085/GB data transfer, $0.01/10K requests |
| **S3** | Static asset storage | $0.023/GB-month storage, $0.005/1K PUT |
| **API Gateway** | REST API routing, rate limiting | $3.50/million requests |
| **Lambda** | Serverless compute (Node.js 20) | $0.20/1M requests, $0.0000166667/GB-second |
| **RDS Aurora** | PostgreSQL database (Serverless v2) | $0.12/ACU-hour + $0.10/GB storage |
| **ElastiCache** | Redis cache | $0.034/hour (cache.t4g.micro) |
| **Secrets Manager** | Database credentials | $0.40/secret/month, $0.05/10K API calls |
| **CloudWatch** | Logs, metrics, alarms | $0.50/GB ingested, $0.10/GB stored, $0.30/dashboard |
| **SNS** | Alert notifications | $0.50/1M requests |
| **WAF** | DDoS protection, bot detection | $5/month + $1/rule/month |

---

## 2. Environment Strategy

### 2.1 Environment Overview

| Environment | Purpose | URL | Auto-Deploy | Data |
|-------------|---------|-----|-------------|------|
| **Development** | Local dev | localhost:3000 | No | Seed data |
| **Staging** | QA testing | staging.calcapp.com | Yes (main branch) | Test data |
| **Production** | Live users | app.calcapp.com | Yes (release tags) | Real data |

### 2.2 Environment Configuration

**Environment Variables** (stored in AWS Secrets Manager):

```bash
# Shared
NODE_ENV=production
LOG_LEVEL=info
API_BASE_URL=https://api.calcapp.com/v1

# Database
DATABASE_URL=postgresql://user:pass@aurora-cluster.us-east-1.rds.amazonaws.com:5432/calcapp

# Authentication
JWT_SECRET=<random-256-bit-secret>
JWT_ACCESS_EXPIRY=7d
JWT_REFRESH_EXPIRY=30d

# Stripe
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_ID=price_1234567890

# External Services
SENTRY_DSN=https://...@sentry.io/...
```

**Per-Environment Overrides**:

- **Development**: `DATABASE_URL=postgresql://localhost:5432/calcapp_dev`
- **Staging**: `STRIPE_SECRET_KEY=sk_test_...` (test mode)
- **Production**: `LOG_LEVEL=warn` (reduce noise)

---

## 3. Infrastructure as Code (Terraform)

### 3.1 Terraform Structure

```
terraform/
├── main.tf                 # Root module
├── variables.tf            # Input variables
├── outputs.tf              # Output values
├── backend.tf              # Remote state configuration
├── modules/
│   ├── networking/         # VPC, subnets, security groups
│   ├── compute/            # Lambda functions
│   ├── storage/            # RDS Aurora, ElastiCache
│   ├── cdn/                # CloudFront, S3
│   └── monitoring/         # CloudWatch, SNS
├── environments/
│   ├── staging/            # Staging environment
│   │   ├── main.tf
│   │   └── terraform.tfvars
│   └── production/         # Production environment
│       ├── main.tf
│       └── terraform.tfvars
```

### 3.2 Example Terraform Configuration

**Root Module** (`terraform/main.tf`):

```hcl
terraform {
  required_version = ">= 1.6"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket         = "calcapp-terraform-state"
    key            = "production/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}

provider "aws" {
  region = var.aws_region
  
  default_tags {
    tags = {
      Project     = "CalcApp"
      Environment = var.environment
      ManagedBy   = "Terraform"
    }
  }
}

# CloudFront + S3 for static assets
module "cdn" {
  source = "./modules/cdn"
  
  domain_name = var.domain_name
  environment = var.environment
}

# API Gateway + Lambda
module "compute" {
  source = "./modules/compute"
  
  environment      = var.environment
  database_url     = module.storage.database_url
  redis_endpoint   = module.storage.redis_endpoint
  jwt_secret       = var.jwt_secret
  stripe_secret    = var.stripe_secret
}

# RDS Aurora + ElastiCache
module "storage" {
  source = "./modules/storage"
  
  environment      = var.environment
  vpc_id           = module.networking.vpc_id
  subnet_ids       = module.networking.private_subnet_ids
  database_name    = "calcapp"
  master_username  = "calcapp_admin"
  master_password  = var.database_password
}

# CloudWatch + SNS
module "monitoring" {
  source = "./modules/monitoring"
  
  environment     = var.environment
  alert_email     = var.alert_email
  lambda_functions = module.compute.lambda_function_names
}
```

**Lambda Function Module** (`terraform/modules/compute/lambda.tf`):

```hcl
resource "aws_lambda_function" "auth" {
  function_name = "calcapp-${var.environment}-auth"
  role          = aws_iam_role.lambda_exec.arn
  handler       = "dist/auth.handler"
  runtime       = "nodejs20.x"
  timeout       = 30
  memory_size   = 512
  
  filename         = "${path.module}/../../lambda-packages/auth.zip"
  source_code_hash = filebase64sha256("${path.module}/../../lambda-packages/auth.zip")
  
  environment {
    variables = {
      NODE_ENV      = var.environment
      DATABASE_URL  = var.database_url
      REDIS_URL     = var.redis_endpoint
      JWT_SECRET    = var.jwt_secret
      SENTRY_DSN    = var.sentry_dsn
    }
  }
  
  vpc_config {
    subnet_ids         = var.subnet_ids
    security_group_ids = [aws_security_group.lambda.id]
  }
  
  tracing_config {
    mode = "Active"  # Enable AWS X-Ray
  }
}

resource "aws_lambda_permission" "apigw_auth" {
  statement_id  = "AllowAPIGatewayInvoke"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.auth.function_name
  principal     = "apigateway.amazonaws.com"
  source_arn    = "${aws_apigatewayv2_api.main.execution_arn}/*/*"
}
```

**RDS Aurora Module** (`terraform/modules/storage/aurora.tf`):

```hcl
resource "aws_rds_cluster" "main" {
  cluster_identifier      = "calcapp-${var.environment}"
  engine                  = "aurora-postgresql"
  engine_version          = "15.4"
  engine_mode             = "provisioned"
  database_name           = var.database_name
  master_username         = var.master_username
  master_password         = var.master_password
  
  db_subnet_group_name    = aws_db_subnet_group.main.name
  vpc_security_group_ids  = [aws_security_group.aurora.id]
  
  backup_retention_period = 35
  preferred_backup_window = "03:00-04:00"
  
  enabled_cloudwatch_logs_exports = ["postgresql"]
  
  serverlessv2_scaling_configuration {
    max_capacity = 4.0
    min_capacity = 0.5
  }
  
  skip_final_snapshot = var.environment != "production"
  
  tags = {
    Name = "calcapp-${var.environment}-aurora"
  }
}

resource "aws_rds_cluster_instance" "main" {
  identifier           = "calcapp-${var.environment}-instance-1"
  cluster_identifier   = aws_rds_cluster.main.id
  instance_class       = "db.serverless"
  engine               = aws_rds_cluster.main.engine
  engine_version       = aws_rds_cluster.main.engine_version
  publicly_accessible  = false
}
```

---

## 4. CI/CD Pipeline

### 4.1 GitHub Actions Workflow

```yaml
# .github/workflows/deploy.yml

name: Deploy

on:
  push:
    branches:
      - main  # Deploy to staging
    tags:
      - 'v*'  # Deploy to production
  pull_request:
    branches:
      - main  # Run tests only

env:
  NODE_VERSION: '20.x'
  AWS_REGION: 'us-east-1'

jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint
        run: npm run lint
      
      - name: Type check
        run: npm run type-check
      
      - name: Unit tests
        run: npm run test:unit
      
      - name: Integration tests
        run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/calcapp_test
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  build-frontend:
    name: Build Frontend
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      
      - name: Install dependencies
        run: npm ci
        working-directory: ./frontend
      
      - name: Build
        run: npm run build
        working-directory: ./frontend
        env:
          VITE_API_URL: ${{ secrets.API_URL }}
      
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: frontend-dist
          path: frontend/dist/
          retention-days: 7

  build-backend:
    name: Build Backend
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      
      - name: Install dependencies
        run: npm ci
        working-directory: ./backend
      
      - name: Build
        run: npm run build
        working-directory: ./backend
      
      - name: Package Lambda functions
        run: |
          cd backend/dist
          zip -r ../../lambda-packages/auth.zip auth.js
          zip -r ../../lambda-packages/history.zip history.js
          zip -r ../../lambda-packages/payment.zip payment.js
      
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: lambda-packages
          path: lambda-packages/
          retention-days: 7

  deploy-staging:
    name: Deploy to Staging
    needs: [build-frontend, build-backend]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Download frontend artifacts
        uses: actions/download-artifact@v3
        with:
          name: frontend-dist
          path: frontend/dist
      
      - name: Download backend artifacts
        uses: actions/download-artifact@v3
        with:
          name: lambda-packages
          path: lambda-packages
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}
      
      - name: Deploy frontend to S3
        run: |
          aws s3 sync frontend/dist/ s3://calcapp-staging-static/ --delete
          aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID_STAGING }} --paths "/*"
      
      - name: Deploy backend to Lambda
        run: |
          aws lambda update-function-code \
            --function-name calcapp-staging-auth \
            --zip-file fileb://lambda-packages/auth.zip
          
          aws lambda update-function-code \
            --function-name calcapp-staging-history \
            --zip-file fileb://lambda-packages/history.zip
          
          aws lambda update-function-code \
            --function-name calcapp-staging-payment \
            --zip-file fileb://lambda-packages/payment.zip
      
      - name: Run database migrations
        run: |
          npm ci
          npx prisma migrate deploy
        working-directory: ./backend
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL_STAGING }}
      
      - name: Run E2E tests
        run: npm run test:e2e
        env:
          BASE_URL: https://staging.calcapp.com
          TEST_USER_EMAIL: ${{ secrets.TEST_USER_EMAIL }}
          TEST_USER_PASSWORD: ${{ secrets.TEST_USER_PASSWORD }}
      
      - name: Notify Slack
        if: always()
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Staging deployment ${{ job.status }}'
          webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}

  deploy-production:
    name: Deploy to Production
    needs: [build-frontend, build-backend]
    if: startsWith(github.ref, 'refs/tags/v')
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      # Similar steps as staging, but with production configuration
      # ... (same as staging with production secrets)
      
      - name: Run smoke tests
        run: npm run test:smoke
        env:
          BASE_URL: https://app.calcapp.com
      
      - name: Rollback on failure
        if: failure()
        run: |
          # Rollback frontend
          aws s3 sync s3://calcapp-prod-static-backup/ s3://calcapp-prod-static/ --delete
          aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID_PROD }} --paths "/*"
          
          # Rollback backend
          aws lambda update-function-code --function-name calcapp-prod-auth --s3-bucket calcapp-lambda-backup --s3-key auth-previous.zip
```

### 4.2 Deployment Pipeline Flow

```mermaid
graph LR
    A[Git Push] --> B{Branch?}
    B -->|feature/*| C[Run Tests]
    B -->|main| D[Deploy Staging]
    B -->|tag v*| E[Deploy Production]
    
    D --> F[Build Frontend]
    D --> G[Build Backend]
    F --> H[Upload to S3]
    G --> I[Update Lambda]
    H --> J[Invalidate CloudFront]
    I --> K[Run Migrations]
    K --> L[E2E Tests]
    L --> M{Pass?}
    M -->|Yes| N[Staging Live]
    M -->|No| O[Rollback]
    
    E --> P[Build Frontend]
    E --> Q[Build Backend]
    P --> R[Upload to S3]
    Q --> S[Update Lambda]
    R --> T[Invalidate CloudFront]
    S --> U[Run Migrations]
    U --> V[Smoke Tests]
    V --> W{Pass?}
    W -->|Yes| X[Production Live]
    W -->|No| Y[Auto Rollback]
```

---

## 5. Database Migrations

### 5.1 Migration Strategy

**Tool**: Prisma Migrate

**Workflow**:

1. **Development**: Create migration

    ```bash
    npx prisma migrate dev --name add_calculation_metadata
    ```

2. **Staging**: Apply migration (CI/CD)

    ```bash
    npx prisma migrate deploy
    ```

3. **Production**: Apply migration with zero-downtime

    ```bash
    # Option 1: Before code deployment (if backward compatible)
    npx prisma migrate deploy
    
    # Option 2: After code deployment (if new code requires new schema)
    # - Deploy code with backward-compatible queries
    # - Run migration
    # - Deploy code that uses new schema
    ```

### 5.2 Zero-Downtime Migration Example

**Scenario**: Add new column `metadata` to `calculation_history` table

**Phase 1: Add column (nullable)**

```sql
-- Migration: 20251107_add_metadata_column
ALTER TABLE calculation_history ADD COLUMN metadata JSONB;
```

**Phase 2: Backfill data** (optional)

```sql
UPDATE calculation_history
SET metadata = '{}'::jsonb
WHERE metadata IS NULL;
```

**Phase 3: Make column NOT NULL** (separate migration, after backfill)

```sql
-- Migration: 20251108_make_metadata_not_null
ALTER TABLE calculation_history ALTER COLUMN metadata SET NOT NULL;
ALTER TABLE calculation_history ALTER COLUMN metadata SET DEFAULT '{}'::jsonb;
```

---

## 6. Monitoring & Observability

### 6.1 CloudWatch Dashboards

**Dashboard: Application Overview**

- API Gateway request count (per endpoint)
- Lambda invocations (per function)
- Lambda errors (per function)
- Lambda duration (p50, p95, p99)
- RDS CPU utilization
- RDS database connections
- ElastiCache hit rate

**Dashboard: Business Metrics**

- Daily Active Users (DAU)
- New user registrations
- Premium subscriptions (new, canceled)
- Calculations per day
- API response time (p95)

### 6.2 CloudWatch Alarms

**Critical Alarms** (page on-call engineer):

- API Gateway 5xx error rate >5% for 5 minutes
- Lambda error rate >10% for 5 minutes
- RDS CPU >90% for 10 minutes
- RDS connections >95 (of 100 max) for 5 minutes

**High Priority Alarms** (investigate within 1 hour):

- API Gateway latency p99 >2s for 10 minutes
- Lambda cold start rate >20% for 10 minutes
- RDS storage >80% full

**Medium Priority Alarms** (investigate within 4 hours):

- CloudFront error rate >1% for 15 minutes
- Failed payment rate >10% for 1 hour

### 6.3 AWS X-Ray Tracing

Enable tracing for all Lambda functions:

```typescript
import AWSXRay from 'aws-xray-sdk-core';
import AWS from 'aws-sdk';

const xrayAWS = AWSXRay.captureAWS(AWS);

export const handler = async (event: APIGatewayProxyEvent) => {
  const segment = AWSXRay.getSegment();
  const subsegment = segment.addNewSubsegment('Database Query');
  
  try {
    const result = await prisma.user.findUnique({ where: { id: userId } });
    subsegment.close();
    return result;
  } catch (error) {
    subsegment.addError(error);
    subsegment.close();
    throw error;
  }
};
```

---

## 7. Backup & Disaster Recovery

### 7.1 Backup Strategy

**RDS Aurora**:

- Automated backups: Every 5 minutes (continuous backup to S3)
- Retention: 35 days
- Manual snapshots: Before major deployments
- Cross-region replication: US-East → EU-West (async)

**S3 Static Assets**:

- Versioning enabled (keep last 10 versions)
- Lifecycle policy: Move to Glacier after 30 days
- Cross-region replication: US-East → US-West

**Application Code**:

- Git repository (GitHub)
- Lambda function versions (keep last 10 versions)
- Docker images (ECR, keep last 10 tags)

### 7.2 Disaster Recovery Procedures

**Scenario 1: Lambda Function Bug** (rollback)

1. Identify previous working version (Lambda console or CLI)
2. Update function alias to previous version
3. Verify with smoke tests
4. **RTO**: 5 minutes, **RPO**: 0

**Scenario 2: Database Corruption** (point-in-time restore)

1. Stop application (prevent writes)
2. Restore Aurora cluster to point-in-time (before corruption)
3. Update database endpoint in Lambda environment variables
4. Resume application
5. **RTO**: 15 minutes, **RPO**: 5 minutes

**Scenario 3: Complete Region Failure** (failover to secondary region)

1. Promote Aurora read replica in EU-West to primary
2. Update DNS (Route 53 failover routing)
3. Deploy Lambda functions to EU-West (pre-configured)
4. Update CloudFront origin to EU-West S3 bucket
5. **RTO**: 1 hour, **RPO**: 1 minute

---

## 8. Security

### 8.1 Network Security

**VPC Configuration**:

- Private subnets: RDS Aurora, ElastiCache, Lambda functions
- Public subnets: NAT Gateway (for Lambda internet access)
- No direct internet access to database

**Security Groups**:

- RDS: Allow PostgreSQL (5432) from Lambda security group only
- ElastiCache: Allow Redis (6379) from Lambda security group only
- Lambda: Allow outbound HTTPS (443) to internet

### 8.2 IAM Roles & Policies

**Lambda Execution Role**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DeleteNetworkInterface"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:us-east-1:123456789012:secret:calcapp-*"
    }
  ]
}
```

### 8.3 Secrets Management

**AWS Secrets Manager**:

- Database credentials (master password)
- JWT secret
- Stripe API keys
- Sentry DSN

**Rotation Policy**:

- Database password: Rotate every 90 days (automated via Lambda)
- JWT secret: Manual rotation (requires user re-authentication)
- Stripe API keys: Manual rotation (as needed)

---

## 9. Cost Optimization

### 9.1 Cost Monitoring

**AWS Cost Explorer**:

- Daily cost breakdown by service
- Monthly budget alerts ($500 threshold)
- Anomaly detection (>$50 unexpected spend)

**Cost Allocation Tags**:

- `Project: CalcApp`
- `Environment: Production|Staging`
- `CostCenter: Engineering`

### 9.2 Cost Optimization Strategies

**Compute**:

- Use Lambda (pay-per-request) instead of EC2 (always-on)
- Provisioned concurrency only for critical endpoints (avoid excessive cold starts)
- Right-size Lambda memory (512MB is optimal for most functions)

**Storage**:

- RDS Aurora Serverless v2 (auto-scales ACU based on load)
- Archive old calculation history to S3 Glacier (25x cheaper)
- S3 Intelligent-Tiering (auto-move infrequent access to cheaper tier)

**Network**:

- CloudFront caching (reduce API Gateway requests)
- Compress responses (gzip, Brotli) to reduce data transfer
- Use S3 Transfer Acceleration for faster uploads (if needed)

**Database**:

- Connection pooling (reduce RDS connection overhead)
- Read replicas only if needed (analytics queries)
- Delete soft-deleted users after 30 days (reduce storage)

---

## 10. Operational Procedures

### 10.1 Deployment Checklist

**Pre-Deployment**:

- [ ] Code review approved (2+ reviewers)
- [ ] All tests passing (unit, integration, E2E)
- [ ] Database migration tested in staging
- [ ] Feature flags configured (if applicable)
- [ ] Rollback plan documented
- [ ] On-call engineer notified

**Deployment**:

- [ ] Create manual RDS snapshot (before migration)
- [ ] Deploy to staging, verify E2E tests pass
- [ ] Deploy to production (via GitHub tag)
- [ ] Monitor CloudWatch alarms for 30 minutes
- [ ] Verify smoke tests pass

**Post-Deployment**:

- [ ] Check error rate (<1%)
- [ ] Check latency p95 (<2s)
- [ ] Verify key user flows (registration, login, calculation)
- [ ] Update CHANGELOG.md
- [ ] Announce deployment in Slack

### 10.2 Incident Response

**Severity Levels**:

- **SEV-1** (Critical): Service down, data loss
- **SEV-2** (High): Degraded performance, elevated errors
- **SEV-3** (Medium): Minor bugs, no user impact

**Incident Response Process**:

1. **Detect**: CloudWatch alarm, user report, monitoring tool
2. **Assess**: Determine severity, impact, root cause
3. **Notify**: Page on-call engineer (SEV-1), notify team (SEV-2)
4. **Mitigate**: Rollback, scale up, disable feature
5. **Resolve**: Deploy fix, verify resolution
6. **Post-Mortem**: Document incident, identify improvements (within 48 hours)

---

## Appendix A: AWS Account Setup

### A.1 Initial Setup Steps

1. **Create AWS Account** (organization best practice: separate accounts for staging/production)
2. **Enable MFA** on root account
3. **Create IAM users** for team members (principle of least privilege)
4. **Set up billing alerts** ($100, $500, $1000 thresholds)
5. **Enable CloudTrail** (audit log for all API calls)
6. **Enable GuardDuty** (threat detection)
7. **Create S3 bucket** for Terraform state (with versioning + encryption)
8. **Create DynamoDB table** for Terraform state locking

### A.2 Terraform Backend Configuration

```hcl
# terraform/backend.tf
terraform {
  backend "s3" {
    bucket         = "calcapp-terraform-state"
    key            = "production/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
    
    # Prevent accidental state deletion
    skip_region_validation      = false
    skip_credentials_validation = false
    skip_metadata_api_check     = false
  }
}
```

---

## Appendix B: Runbook

### B.1 Deploy New Version

```bash
# 1. Create release tag
git tag -a v1.2.0 -m "Release v1.2.0: Add calculation export feature"
git push origin v1.2.0

# 2. GitHub Actions deploys automatically

# 3. Verify deployment
curl https://app.calcapp.com/health
curl https://api.calcapp.com/v1/health

# 4. Monitor logs
aws logs tail /aws/lambda/calcapp-prod-auth --follow
```

### B.2 Rollback Deployment

```bash
# 1. Rollback frontend (S3 + CloudFront)
aws s3 sync s3://calcapp-prod-static-backup/ s3://calcapp-prod-static/ --delete
aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/*"

# 2. Rollback backend (Lambda)
aws lambda update-function-code \
  --function-name calcapp-prod-auth \
  --s3-bucket calcapp-lambda-backup \
  --s3-key auth-v1.1.0.zip
```

### B.3 Restore Database

```bash
# 1. Identify restore point
aws rds describe-db-cluster-snapshots \
  --db-cluster-identifier calcapp-prod \
  --query 'DBClusterSnapshots[*].[SnapshotCreateTime,DBClusterSnapshotIdentifier]' \
  --output table

# 2. Restore to new cluster
aws rds restore-db-cluster-to-point-in-time \
  --source-db-cluster-identifier calcapp-prod \
  --db-cluster-identifier calcapp-prod-restored \
  --restore-to-time 2025-11-07T10:00:00Z

# 3. Update Lambda environment variables
aws lambda update-function-configuration \
  --function-name calcapp-prod-auth \
  --environment "Variables={DATABASE_URL=postgresql://...restored-cluster...}"
```

---

**Document Status**: Draft → Review → Approved  
**Next Review Date**: 2025-11-14  
**Owner**: System Architect  
**Approvers**: DevOps Engineer, Technical Lead, Security Architect
