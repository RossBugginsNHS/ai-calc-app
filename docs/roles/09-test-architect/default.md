# Role 9: Test Architect

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: Testing Strategy & Quality Assurance  
**Execution Order**: 9th (can run parallel with roles 5-8)  
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

1. **Test Driven Development (TDD)** - Tests written before code; define comprehensive test strategy.
2. **Shift Left** - Test early and often; fail fast.
3. **Automated Security Scanning** - Include SAST/DAST in test strategy and CI/CD pipeline.

---

## Role Description

The Test Architect designs a comprehensive testing strategy that ensures software quality at all levels. This role defines what needs to be tested, how it should be tested, and what quality standards must be met. The Test Architect creates test plans that cover functional correctness, performance, security, and usability.

### Key Responsibilities

1. **Test Strategy Definition**: Overall approach to testing across all levels
2. **Test Planning**: Detailed test plans for each testing phase
3. **Test Case Design**: Create test scenarios and test cases
4. **Test Data Management**: Plan test data creation and management
5. **Test Automation Strategy**: Define what and how to automate
6. **Performance Testing**: Plan load, stress, and scalability tests
7. **Security Testing**: Define security test scenarios
8. **Quality Metrics**: Define code quality and test coverage standards

### Core Activities

- Design test strategy and approach
- Create test plans for all testing levels
- Define test scenarios from requirements
- Design test data strategies
- Plan test automation framework
- Define performance test scenarios
- Establish quality gates and metrics
- Plan security and penetration testing
- Define acceptance criteria validation
- Create test traceability matrix

---

## Input Artifacts

### Required Inputs

1. **`docs/requirements/functional-requirements.md`**
   - Features to test
   - Acceptance criteria
   - Business logic rules

2. **`docs/requirements/user-stories.md`**
   - User scenarios to validate
   - User acceptance criteria
   - User workflows

3. **`docs/architecture/system-architecture.md`**
   - System components to test
   - Integration points
   - Component interactions

4. **`docs/requirements/non-functional-requirements.md`**
   - Performance requirements to validate
   - Security requirements to test
   - Usability standards

---

## Output Artifacts

The Test Architect produces four comprehensive testing documents:

### 1. `docs/planning/test-strategy.md`

**Purpose**: Overall testing approach and philosophy

**Contents**:

```markdown
## Test Strategy Overview

**Testing Philosophy**: Shift-left testing, test automation first, continuous testing

**Quality Goals**:
- Code coverage: Minimum 80%, target 90%
- Automated test coverage: 90% of functional requirements
- Zero critical bugs in production
- Performance within SLA requirements

## Testing Pyramid

```
         /\
        /  \  E2E Tests (10%)
       /____\
      /      \ Integration Tests (30%)
     /________\
    /          \ Unit Tests (60%)
   /____________\
```

**Test Distribution**:
- **Unit Tests (60%)**: Fast, isolated, developer-written
- **Integration Tests (30%)**: Component interaction testing
- **E2E Tests (10%)**: Full user journey testing

## Testing Levels

### Level 1: Unit Testing

**Purpose**: Test individual functions/methods in isolation

**Scope**:
- Business logic functions
- Utility functions
- Data transformations
- Validation rules

**Tools**: Jest / pytest / JUnit / xUnit

**Example Test**:
```javascript
describe('calculateTotal', () => {
  it('should calculate order total correctly', () => {
    const items = [
      { price: 10.00, quantity: 2 },
      { price: 15.00, quantity: 1 }
    ];
    const total = calculateTotal(items);
    expect(total).toBe(35.00);
  });
  
  it('should handle empty cart', () => {
    const total = calculateTotal([]);
    expect(total).toBe(0);
  });
});
```

**Coverage Target**: 90%+

### Level 2: Integration Testing

**Purpose**: Test interaction between components

**Scope**:
- API endpoint testing
- Database operations
- External service integration
- Message queue interactions

**Tools**: Supertest / pytest / Postman / RestAssured

**Example Test**:
```javascript
describe('POST /api/v1/users', () => {
  it('should create a new user', async () => {
    const response = await request(app)
      .post('/api/v1/users')
      .send({
        email: 'test@example.com',
        username: 'testuser',
        password: 'SecurePass123'
      });
    
    expect(response.status).toBe(201);
    expect(response.body.data.email).toBe('test@example.com');
    
    // Verify database persistence
    const user = await db.users.findByEmail('test@example.com');
    expect(user).toBeDefined();
  });
});
```

**Coverage Target**: 80%+

### Level 3: End-to-End Testing

**Purpose**: Test complete user workflows

**Scope**:
- Critical user journeys
- Complete business processes
- Cross-system workflows

**Tools**: Cypress / Playwright / Selenium / Puppeteer

**Example Test**:
```javascript
describe('User Registration Flow', () => {
  it('should allow user to register and login', () => {
    // Visit registration page
    cy.visit('/register');
    
    // Fill registration form
    cy.get('[data-testid="email"]').type('user@example.com');
    cy.get('[data-testid="password"]').type('SecurePass123');
    cy.get('[data-testid="submit"]').click();
    
    // Verify redirect to dashboard
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome, user@example.com');
  });
});
```

**Coverage Target**: All critical user journeys

## Test Automation Strategy

**Automation Priorities**:
1. **High Priority**: Smoke tests, regression tests, API tests
2. **Medium Priority**: Integration tests, common workflows
3. **Low Priority**: Edge cases, visual testing

**Automation Framework**:
- **Unit Tests**: Run on every commit
- **Integration Tests**: Run on every PR
- **E2E Tests**: Run nightly and pre-release

**CI/CD Integration**:
- Fast tests (< 5 min): Block PR merge
- Slow tests (> 5 min): Run asynchronously, report results

## Manual Testing Strategy

**When Manual Testing is Needed**:
- Exploratory testing
- Usability testing
- Visual design verification
- Complex user workflows (not yet automated)
- Edge cases not worth automating

**Manual Test Process**:
1. Test plan review
2. Test case execution
3. Bug reporting
4. Regression verification

## Test Data Strategy

**Test Data Sources**:
- **Fixtures**: Hardcoded test data for unit tests
- **Factories**: Programmatically generated test data
- **Seeders**: Database seeding for integration tests
- **Sanitized Production Data**: For staging (anonymized)

**Test Data Management**:
```javascript
// Factory example
const userFactory = {
  create: (overrides = {}) => ({
    email: faker.internet.email(),
    username: faker.internet.userName(),
    firstName: faker.person.firstName(),
    lastName: faker.person.lastName(),
    ...overrides
  })
};

// Usage
const testUser = userFactory.create({ 
  email: 'specific@example.com' 
});
```

## Quality Gates

**Definition of Done (DoD)**:
- [ ] All acceptance criteria met
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Code coverage > 80%
- [ ] No critical/high severity bugs
- [ ] Code review approved
- [ ] Documentation updated

**Pull Request Gates**:
- All tests must pass
- Code coverage must not decrease
- No new linting errors
- Security scan passes

## Testing Environments

**Environment Strategy**:

1. **Local Development**
   - Purpose: Developer testing
   - Data: Mock data, fixtures
   - Services: Local or mocked

2. **CI Environment**
   - Purpose: Automated test execution
   - Data: Test fixtures, ephemeral
   - Services: Dockerized dependencies

3. **Staging**
   - Purpose: Pre-production testing
   - Data: Sanitized production clone
   - Services: Production-like

4. **Production**
   - Purpose: Smoke tests only
   - Data: Real production data
   - Services: Live services
```

### 2. `docs/planning/test-plan.md`

**Purpose**: Detailed test cases and scenarios

**Contents**:

```markdown
## Test Plan Overview

**Project**: [Project Name]  
**Version**: 1.0  
**Test Cycle**: Release v1.0  
**Test Period**: 2025-11-15 to 2025-12-01

## Test Scope

### In Scope

- User authentication and authorization
- User profile management
- Product catalog functionality
- Order processing
- Payment processing
- API endpoints
- Database operations
- UI components

### Out of Scope

- Third-party service internals
- Infrastructure components (tested separately)
- Historical data migration (one-time)

## Test Cases

### TC-001: User Registration

**Test ID**: TC-001  
**Feature**: User Authentication  
**Related Requirements**: FR-001, US-001  
**Priority**: High  
**Test Type**: Functional

**Preconditions**:
- User is not logged in
- Email address is not registered

**Test Steps**:
1. Navigate to /register
2. Enter valid email address
3. Enter valid username (alphanumeric, 3-30 chars)
4. Enter valid password (min 8 chars, mixed case, number)
5. Confirm password
6. Click "Register" button

**Expected Results**:
- User account created in database
- User automatically logged in
- Redirected to dashboard
- Welcome email sent
- User record includes all provided information

**Test Data**:
- Email: testuser001@example.com
- Username: testuser001
- Password: Test@123456

**Actual Results**: [To be filled during test execution]  
**Status**: [Pass/Fail/Blocked]  
**Tested By**: [Tester name]  
**Tested On**: [Date]

---

### TC-002: User Registration - Duplicate Email

**Test ID**: TC-002  
**Feature**: User Authentication  
**Related Requirements**: FR-001  
**Priority**: High  
**Test Type**: Negative

**Preconditions**:
- Email address already registered

**Test Steps**:
1. Navigate to /register
2. Enter existing email address
3. Enter username and password
4. Click "Register" button

**Expected Results**:
- Registration fails
- Error message: "User with this email already exists"
- No new user created
- User remains on registration page
- Email field highlighted

**Test Data**:
- Email: existing@example.com (pre-existing)

---

### TC-003: User Login - Valid Credentials

**Test ID**: TC-003  
**Feature**: User Authentication  
**Related Requirements**: FR-002, US-002  
**Priority**: Critical  
**Test Type**: Functional

**Preconditions**:
- User account exists
- User is not logged in

**Test Steps**:
1. Navigate to /login
2. Enter valid email
3. Enter correct password
4. Click "Login" button

**Expected Results**:
- User successfully authenticated
- JWT token generated and stored
- Redirected to dashboard
- Last login timestamp updated
- Session created

---

### TC-010: Product Search

**Test ID**: TC-010  
**Feature**: Product Catalog  
**Related Requirements**: FR-020, US-010  
**Priority**: High  
**Test Type**: Functional

**Test Steps**:
1. Navigate to product catalog
2. Enter search term in search box
3. Press Enter or click Search button

**Expected Results**:
- Products matching search term displayed
- Results include product name, image, price
- Results sorted by relevance
- Pagination displayed if > 20 results
- "No results" message if no matches

**Test Data**:
- Search term: "laptop"
- Expected results: At least 5 products

---

### TC-025: Order Checkout - Complete Flow

**Test ID**: TC-025  
**Feature**: Order Processing  
**Related Requirements**: FR-030, FR-031, US-020  
**Priority**: Critical  
**Test Type**: End-to-End

**Preconditions**:
- User logged in
- Cart contains products
- Valid payment method on file

**Test Steps**:
1. Navigate to shopping cart
2. Review items in cart
3. Click "Proceed to Checkout"
4. Select shipping address
5. Select payment method
6. Review order summary
7. Click "Place Order"

**Expected Results**:
- Order created in database
- Payment processed successfully
- Order confirmation displayed
- Order confirmation email sent
- Inventory decremented
- Order appears in user's order history

## Test Execution Schedule

**Phase 1: Unit Testing** (Week 1)
- Developer execution
- All unit tests must pass
- Code coverage > 80%

**Phase 2: Integration Testing** (Week 2)
- API testing
- Database integration
- External service integration

**Phase 3: System Testing** (Week 3)
- End-to-end workflows
- UI testing
- Cross-browser testing

**Phase 4: User Acceptance Testing** (Week 4)
- Stakeholder validation
- Real user scenarios
- Final sign-off

## Test Metrics

**Track and Report**:
- Total test cases: X
- Test cases executed: Y
- Test cases passed: Z
- Test cases failed: W
- Test coverage: %
- Defect density: defects per requirement
- Pass rate: %

## Entry and Exit Criteria

**Entry Criteria**:
- Code is deployed to test environment
- Test environment is stable
- Test data is prepared
- Test cases are reviewed and approved

**Exit Criteria**:
- All planned test cases executed
- No open critical/high severity bugs
- Code coverage meets threshold (80%)
- Performance benchmarks met
- Stakeholder sign-off received
```

### 3. `docs/planning/test-scenarios.md`

**Purpose**: Test scenarios mapped to requirements

**Contents**:

```markdown
## Test Scenario Mapping

### Scenario 1: New User Registration and First Order

**User Story**: US-001, US-020  
**Requirements**: FR-001, FR-002, FR-030

**Scenario Flow**:
1. User visits homepage
2. Clicks "Sign Up"
3. Completes registration form
4. Receives welcome email
5. Browses product catalog
6. Adds product to cart
7. Proceeds to checkout
8. Completes payment
9. Receives order confirmation

**Test Cases**: TC-001, TC-003, TC-010, TC-025

**Success Criteria**:
- User account created
- User can log in
- Order is placed successfully
- All emails received
- Order appears in history

---

### Scenario 2: Password Reset Flow

**User Story**: US-005  
**Requirements**: FR-006

**Scenario Flow**:
1. User clicks "Forgot Password"
2. Enters email address
3. Receives reset email with token
4. Clicks link in email
5. Enters new password
6. Password updated successfully
7. User can log in with new password

**Test Cases**: TC-050, TC-051, TC-052

**Edge Cases**:
- Invalid email
- Expired reset token
- Weak password validation
- Token already used

---

## Traceability Matrix

| Requirement | User Story | Test Scenario | Test Cases | Status |
|-------------|------------|---------------|------------|--------|
| FR-001 | US-001 | Scenario 1 | TC-001, TC-002, TC-003 | Planned |
| FR-002 | US-002 | Scenario 1 | TC-003, TC-004 | Planned |
| FR-006 | US-005 | Scenario 2 | TC-050-TC-052 | Planned |
| FR-020 | US-010 | Scenario 1 | TC-010-TC-012 | Planned |
| FR-030 | US-020 | Scenario 1 | TC-025-TC-030 | Planned |

## Boundary Value Testing

### Test: User Age Validation

**Requirement**: User must be 18-120 years old

**Boundary Values**:
- Just below minimum: 17 (should fail)
- Minimum: 18 (should pass)
- Just above minimum: 19 (should pass)
- Normal: 25, 40, 60 (should pass)
- Just below maximum: 119 (should pass)
- Maximum: 120 (should pass)
- Just above maximum: 121 (should fail)

### Test: Product Price

**Requirement**: Price 0.01 to 999,999.99

**Boundary Values**:
- Below minimum: 0.00 (should fail)
- Minimum: 0.01 (should pass)
- Normal: 99.99, 499.99 (should pass)
- Maximum: 999,999.99 (should pass)
- Above maximum: 1,000,000.00 (should fail)

## Equivalence Partitioning

### Test: Email Validation

**Valid Partition**:
- user@example.com
- john.doe@company.co.uk
- test+tag@subdomain.example.com

**Invalid Partitions**:
- Missing @: userexample.com
- Missing domain: user@
- Missing local: @example.com
- Special chars: user<>@example.com
- Spaces: user @example.com
```

### 4. `docs/planning/performance-test-plan.md`

**Purpose**: Performance, load, and stress testing strategy

**Contents**:

```markdown
## Performance Testing Overview

**Objectives**:
- Validate performance requirements met
- Identify system bottlenecks
- Determine maximum capacity
- Ensure scalability

**Performance Requirements** (from NFRs):
- Response time: < 500ms (p95)
- Throughput: 1000 requests/second
- Concurrent users: 10,000
- Availability: 99.9%

## Performance Test Types

### 1. Load Testing

**Purpose**: Verify system performs under expected load

**Test Scenario**: Normal Business Day
- **Users**: 5,000 concurrent users
- **Duration**: 1 hour
- **Ramp-up**: 10 minutes
- **User Actions**:
  - Browse products: 60%
  - Search: 20%
  - Add to cart: 10%
  - Checkout: 5%
  - View account: 5%

**Success Criteria**:
- Response time < 500ms (p95)
- Error rate < 0.1%
- No memory leaks
- CPU < 70%

### 2. Stress Testing

**Purpose**: Determine breaking point

**Test Scenario**: Beyond Capacity
- **Users**: Start at 5,000, increase by 1,000 every 5 minutes
- **Duration**: Until failure or 20,000 users
- **Observe**: When does system start degrading?

**Success Criteria**:
- Graceful degradation (not crash)
- System recovers after load reduction
- Error messages instead of crashes
- Identify bottleneck

### 3. Spike Testing

**Purpose**: Test sudden traffic increase

**Test Scenario**: Flash Sale
- **Baseline**: 1,000 users
- **Spike**: Jump to 10,000 users in 1 minute
- **Duration**: 10 minutes at peak
- **Return**: Back to 1,000 users

**Success Criteria**:
- System handles spike
- Auto-scaling triggers
- Response time stays acceptable
- No data loss

### 4. Endurance Testing

**Purpose**: Detect memory leaks and performance degradation

**Test Scenario**: Sustained Load
- **Users**: 5,000 concurrent
- **Duration**: 24 hours
- **User Actions**: Mixed realistic usage

**Success Criteria**:
- No memory leaks
- Response time stable
- No resource exhaustion
- Database connections stable

## Performance Test Scenarios

### Scenario 1: Product Search Performance

**Endpoint**: GET /api/v1/products?search={query}

**Load Profile**:
- 100 requests/second
- Various search terms
- Different result set sizes

**Measurements**:
- Response time distribution
- Database query time
- Cache hit rate

**Expected Results**:
- p50: < 100ms
- p95: < 300ms
- p99: < 500ms

### Scenario 2: Order Checkout Performance

**Endpoint**: POST /api/v1/orders

**Load Profile**:
- 50 requests/second
- Real transaction flow
- Payment processing included

**Measurements**:
- End-to-end transaction time
- Database write latency
- External payment API latency

**Expected Results**:
- p50: < 500ms
- p95: < 1000ms
- p99: < 2000ms

### Scenario 3: User Authentication

**Endpoint**: POST /api/v1/auth/login

**Load Profile**:
- 200 requests/second
- Valid credentials
- Password hashing time

**Measurements**:
- Response time
- JWT generation time
- Database query time

**Expected Results**:
- p50: < 200ms
- p95: < 400ms
- Success rate: 100%

## Performance Testing Tools

**Tool**: Apache JMeter / k6 / Gatling / Locust

**Test Script Example** (k6):
```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '5m', target: 1000 },  // Ramp up
    { duration: '30m', target: 1000 }, // Stay at load
    { duration: '5m', target: 0 },     // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],  // 95% < 500ms
    http_req_failed: ['rate<0.01'],     // Error rate < 1%
  },
};

export default function () {
  // Search products
  const searchRes = http.get(
    'https://api.example.com/v1/products?search=laptop'
  );
  check(searchRes, {
    'search status is 200': (r) => r.status === 200,
    'search time < 500ms': (r) => r.timings.duration < 500,
  });
  
  sleep(1);
  
  // View product details
  const productRes = http.get(
    'https://api.example.com/v1/products/123'
  );
  check(productRes, {
    'product status is 200': (r) => r.status === 200,
  });
  
  sleep(2);
}
```

## Database Performance Testing

**Test Areas**:
- Query performance with large datasets
- Index effectiveness
- Connection pool performance
- Transaction throughput

**Test Data Volume**:
- Users: 1,000,000 records
- Products: 100,000 records
- Orders: 5,000,000 records
- Order Items: 15,000,000 records

**Queries to Test**:
```sql
-- Product search (most frequent)
SELECT * FROM products 
WHERE name ILIKE '%laptop%' 
ORDER BY created_at DESC 
LIMIT 20;

-- Order history (common user query)
SELECT * FROM orders 
WHERE user_id = ? 
ORDER BY created_at DESC 
LIMIT 50;

-- Order details (complex join)
SELECT o.*, oi.*, p.* 
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE o.id = ?;
```

## Monitoring During Performance Tests

**Metrics to Monitor**:

**Application Metrics**:
- Request rate
- Response time (p50, p95, p99)
- Error rate
- Active connections

**System Metrics**:
- CPU usage
- Memory usage
- Disk I/O
- Network I/O

**Database Metrics**:
- Queries per second
- Connection pool usage
- Slow query count
- Cache hit ratio

**Infrastructure Metrics**:
- Pod/container count
- Auto-scaling events
- Load balancer metrics

## Performance Test Environment

**Environment**: Staging (production-like)

**Infrastructure**:
- Same instance types as production
- Same number of replicas
- Same database size
- Isolated from other testing

**Data**:
- Production-like volume
- Realistic data distribution
- Sanitized (no real PII)

## Performance Test Report

**Report Includes**:
1. Test summary and objectives
2. Test scenarios executed
3. Performance metrics (with graphs)
4. Bottlenecks identified
5. Scalability analysis
6. Recommendations
7. Comparison to requirements

**Sample Metrics Table**:

| Metric | Requirement | Actual | Status |
|--------|------------|--------|--------|
| p95 Response Time | < 500ms | 423ms | ✅ Pass |
| Throughput | 1000 req/s | 1250 req/s | ✅ Pass |
| Error Rate | < 0.1% | 0.05% | ✅ Pass |
| Max Users | 10,000 | 12,500 | ✅ Pass |
```

---

## Quality Criteria

Before completing this role, ensure:

- [ ] Test strategy covers all testing levels
- [ ] Test cases cover all functional requirements
- [ ] Performance test scenarios defined
- [ ] Test automation strategy documented
- [ ] Quality gates and metrics established
- [ ] Test data strategy defined
- [ ] Test environment requirements specified
- [ ] Traceability matrix complete
- [ ] Edge cases and negative scenarios included
- [ ] Security testing approach defined
- [ ] Usability testing plan created
- [ ] Test tools and frameworks selected

---

## Transition to Next Roles

The Test Architect's outputs inform:

**To Developers** (during implementation):
- Test cases → Implementation guidance
- Quality gates → Development standards
- Test automation → Test writing

**To QA Team**:
- Test plan → Test execution
- Test cases → Manual testing
- Test data → Data preparation

**To DevOps Engineer**:
- Test automation → CI/CD integration
- Performance tests → Load testing setup

---

## Tips for Success

1. **Automate Early**: Write tests as features are developed
2. **Focus on Value**: Test what matters most first
3. **Think Negative**: Test failure scenarios thoroughly
4. **Realistic Data**: Use production-like test data
5. **Continuous Testing**: Integrate tests into CI/CD
6. **Monitor Quality**: Track metrics continuously
7. **Fast Feedback**: Fast tests enable fast iteration
8. **Maintain Tests**: Keep tests up to date with code
9. **Document Well**: Clear test documentation aids debugging
10. **Performance Matters**: Test performance early and often

---

**Previous Role**: [DevOps Engineer](./08-devops-engineer.md)  
**Next Role**: [Technical Lead](./10-technical-lead.md)
