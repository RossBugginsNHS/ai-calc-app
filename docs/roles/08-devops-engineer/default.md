# Role 8: DevOps Engineer

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: Infrastructure & CI/CD  
**Execution Order**: 8th (can run parallel with roles 5-7, 9)  
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

1. **CI/CD Pipeline** - Automate everything; integrate with GitHub
2. **Infrastructure as Code** - Everything versioned and reproducible
3. **Cloud Provider Agnostic** - Design for portability (ask: AWS/Azure/GCP, but provide Docker Compose/local alternatives)
4. **Local Development Parity** - Developers must be able to test and deploy locally
5. **Dev/Prod Parity** - Environments should be as similar as possible
6. **Publishable Artifacts** - Build once, deploy many times
7. **Container Orchestration** - Ask preference (Kubernetes, Docker Swarm, Docker Compose)
8. **Code Quality Gates** - Linting, formatting, complexity checks in CI/CD
9. **Dependency Management** - Regular updates, vulnerability scanning
10. **Observability** - Logging, metrics, tracing from day one
11. **Configuration Management** - Ask how to handle secrets/config across environments
12. **Code Blocks Must Specify Language** - Always include language identifier in all code blocks

---

## Role Description

The DevOps Engineer designs and documents the CI/CD pipeline, infrastructure as code, deployment strategies, and monitoring solutions. This role bridges development and operations, ensuring smooth deployment, reliability, and observability of the application. The DevOps Engineer focuses on automation, scalability, and operational excellence.

### Key Responsibilities

1. **CI/CD Pipeline Design**: Automate build, test, and deployment processes
2. **Infrastructure as Code**: Define infrastructure in version-controlled code
3. **Deployment Strategy**: Plan zero-downtime deployments
4. **Monitoring & Observability**: Design logging, metrics, and alerting
5. **Environment Management**: Define dev, staging, production environments
6. **Security Integration**: Integrate security scanning in pipeline
7. **Disaster Recovery**: Plan backup and recovery procedures
8. **Performance Optimization**: Optimize infrastructure costs and performance

### Core Activities

- Design CI/CD pipeline stages
- Create infrastructure as code templates
- Define deployment strategies (blue-green, canary, rolling)
- Plan monitoring and alerting systems
- Document environment configurations
- Design secrets management strategy
- Plan disaster recovery procedures
- Optimize cloud resource usage
- Establish SLAs and SLOs
- Document runbooks for operations

---

## Input Artifacts

### Required Inputs

1. **`docs/architecture/deployment-architecture.md`**
   - Infrastructure requirements
   - Deployment model
   - Scaling strategy
   - Environment structure

2. **`docs/architecture/technology-stack.md`**
   - Technologies to deploy
   - Build tools and requirements
   - Runtime environments

3. **`docs/requirements/non-functional-requirements.md`**
   - Performance requirements
   - Availability requirements
   - Reliability requirements
   - Compliance requirements

4. **`docs/architecture/security-architecture.md`**
   - Security controls
   - Access management
   - Secrets management

---

## Output Artifacts

The DevOps Engineer produces four comprehensive operational documents:

### 1. `docs/planning/cicd-pipeline.md`

**Purpose**: Complete CI/CD pipeline design and implementation plan

**Contents**:

```markdown
## CI/CD Overview

**CI/CD Tool**: GitHub Actions / GitLab CI / Jenkins / CircleCI  
**Version Control**: Git (GitHub/GitLab/Bitbucket)  
**Artifact Registry**: Docker Hub / ECR / GCR / Artifact Registry  
**Deployment Tool**: ArgoCD / Flux / kubectl / AWS CodeDeploy

## Pipeline Philosophy

- **Continuous Integration**: Every commit triggers build and tests
- **Continuous Delivery**: Every passed build is deployable
- **Continuous Deployment**: Automatic deployment to staging, manual to production
- **Fast Feedback**: Pipeline completes in < 15 minutes
- **Fail Fast**: Stop pipeline on first failure

## Branch Strategy

**Branching Model**: GitFlow / Trunk-Based Development

**Branches**:
- `main`: Production-ready code
- `develop`: Integration branch for features
- `feature/*`: Feature development branches
- `hotfix/*`: Production hotfix branches
- `release/*`: Release preparation branches

**Deployment Triggers**:
- `main` branch → Deploys to production (after approval)
- `develop` branch → Auto-deploys to staging
- `feature/*` → Deploys to preview environment (optional)
- Pull requests → Runs tests, no deployment

## Pipeline Stages

### Stage 1: Code Quality & Security

**Trigger**: On every push and pull request

**Steps**:

1. **Checkout Code**
   ```yaml
   - name: Checkout
     uses: actions/checkout@v3
   ```

2. **Lint & Format Check**
   ```yaml
   - name: Lint Code
     run: |
       npm run lint
       npm run format:check
   ```

3. **Security Scanning**
   ```yaml
   - name: Security Scan - Dependencies
     uses: snyk/actions/node@master
     with:
       command: test
   
   - name: Security Scan - Code
     uses: github/codeql-action/analyze@v2
   ```

4. **License Compliance**
   ```yaml
   - name: Check Licenses
     run: npm run license-check
   ```

**Exit Criteria**: All checks pass, no critical vulnerabilities

### Stage 2: Build

**Trigger**: After code quality stage passes

**Steps**:

1. **Install Dependencies**
   ```yaml
   - name: Cache Dependencies
     uses: actions/cache@v3
     with:
       path: ~/.npm
       key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
   
   - name: Install Dependencies
     run: npm ci
   ```

2. **Build Application**
   ```yaml
   - name: Build
     run: npm run build
     env:
       NODE_ENV: production
   ```

3. **Build Docker Image**
   ```yaml
   - name: Build Docker Image
     run: |
       docker build -t ${{ env.IMAGE_NAME }}:${{ github.sha }} .
       docker tag ${{ env.IMAGE_NAME }}:${{ github.sha }} ${{ env.IMAGE_NAME }}:latest
   ```

**Exit Criteria**: Build completes successfully, artifacts created

### Stage 3: Test

**Trigger**: After build stage completes

**Steps**:

1. **Unit Tests**
   ```yaml
   - name: Run Unit Tests
     run: npm run test:unit
   ```

2. **Integration Tests**
   ```yaml
   - name: Run Integration Tests
     run: npm run test:integration
     env:
       DATABASE_URL: postgresql://test:test@localhost:5432/testdb
   ```

3. **API Tests**
   ```yaml
   - name: Run API Tests
     run: npm run test:api
   ```

4. **Code Coverage**
   ```yaml
   - name: Upload Coverage
     uses: codecov/codecov-action@v3
     with:
       files: ./coverage/lcov.info
       fail_ci_if_error: true
       threshold: 80
   ```

**Exit Criteria**: All tests pass, coverage > 80%

### Stage 4: Container Security Scan

**Trigger**: After successful build

**Steps**:

1. **Scan Docker Image**
   ```yaml
   - name: Scan Image for Vulnerabilities
     uses: aquasecurity/trivy-action@master
     with:
       image-ref: ${{ env.IMAGE_NAME }}:${{ github.sha }}
       format: 'sarif'
       severity: 'CRITICAL,HIGH'
   ```

**Exit Criteria**: No critical vulnerabilities in container image

### Stage 5: Push Artifacts

**Trigger**: After all tests pass (on main/develop branches only)

**Steps**:

1. **Push Docker Image**
   ```yaml
   - name: Login to Container Registry
     uses: docker/login-action@v2
     with:
       registry: ${{ env.REGISTRY }}
       username: ${{ secrets.REGISTRY_USERNAME }}
       password: ${{ secrets.REGISTRY_PASSWORD }}
   
   - name: Push Image
     run: |
       docker push ${{ env.IMAGE_NAME }}:${{ github.sha }}
       docker push ${{ env.IMAGE_NAME }}:latest
   ```

2. **Generate Release Notes** (for main branch)
   ```yaml
   - name: Generate Changelog
     run: npm run changelog
   ```

**Exit Criteria**: Artifacts successfully pushed to registry

### Stage 6: Deploy

**Deploy to Staging** (automatic on develop branch):
```yaml
- name: Deploy to Staging
  run: |
    kubectl config use-context staging
    kubectl set image deployment/app app=${{ env.IMAGE_NAME }}:${{ github.sha }}
    kubectl rollout status deployment/app
```

**Deploy to Production** (manual approval on main branch):
```yaml
- name: Deploy to Production
  if: github.ref == 'refs/heads/main'
  environment:
    name: production
    url: https://app.example.com
  run: |
    kubectl config use-context production
    kubectl set image deployment/app app=${{ env.IMAGE_NAME }}:${{ github.sha }}
    kubectl rollout status deployment/app
```

**Exit Criteria**: Deployment successful, health checks pass

### Stage 7: Post-Deployment

**Steps**:

1. **Smoke Tests**
   ```yaml
   - name: Run Smoke Tests
     run: |
       curl -f https://app.example.com/health || exit 1
       npm run test:smoke
   ```

2. **Notify Team**
   ```yaml
   - name: Slack Notification
     uses: slackapi/slack-github-action@v1
     with:
       payload: |
         {
           "text": "Deployment to ${{ github.event.environment }} successful",
           "version": "${{ github.sha }}"
         }
   ```

**Exit Criteria**: Application healthy, team notified

## Pipeline Performance

**Target Times**:
- Code Quality & Security: < 2 minutes
- Build: < 3 minutes
- Test: < 5 minutes
- Container Scan: < 2 minutes
- Deploy: < 3 minutes
- **Total**: < 15 minutes

## Failure Handling

**On Failure**:
- Stop pipeline immediately
- Notify team via Slack/Email
- Block merging to protected branches
- Create issue for investigation

**Rollback Procedure**:
```yaml
- name: Rollback Deployment
  if: failure()
  run: |
    kubectl rollout undo deployment/app
    kubectl rollout status deployment/app
```

## Environment Variables & Secrets

**Managed via**:
- GitHub Secrets / GitLab CI Variables
- AWS Secrets Manager / Azure Key Vault
- Kubernetes Secrets

**Required Secrets**:
- `REGISTRY_USERNAME`, `REGISTRY_PASSWORD`: Container registry
- `KUBECONFIG`: Kubernetes configuration
- `DATABASE_URL`: Database connection (per environment)
- `API_KEYS`: Third-party service keys
- `JWT_SECRET`: Application secrets
```

### 2. `docs/planning/infrastructure-as-code.md`

**Purpose**: Infrastructure definitions and provisioning

**Contents**:

```markdown
## Infrastructure as Code Overview

**IaC Tool**: Terraform / CloudFormation / Azure ARM / Pulumi  
**Cloud Provider**: AWS / Azure / GCP  
**Configuration Management**: Ansible / Chef (if needed)  
**State Management**: Terraform Cloud / S3 backend

## Infrastructure Components

### Networking

**VPC Configuration** (Terraform example):
```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name        = "${var.project_name}-vpc"
    Environment = var.environment
  }
}

# Public Subnets (for load balancers)
resource "aws_subnet" "public" {
  count                   = 2
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.${count.index}.0/24"
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
  
  tags = {
    Name = "${var.project_name}-public-${count.index + 1}"
  }
}

# Private Subnets (for application servers)
resource "aws_subnet" "private" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 10}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  tags = {
    Name = "${var.project_name}-private-${count.index + 1}"
  }
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name = "${var.project_name}-igw"
  }
}

# NAT Gateway (for private subnet internet access)
resource "aws_nat_gateway" "main" {
  allocation_id = aws_eip.nat.id
  subnet_id     = aws_subnet.public[0].id
  
  tags = {
    Name = "${var.project_name}-nat"
  }
}
```

### Compute Resources

**ECS/Kubernetes Cluster**:
```hcl
# EKS Cluster
resource "aws_eks_cluster" "main" {
  name     = "${var.project_name}-cluster"
  role_arn = aws_iam_role.cluster.arn
  version  = "1.28"
  
  vpc_config {
    subnet_ids              = aws_subnet.private[*].id
    endpoint_private_access = true
    endpoint_public_access  = true
  }
  
  depends_on = [
    aws_iam_role_policy_attachment.cluster_policy
  ]
}

# Node Group
resource "aws_eks_node_group" "main" {
  cluster_name    = aws_eks_cluster.main.name
  node_group_name = "${var.project_name}-nodes"
  node_role_arn   = aws_iam_role.node.arn
  subnet_ids      = aws_subnet.private[*].id
  
  scaling_config {
    desired_size = 2
    max_size     = 5
    min_size     = 1
  }
  
  instance_types = ["t3.medium"]
  
  depends_on = [
    aws_iam_role_policy_attachment.node_policy
  ]
}
```

### Database

**RDS Database**:
```hcl
# RDS PostgreSQL
resource "aws_db_instance" "main" {
  identifier     = "${var.project_name}-db"
  engine         = "postgres"
  engine_version = "15.3"
  instance_class = "db.t3.medium"
  
  allocated_storage     = 20
  max_allocated_storage = 100
  storage_encrypted     = true
  
  db_name  = var.db_name
  username = var.db_username
  password = var.db_password  # Should use secrets manager
  
  multi_az               = var.environment == "production"
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.database.id]
  
  backup_retention_period = 7
  backup_window          = "03:00-04:00"
  maintenance_window     = "sun:04:00-sun:05:00"
  
  skip_final_snapshot = var.environment != "production"
  
  tags = {
    Name = "${var.project_name}-db"
  }
}

# Redis Cache
resource "aws_elasticache_cluster" "main" {
  cluster_id           = "${var.project_name}-cache"
  engine               = "redis"
  node_type            = "cache.t3.micro"
  num_cache_nodes      = 1
  parameter_group_name = "default.redis7"
  port                 = 6379
  subnet_group_name    = aws_elasticache_subnet_group.main.name
  security_group_ids   = [aws_security_group.cache.id]
}
```

### Load Balancer

```hcl
# Application Load Balancer
resource "aws_lb" "main" {
  name               = "${var.project_name}-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = aws_subnet.public[*].id
  
  enable_deletion_protection = var.environment == "production"
  
  tags = {
    Name = "${var.project_name}-alb"
  }
}

# Target Group
resource "aws_lb_target_group" "app" {
  name     = "${var.project_name}-tg"
  port     = 3000
  protocol = "HTTP"
  vpc_id   = aws_vpc.main.id
  
  health_check {
    path                = "/health"
    healthy_threshold   = 2
    unhealthy_threshold = 10
    timeout             = 5
    interval            = 30
  }
}

# HTTPS Listener
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.main.arn
  port              = "443"
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-TLS-1-2-2017-01"
  certificate_arn   = aws_acm_certificate.main.arn
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}
```

## Environment Management

**Workspaces/Environments**:
- Development: `dev`
- Staging: `staging`
- Production: `prod`

**Variable Files**:
- `terraform.tfvars` (checked into git, non-sensitive)
- `secrets.tfvars` (not in git, sensitive values)

**Per-Environment Configuration**:
```hcl
# dev.tfvars
environment     = "development"
instance_type   = "t3.small"
min_instances   = 1
max_instances   = 2
db_instance_class = "db.t3.micro"
multi_az        = false

# prod.tfvars
environment     = "production"
instance_type   = "t3.large"
min_instances   = 3
max_instances   = 10
db_instance_class = "db.r5.xlarge"
multi_az        = true
```

## State Management

**Backend Configuration**:
```hcl
terraform {
  backend "s3" {
    bucket         = "company-terraform-state"
    key            = "project-name/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

## Deployment Process

1. **Initialize**: `terraform init`
2. **Plan**: `terraform plan -var-file=prod.tfvars`
3. **Review**: Manual review of changes
4. **Apply**: `terraform apply -var-file=prod.tfvars`
5. **Verify**: Verify infrastructure is healthy
```

### 3. `docs/planning/monitoring-strategy.md`

**Purpose**: Observability, logging, and alerting strategy

**Contents**:

```markdown
## Monitoring Overview

**Monitoring Stack**:
- **Metrics**: Prometheus + Grafana / CloudWatch / Datadog
- **Logging**: ELK Stack / CloudWatch Logs / Loki
- **Tracing**: Jaeger / X-Ray / Zipkin
- **Alerting**: Alertmanager / PagerDuty / Opsgenie
- **Uptime**: UptimeRobot / Pingdom

## Metrics Collection

### Application Metrics

**Custom Application Metrics**:
```javascript
// Request metrics
http_requests_total (counter)
http_request_duration_seconds (histogram)
http_request_size_bytes (histogram)
http_response_size_bytes (histogram)

// Business metrics
user_registrations_total (counter)
orders_created_total (counter)
payment_transactions_total (counter)
revenue_dollars_total (counter)

// Application health
app_healthy (gauge: 0 or 1)
database_connections_active (gauge)
cache_hit_ratio (gauge)
queue_length (gauge)
```

**Database Metrics**:
- Connection pool usage
- Query execution time
- Slow queries count
- Database size
- Replication lag

**Cache Metrics**:
- Hit/miss ratio
- Memory usage
- Eviction rate
- Connection count

### Infrastructure Metrics

**Compute**:
- CPU utilization
- Memory usage
- Disk I/O
- Network I/O

**Kubernetes Metrics** (if applicable):
- Pod count by status
- Container restarts
- Node resource usage
- Cluster capacity

## Logging Strategy

### Log Levels

- **ERROR**: Application errors, exceptions
- **WARN**: Warning conditions, degraded performance
- **INFO**: Normal operations, key events
- **DEBUG**: Detailed debugging information (dev only)

### Structured Logging

**Log Format** (JSON):
```json
{
  "timestamp": "2025-11-07T10:30:00.123Z",
  "level": "INFO",
  "service": "api-server",
  "environment": "production",
  "requestId": "req_abc123",
  "userId": 12345,
  "method": "POST",
  "path": "/api/v1/orders",
  "statusCode": 201,
  "duration": 245,
  "message": "Order created successfully",
  "metadata": {
    "orderId": 67890,
    "totalAmount": 99.99
  }
}
```

### Log Categories

**Application Logs**:
- HTTP requests/responses
- Authentication events
- Business operations
- Errors and exceptions

**Security Logs**:
- Authentication attempts (success/failure)
- Authorization decisions
- API key usage
- Suspicious activity

**Audit Logs**:
- Data modifications
- Admin actions
- Configuration changes
- User management

### Log Retention

**Retention Periods**:
- Development: 7 days
- Staging: 30 days
- Production: 90 days
- Audit logs: 1-7 years (compliance dependent)

## Dashboards

### Dashboard 1: Application Health

**Metrics**:
- Request rate (requests/second)
- Error rate (%)
- Response time (p50, p95, p99)
- Active users
- System health status

### Dashboard 2: Infrastructure

**Metrics**:
- CPU usage per service
- Memory usage per service
- Disk usage
- Network throughput
- Pod/container status

### Dashboard 3: Business Metrics

**Metrics**:
- User registrations (hourly/daily)
- Orders created
- Revenue
- Conversion rate
- Active users

### Dashboard 4: Database

**Metrics**:
- Queries per second
- Slow query count
- Connection pool usage
- Replication lag
- Database size growth

## Alerting

### Alert Rules

**Critical Alerts** (Page on-call immediately):

**High Error Rate**:
```yaml
alert: HighErrorRate
expr: |
  rate(http_requests_total{status=~"5.."}[5m]) / 
  rate(http_requests_total[5m]) > 0.05
for: 5m
severity: critical
message: Error rate above 5% for 5 minutes
```

**Service Down**:
```yaml
alert: ServiceDown
expr: up{job="api-server"} == 0
for: 2m
severity: critical
message: API server is down
```

**Database Down**:
```yaml
alert: DatabaseDown
expr: database_up == 0
for: 1m
severity: critical
message: Database is unreachable
```

**Warning Alerts** (Notify team, no page):

**High Response Time**:
```yaml
alert: HighResponseTime
expr: |
  histogram_quantile(0.95, 
    rate(http_request_duration_seconds_bucket[5m])
  ) > 2
for: 10m
severity: warning
message: 95th percentile response time above 2 seconds
```

**High CPU Usage**:
```yaml
alert: HighCPUUsage
expr: cpu_usage_percent > 80
for: 15m
severity: warning
message: CPU usage above 80% for 15 minutes
```

### Alert Routing

**Routing Rules**:
```yaml
routes:
  - match:
      severity: critical
    receiver: pagerduty
    continue: true
  
  - match:
      severity: warning
    receiver: slack
  
  - match:
      severity: info
    receiver: email
```

**Receivers**:
- **PagerDuty**: Critical alerts, 24/7 on-call
- **Slack**: Warning alerts, team channel
- **Email**: Info alerts, dev team

### Alert Fatigue Prevention

- **Proper Thresholds**: Tune to avoid false positives
- **Appropriate Duration**: Don't alert on brief spikes
- **Alert Grouping**: Group related alerts
- **Silence During Deployments**: Temporary silence expected alerts
- **Runbooks**: Every alert has runbook link

## SLIs, SLOs, SLAs

### Service Level Indicators (SLIs)

**Availability**:
- Measurement: Successful requests / Total requests
- Target: 99.9%

**Latency**:
- Measurement: 95th percentile response time
- Target: < 500ms

**Error Rate**:
- Measurement: 5xx errors / Total requests
- Target: < 0.1%

### Service Level Objectives (SLOs)

**Monthly SLOs**:
- Availability: 99.9% (43.2 minutes downtime allowed)
- P95 Latency: < 500ms for 99% of requests
- Error Rate: < 0.1%

### Service Level Agreements (SLAs)

**Customer-Facing SLA**:
- Availability: 99.9% uptime
- Support Response: < 1 hour for critical issues
- Credits: 10% monthly fee per 0.1% below SLA

## Health Checks

**Endpoint**: `GET /health`

**Response**:
```json
{
  "status": "healthy",
  "version": "1.2.3",
  "uptime": 86400,
  "checks": {
    "database": "healthy",
    "cache": "healthy",
    "queue": "healthy"
  }
}
```

**Status Codes**:
- 200: Healthy
- 503: Unhealthy
```

### 4. `docs/planning/deployment-strategy.md`

**Purpose**: Deployment approaches and procedures

**Contents**:

```markdown
## Deployment Strategy Overview

**Primary Strategy**: Rolling Deployment with Canary Testing  
**Rollback Time**: < 5 minutes  
**Zero-Downtime**: Yes

## Deployment Strategies

### 1. Rolling Deployment (Default)

**Description**: Gradually replace old version with new version

**Process**:
1. Deploy new version to subset of servers
2. Wait for health checks
3. Continue rolling to remaining servers
4. Complete when all servers updated

**Kubernetes Configuration**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
spec:
  replicas: 6
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2        # Max 2 pods above desired count
      maxUnavailable: 1  # Max 1 pod unavailable
  template:
    spec:
      containers:
      - name: app
        image: app:v1.2.3
        readinessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 5
```

**Advantages**:
- Zero downtime
- Gradual rollout reduces risk
- Automatic rollback on health check failure

**Disadvantages**:
- Both versions run simultaneously
- Longer deployment time

### 2. Blue-Green Deployment

**Description**: Run two identical environments, switch traffic

**Process**:
1. Deploy new version to "green" environment
2. Test green environment thoroughly
3. Switch load balancer to green
4. Keep blue as instant rollback option

**Infrastructure**:
```
Load Balancer
├── Blue Environment (v1.2.2) - 100% traffic initially
└── Green Environment (v1.2.3) - 0% traffic initially

After switch:
├── Blue Environment (v1.2.2) - 0% traffic (standby)
└── Green Environment (v1.2.3) - 100% traffic
```

**Advantages**:
- Instant rollback (just switch back)
- Test new version in production environment
- Zero downtime

**Disadvantages**:
- Requires double infrastructure
- Database migrations challenging

### 3. Canary Deployment

**Description**: Route small percentage of traffic to new version

**Process**:
1. Deploy new version alongside old version
2. Route 5% of traffic to new version
3. Monitor metrics for 30 minutes
4. If healthy, increase to 25%, then 50%, then 100%
5. If issues, rollback immediately

**Traffic Distribution**:
```
v1.2.2 (old): 95% traffic → 75% → 50% → 0%
v1.2.3 (new): 5% traffic → 25% → 50% → 100%
```

**Monitoring During Canary**:
- Error rate comparison
- Response time comparison
- Resource usage
- Business metrics

**Advantages**:
- Minimizes blast radius
- Real user testing
- Gradual rollout

**Disadvantages**:
- Complex traffic routing
- Requires sophisticated monitoring

## Pre-Deployment Checklist

- [ ] All tests passing in CI/CD
- [ ] Security scans completed
- [ ] Database migrations tested
- [ ] Feature flags configured
- [ ] Rollback plan documented
- [ ] Monitoring dashboards ready
- [ ] On-call engineer available
- [ ] Stakeholders notified
- [ ] Backup taken (if major release)

## Deployment Process

### Step 1: Pre-Deployment

```bash
# 1. Verify current production status
kubectl get deployments
kubectl get pods

# 2. Check health of current version
curl https://api.example.com/health

# 3. Create backup (major releases)
./scripts/backup-database.sh

# 4. Enable maintenance mode (if needed)
# kubectl scale deployment/app --replicas=1
```

### Step 2: Deployment

```bash
# 1. Update deployment with new image
kubectl set image deployment/app \
  app=myregistry/app:v1.2.3

# 2. Monitor rollout
kubectl rollout status deployment/app

# 3. Watch pod status
kubectl get pods -w
```

### Step 3: Post-Deployment Verification

```bash
# 1. Check health endpoint
curl https://api.example.com/health

# 2. Run smoke tests
npm run test:smoke

# 3. Check logs for errors
kubectl logs -l app=app --tail=100

# 4. Monitor metrics
# - Check Grafana dashboards
# - Verify error rates normal
# - Confirm response times acceptable
```

### Step 4: Completion

```bash
# 1. Update deployment records
echo "v1.2.3 deployed at $(date)" >> deployments.log

# 2. Notify team
./scripts/notify-slack.sh "Deployment v1.2.3 successful"

# 3. Monitor for 30 minutes
# Watch dashboards for anomalies
```

## Rollback Procedures

### Automatic Rollback

**Triggers**:
- Health check failures
- High error rate (> 5%)
- Crash loop detection

**Kubernetes Auto-Rollback**:
```yaml
spec:
  progressDeadlineSeconds: 600
  minReadySeconds: 30
  # Automatically rolls back if pods don't become ready
```

### Manual Rollback

**Quick Rollback**:
```bash
# Rollback to previous version
kubectl rollout undo deployment/app

# Rollback to specific revision
kubectl rollout undo deployment/app --to-revision=5

# Check rollback status
kubectl rollout status deployment/app
```

**Database Rollback** (if migrations applied):
```bash
# Run down migration
npm run migrate:down

# Restore from backup (if needed)
./scripts/restore-database.sh backup-20251107.sql
```

## Database Migrations

### Migration Strategy

**Backward-Compatible Migrations**:
- Add columns (don't remove)
- Make new columns nullable initially
- Remove in later release

**Migration Process**:
1. **Deploy 1**: Add new column (nullable)
2. **Deploy 2**: Start writing to new column
3. **Deploy 3**: Backfill existing data
4. **Deploy 4**: Make column non-nullable
5. **Deploy 5**: Remove old column

### Zero-Downtime Migrations

**Safe Operations**:
- Add table
- Add nullable column
- Add index (concurrently)
- Remove unused column

**Unsafe Operations** (require downtime or special handling):
- Rename column
- Change column type
- Add NOT NULL constraint to existing column
- Remove index

**Example: Add Column**:
```sql
-- Safe: Add nullable column
ALTER TABLE users ADD COLUMN phone VARCHAR(20) NULL;

-- Later release: Make non-null after backfill
ALTER TABLE users ALTER COLUMN phone SET NOT NULL;
```

## Feature Flags

**Purpose**: Decouple deployment from feature release

**Flag Configuration**:
```javascript
const featureFlags = {
  newCheckoutFlow: {
    enabled: true,
    rollout: 50,  // Percentage of users
    users: ['beta-tester@example.com']  // Specific users
  },
  darkMode: {
    enabled: true,
    rollout: 100
  }
};
```

**Benefits**:
- Deploy code without enabling feature
- Gradual feature rollout
- A/B testing capability
- Instant feature toggle without redeployment

## Deployment Schedule

**Regular Deployments**:
- Tuesday/Thursday: 10:00 AM - 3:00 PM
- Avoid: Fridays, holidays, end of month

**Emergency Hotfixes**:
- Any time, follow expedited process
- Requires manager approval
- Skip staging (if critical)

## Post-Mortem Process

**After Failed Deployment**:
1. Document what happened
2. Root cause analysis
3. Identify preventive measures
4. Update runbooks
5. Share learnings with team
```

---

## Quality Criteria

Before completing this role, ensure:

- [ ] CI/CD pipeline covers all stages
- [ ] Infrastructure is fully defined in code
- [ ] All environments are documented
- [ ] Monitoring covers key metrics
- [ ] Alerts are configured with proper thresholds
- [ ] Deployment strategy supports zero downtime
- [ ] Rollback procedures are documented and tested
- [ ] Security scanning is integrated
- [ ] Secrets management is secure
- [ ] Backup and recovery procedures are defined
- [ ] SLOs and SLAs are established
- [ ] Disaster recovery plan exists
- [ ] Runbooks are created for common operations

---

## Transition to Next Roles

The DevOps Engineer's outputs inform:

**To Developers** (during implementation):
- CI/CD pipeline → Build and test automation
- Infrastructure → Local development setup
- Monitoring → Instrumentation requirements

**To Operations** (during maintenance):
- Runbooks → Operational procedures
- Monitoring → System health tracking
- Alerts → Incident response

---

## Tips for Success

1. **Automate Everything**: Manual processes lead to errors
2. **Monitor First**: Deploy monitoring before application
3. **Start Simple**: Don't over-engineer initially
4. **Document Runbooks**: Future you will thank you
5. **Test Rollbacks**: Practice rollback procedures
6. **Security Always**: Scan early and often
7. **Cost Awareness**: Monitor and optimize cloud costs
8. **Gradual Rollouts**: Reduce risk with canary deployments
9. **Log Strategically**: Too much is better than too little
10. **Learn from Failures**: Every incident is a learning opportunity

---

**Previous Role**: [API Designer](./07-api-designer.md)  
**Next Role**: [Test Architect](./09-test-architect.md)
