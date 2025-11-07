# Architecture Decision Records (ADRs)

**Project**: Basic Calculator Web App  
**Version**: 1.0  
**Date**: 2025-11-07  
**Author**: System Architect  
**Status**: Living Document

---

## Overview

This document contains all significant architectural decisions made for the Basic Calculator Web App. Each decision is documented with context, options considered, decision made, and consequences.

**ADR Format** (based on Michael Nygard's template):

- **Title**: Short, descriptive title
- **Status**: Proposed | Accepted | Deprecated | Superseded
- **Date**: Decision date
- **Context**: Problem statement and constraints
- **Options Considered**: Alternatives evaluated
- **Decision**: What we decided and why
- **Consequences**: Positive and negative outcomes
- **Related**: Links to other ADRs

---

## Table of Contents

1. [ADR-001: Use Vanilla TypeScript (No Framework)](#adr-001-use-vanilla-typescript-no-framework)
2. [ADR-002: Serverless Architecture with AWS Lambda](#adr-002-serverless-architecture-with-aws-lambda)
3. [ADR-003: PostgreSQL for Data Storage](#adr-003-postgresql-for-data-storage)
4. [ADR-004: RESTful API (Not GraphQL)](#adr-004-restful-api-not-graphql)
5. [ADR-005: Monorepo Structure](#adr-005-monorepo-structure)
6. [ADR-006: Stripe for Payment Processing](#adr-006-stripe-for-payment-processing)
7. [ADR-007: JWT Authentication](#adr-007-jwt-authentication)
8. [ADR-008: Prisma ORM for Database Access](#adr-008-prisma-orm-for-database-access)
9. [ADR-009: Infrastructure as Code with Terraform](#adr-009-infrastructure-as-code-with-terraform)
10. [ADR-010: LocalStorage for Free Tier History](#adr-010-localstorage-for-free-tier-history)

---

## ADR-001: Use Vanilla TypeScript (No Framework)

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Technical Lead  

### Context

The calculator UI is relatively simple (buttons, display, history list). We need to meet strict performance requirements (page load <2s, bundle size <50KB gzipped). We're considering whether to use a frontend framework (React, Vue, Svelte) or vanilla TypeScript.

**Constraints**:

- Bundle size must be <50KB gzipped (NFR-1.3)
- Page load time <2s on 4G (NFR-1.1)
- Team has TypeScript expertise
- Simple UI (no complex state management needs)

### Options Considered

#### Option 1: React + TypeScript

**Pros**:

- Large ecosystem (component libraries, tools)
- Team familiarity
- Fast development (reusable components)

**Cons**:

- React + ReactDOM ~45KB gzipped (leaves only 5KB for app code)
- Framework overhead (hydration, virtual DOM)
- Overkill for simple calculator UI

**Estimated Bundle Size**: 65-80KB gzipped (exceeds limit)

#### Option 2: Svelte

**Pros**:

- Compiles to vanilla JS (no runtime)
- Small bundle size (~3KB runtime)
- Fast performance (no virtual DOM)

**Cons**:

- Less team familiarity
- Smaller ecosystem than React
- Still adds build complexity

**Estimated Bundle Size**: 25-35KB gzipped (within limit)

#### Option 3: Vanilla TypeScript

**Pros**:

- Zero framework overhead
- Complete control over bundle size
- Fastest possible performance
- Educational value (understand web fundamentals)
- No framework lock-in or upgrade concerns

**Cons**:

- More boilerplate code (manual DOM manipulation)
- No ecosystem of UI components
- Slower development for complex UIs
- Manual state management

**Estimated Bundle Size**: 20-30KB gzipped (well within limit)

#### Option 4: Lit (Web Components)

**Pros**:

- Standards-based (Web Components API)
- Tiny runtime (~5KB)
- Reusable components

**Cons**:

- Still adds dependency
- Overkill for simple UI

**Estimated Bundle Size**: 30-40KB gzipped

### Decision

**We will use Vanilla TypeScript** (Option 3) for the frontend.

**Rationale**:

1. **Performance**: No framework overhead = smallest possible bundle (20-30KB vs 65-80KB for React)
2. **Simplicity**: Calculator UI is simple enough that framework benefits don't outweigh costs
3. **Learning**: Team gains deeper understanding of web fundamentals
4. **Bundle Budget**: Leaves 20-30KB headroom for future features
5. **No Lock-In**: Easy to migrate to Lit or Preact if complexity grows

### Consequences

**Positive**:

- ✅ Smallest possible bundle size (meets NFR-1.3 with headroom)
- ✅ Fastest page load time (no framework hydration)
- ✅ No framework upgrade concerns (breaking changes, security patches)
- ✅ Complete control over performance optimizations

**Negative**:

- ❌ More verbose code (manual DOM manipulation, event handling)
- ❌ Slower development for complex features
- ❌ Manual state management (custom EventBus pattern needed)
- ❌ No component ecosystem (must build everything)

**Mitigation**:

- If UI complexity grows beyond calculator, migrate to Lit (5KB) or Preact (3KB)
- Use TypeScript strict mode to catch errors early
- Create reusable utility functions (e.g., `createElement`, `addClass`)

### Related

- ADR-005: Monorepo Structure (shared TypeScript types between frontend/backend)

---

## ADR-002: Serverless Architecture with AWS Lambda

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, DevOps Engineer  

### Context

Premium users need backend services for authentication and persistent history. We need a scalable, cost-efficient backend that can grow from 1K to 100K concurrent users. We're considering serverless (Lambda), containers (ECS/Fargate), or traditional servers (EC2).

**Constraints**:

- Must support 1K concurrent users at launch (NFR-2.1)
- Must scale to 100K concurrent users by Year 2 (NFR-2.1)
- Budget: $500/month for 1K premium users
- Team has Node.js/TypeScript expertise

### Options Considered

#### Option 1: AWS Lambda (Serverless)

**Pros**:

- **Zero cost at low usage** (1M free requests/month)
- **Auto-scaling** (no capacity planning, scales to millions of requests)
- **Pay-per-request** (cost-efficient for bursty traffic)
- **Managed infrastructure** (no patching, OS maintenance)
- **Fast deployment** (update function code in seconds)

**Cons**:

- Cold start latency (~200ms for Node.js)
- 15-minute execution time limit (not an issue for our <1s requests)
- Stateless (must store state in database or cache)
- Vendor lock-in (AWS-specific)

**Cost Estimate** (1K premium users, 30M requests/month):

- Lambda: $6/month (30M × $0.20/1M)
- API Gateway: $105/month (30M × $3.50/1M)
- **Total**: $111/month

#### Option 2: AWS ECS Fargate (Containers)

**Pros**:

- **Containerized** (portable, consistent dev/prod)
- **No server management** (managed by AWS)
- **Better for long-running processes**

**Cons**:

- **Always-on cost** (pay for vCPU/memory even at 0 requests)
- **Manual scaling** (configure auto-scaling rules)
- **Slower deployment** (container builds, ECS updates)

**Cost Estimate** (1K premium users):

- 2 Fargate tasks (1 vCPU, 2GB RAM): $30/month each = $60/month
- ALB: $20/month
- **Total**: $80/month (but pays even at 0 traffic)

#### Option 3: AWS EC2 (Virtual Machines)

**Pros**:

- **Full control** (SSH access, custom software)
- **Cheapest** at high traffic (no per-request fees)

**Cons**:

- **Always-on cost** (pay 24/7 even at 0 requests)
- **Manual management** (OS patches, security updates, scaling)
- **Slower deployment** (app restarts, server provisioning)
- **No auto-scaling** (must configure ASG, warm-up time)

**Cost Estimate** (1K premium users):

- 2× t3.small instances: $15/month each = $30/month
- ALB: $20/month
- **Total**: $50/month (but requires manual management)

### Decision

**We will use AWS Lambda** (Option 1) for backend services.

**Rationale**:

1. **Cost Efficiency**: Zero cost at low usage (perfect for launch), scales cost linearly with usage
2. **Auto-Scaling**: Handles 1K → 100K users without configuration changes
3. **Zero Management**: No server patching, OS updates, or capacity planning
4. **Fast Deployment**: Update function code in seconds (critical for rapid iteration)
5. **Pay-per-Request**: Fair pricing (only pay for actual usage, not idle time)

**Trade-off**: Cold starts (~200ms) are acceptable for our use case (most requests <1s, cold starts rare with provisioned concurrency)

### Consequences

**Positive**:

- ✅ Lowest cost at launch ($111/month for 1K premium users)
- ✅ Seamless scaling to 100K users (just request AWS limit increase)
- ✅ Zero infrastructure management (focus on features, not servers)
- ✅ Fast deployment (update function, no server restarts)

**Negative**:

- ❌ Cold start latency (~200ms for first request after idle)
- ❌ AWS vendor lock-in (though Lambda API is becoming standard)
- ❌ Stateless architecture (must store sessions in database/Redis)
- ❌ Debugging harder than local server (must use CloudWatch logs)

**Mitigation**:

- Use **provisioned concurrency** for Auth endpoint (eliminate cold starts)
- Use **AWS X-Ray** for distributed tracing (debug Lambda invocations)
- Abstract Lambda handler logic into separate service layer (easier testing)

### Related

- ADR-009: Infrastructure as Code with Terraform (deploy Lambda functions)

---

## ADR-003: PostgreSQL for Data Storage

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Database Designer  

### Context

Premium users need persistent storage for user accounts, subscriptions, and calculation history. We need a database that supports ACID transactions (for payments), scales to 100K users, and integrates well with our serverless architecture.

**Constraints**:

- ACID transactions required (payments, subscriptions)
- Must store: users, subscriptions, calculation history
- Budget: $85/month for database
- Calculation history may grow large (10K calcs/user × 1K users = 10M rows)

### Options Considered

#### Option 1: PostgreSQL (RDS Aurora Serverless v2)

**Pros**:

- **ACID compliance** (critical for payments, user accounts)
- **Strong data integrity** (foreign keys, constraints, triggers)
- **JSON support** (JSONB for flexible calculation history metadata)
- **Mature ecosystem** (Prisma ORM, pgAdmin, pg_dump)
- **Aurora Serverless v2** (auto-scales ACU based on load)
- **Familiar SQL** (easier hiring, standard syntax)

**Cons**:

- Vertical scaling limits (but sufficient for 100K users)
- More complex than NoSQL (schema migrations)

**Cost Estimate** (1K premium users):

- Aurora Serverless v2: 1 ACU × $0.12/hour × 730 hours = $88/month
- Storage: 100GB × $0.10/GB = $10/month
- **Total**: $98/month

#### Option 2: MongoDB (Atlas)

**Pros**:

- **Schemaless** (flexible schema, no migrations)
- **Horizontal scaling** (sharding built-in)
- **JSON-native** (natural fit for calculation metadata)

**Cons**:

- **No ACID transactions** (before v4.0, still limited)
- **Weaker data integrity** (no foreign keys, manual validation)
- **Requires manual validation** (app must enforce constraints)
- **Unfamiliar to team** (learning curve)

**Cost Estimate** (1K premium users):

- Atlas M10 (shared cluster): $57/month
- Storage: $0.25/GB (included up to 10GB)
- **Total**: $57/month

#### Option 3: DynamoDB (AWS NoSQL)

**Pros**:

- **Serverless** (pay-per-request, auto-scales)
- **Fast** (single-digit ms latency)
- **Fully managed** (no patching, backups automatic)

**Cons**:

- **No ACID transactions** (eventual consistency)
- **No joins** (must denormalize data, complex queries hard)
- **No foreign keys** (manual validation)
- **Difficult schema changes** (must handle multiple versions)

**Cost Estimate** (1K premium users, 30M requests/month):

- DynamoDB: 30M × $1.25/1M = $37.50/month
- Storage: 10GB × $0.25/GB = $2.50/month
- **Total**: $40/month

#### Option 4: MySQL (RDS)

**Pros**:

- **ACID compliance**
- **Cheaper than PostgreSQL** (Aurora MySQL vs Aurora PostgreSQL)
- **Familiar SQL**

**Cons**:

- **Weaker JSON support** (compared to PostgreSQL JSONB)
- **Less advanced features** (no CTEs until 8.0, no window functions until 8.0)

**Cost Estimate**: Similar to PostgreSQL (~$88/month)

### Decision

**We will use PostgreSQL (RDS Aurora Serverless v2)** (Option 1).

**Rationale**:

1. **ACID Transactions**: Critical for payment processing (subscriptions, invoices)
2. **Data Integrity**: Foreign keys and constraints ensure data consistency
3. **JSON Support**: JSONB for flexible calculation history metadata (queryable, indexed)
4. **Mature Ecosystem**: Prisma ORM, pgAdmin, standard SQL tooling
5. **Auto-Scaling**: Aurora Serverless v2 scales ACU based on load (0.5 → 16 ACU)
6. **Team Familiarity**: Standard SQL (easier hiring, training)

### Consequences

**Positive**:

- ✅ ACID guarantees (no payment double-charges, no lost subscriptions)
- ✅ Strong data integrity (foreign keys prevent orphaned records)
- ✅ Flexible schema (JSONB for metadata, still queryable)
- ✅ Auto-scaling (Aurora Serverless v2 adjusts to load)
- ✅ Familiar tooling (pgAdmin, Prisma Studio, SQL editors)

**Negative**:

- ❌ Vertical scaling limit (but sufficient for 100K users)
- ❌ Schema migrations required (must plan backward-compatible changes)
- ❌ Higher cost than DynamoDB ($98/month vs $40/month)

**Mitigation**:

- If we hit scaling limits (>100K users), add read replicas or migrate to Citus (sharded PostgreSQL)
- Use Prisma Migrate for zero-downtime migrations (backward-compatible changes)
- Archive old calculation history to S3 Glacier (reduce storage cost)

### Related

- ADR-008: Prisma ORM for Database Access

---

## ADR-004: RESTful API (Not GraphQL)

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, API Designer  

### Context

Premium features (authentication, history sync, subscriptions) require an API. We need to decide between REST and GraphQL.

**Constraints**:

- Simple API (5 resources: auth, history, subscription, webhooks)
- Team has REST experience, minimal GraphQL experience
- Need to support webhooks (Stripe subscription events)

### Options Considered

#### Option 1: RESTful API

**Pros**:

- **Simple** (HTTP verbs, resource-based URLs)
- **Standard** (HTTP caching, CDN-friendly)
- **Tooling** (Postman, Swagger, OpenAPI)
- **Team familiarity** (everyone knows REST)
- **Webhook-friendly** (simple POST endpoints)

**Cons**:

- **Over-fetching** (fetch full user object even if only need email)
- **Multiple round-trips** (get user, then get subscription separately)
- **Versioning required** (URL path versioning)

#### Option 2: GraphQL

**Pros**:

- **Precise queries** (fetch only fields needed, no over-fetching)
- **Single endpoint** (POST /graphql for all operations)
- **Self-documenting** (GraphQL schema introspection)
- **Strong typing** (schema-first design)

**Cons**:

- **Complexity** (schema, resolvers, N+1 queries, caching)
- **No HTTP caching** (everything is POST)
- **Webhooks awkward** (Stripe sends REST POST, must convert)
- **Team unfamiliarity** (learning curve)
- **Overkill for simple API** (5 resources, simple queries)

### Decision

**We will use RESTful API** (Option 1).

**Rationale**:

1. **Simplicity**: Only 5 resources, simple CRUD operations (no complex queries)
2. **Team Familiarity**: Everyone knows REST, fast development
3. **HTTP Caching**: Can cache GET requests (CDN, browser)
4. **Webhook Support**: Stripe sends REST POST (no conversion needed)
5. **Tooling**: OpenAPI spec, Swagger UI, Postman collections

### Consequences

**Positive**:

- ✅ Fast development (no GraphQL schema, resolvers)
- ✅ Standard HTTP caching (CloudFront, browser cache)
- ✅ Simple debugging (cURL, Postman)
- ✅ Webhook-friendly (Stripe POST /webhooks/stripe)

**Negative**:

- ❌ Over-fetching (fetch full user object even if only need email)
- ❌ Multiple round-trips (GET /auth/me, then GET /subscription)
- ❌ API versioning required (v1, v2 in URL path)

**Mitigation**:

- Use sparse fieldsets if over-fetching becomes an issue (`GET /users?fields=id,email`)
- Batch related data in responses (e.g., include subscription in `GET /auth/me`)
- Version API via URL path (`/v1/`, `/v2/`), deprecation policy (12 months)

### Related

- None

---

## ADR-005: Monorepo Structure

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Technical Lead  

### Context

The project has frontend (TypeScript), backend (TypeScript), and infrastructure (Terraform) code. We need to decide between monorepo (single repository) or multi-repo (separate repositories).

**Constraints**:

- Need to share TypeScript types (API contracts)
- CI/CD should build both frontend and backend
- Team size: 2-5 developers

### Options Considered

#### Option 1: Monorepo (Frontend + Backend + Infrastructure)

**Pros**:

- **Shared types** (API contracts in TypeScript interfaces)
- **Atomic commits** (change frontend + backend in one commit)
- **Single CI/CD pipeline** (build both, deploy together)
- **Simpler local development** (one git clone)

**Cons**:

- Larger repo size (but negligible for this project)
- Requires workspace tooling (pnpm workspaces, Nx, Turborepo)

**Structure**:

```
calcapp/
├── frontend/    (Vite + TypeScript)
├── backend/     (Lambda + TypeScript)
├── shared/      (TypeScript types, utilities)
├── terraform/   (Infrastructure as Code)
└── package.json (pnpm workspaces)
```

#### Option 2: Multi-Repo (Separate Repositories)

**Pros**:

- **Independent versioning** (frontend v1.0, backend v2.0)
- **Separate CI/CD** (deploy frontend without backend)
- **Smaller repos** (faster git clone)

**Cons**:

- **No shared types** (must publish npm package or duplicate code)
- **Cross-repo changes hard** (change API contract = 2 commits, 2 PRs)
- **Multiple git clones** (frontend, backend, infrastructure)
- **CI/CD complexity** (coordinate deployments across repos)

### Decision

**We will use Monorepo** (Option 1).

**Rationale**:

1. **Shared Types**: API contracts in `shared/` folder (type-safe frontend/backend)
2. **Atomic Commits**: Change API endpoint + frontend consumer in one commit
3. **Single CI/CD**: Build both, deploy together (ensures compatibility)
4. **Simpler Development**: One `git clone`, one `npm install`

### Consequences

**Positive**:

- ✅ Type-safe API contracts (shared TypeScript interfaces)
- ✅ Atomic changes (no "forgot to update frontend" bugs)
- ✅ Single CI/CD pipeline (simpler GitHub Actions workflow)
- ✅ Easier local development (one repo, one command to start)

**Negative**:

- ❌ Requires workspace tooling (pnpm workspaces, Nx, or Turborepo)
- ❌ Larger repo size (but <10MB for this project)

**Mitigation**:

- Use **pnpm workspaces** for simple workspace management
- Use **Turborepo** if build times become slow (caching, parallelization)

### Related

- ADR-001: Vanilla TypeScript (shared types benefit from TypeScript)

---

## ADR-006: Stripe for Payment Processing

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Security Architect  

### Context

Premium subscriptions require payment processing. We need a payment gateway that handles recurring subscriptions, is PCI DSS compliant, and integrates easily with our serverless backend.

**Constraints**:

- Must be PCI DSS Level 1 compliant (we don't want to handle card data)
- Must support recurring subscriptions (monthly billing)
- Budget: 2.9% + $0.30 per transaction (industry standard)

### Options Considered

#### Option 1: Stripe

**Pros**:

- **PCI DSS Level 1** (highest certification)
- **Client-side tokenization** (Stripe.js, we never touch card data)
- **Subscription management** (built-in, no custom logic)
- **Webhooks** (subscription.updated, invoice.paid, etc.)
- **Developer-friendly** (excellent docs, TypeScript SDK)

**Cons**:

- 2.9% + $0.30 per transaction (standard pricing)
- US-focused (higher fees for international cards)

**Cost Estimate** (1K subscriptions × $5/month):

- Revenue: $5000/month
- Stripe fees: $5000 × 2.9% + $0.30 × 1000 = $145 + $300 = $445/month
- **Net revenue**: $4555/month (91% of gross)

#### Option 2: PayPal

**Pros**:

- **Brand recognition** (users trust PayPal)
- **No card data handling** (PCI DSS compliant)

**Cons**:

- **Higher fees** (3.49% + $0.49 per transaction)
- **Poorer developer experience** (complex API, less documentation)
- **Subscription management less mature**

**Cost Estimate** (1K subscriptions × $5/month):

- Revenue: $5000/month
- PayPal fees: $5000 × 3.49% + $0.49 × 1000 = $174.50 + $490 = $664.50/month
- **Net revenue**: $4335.50/month (87% of gross)

#### Option 3: Braintree (PayPal-owned)

**Pros**:

- **Similar to Stripe** (client-side tokenization)
- **Better international support** (PayPal network)

**Cons**:

- **Fees similar to PayPal** (~3.49% + $0.49)
- **Less developer-friendly** than Stripe

### Decision

**We will use Stripe** (Option 1).

**Rationale**:

1. **PCI DSS Compliance**: Level 1 certification (we never handle card data)
2. **Developer Experience**: Excellent docs, TypeScript SDK, testing tools
3. **Subscription Management**: Built-in recurring billing, proration, cancellation
4. **Webhooks**: Subscription lifecycle events (easy integration)
5. **Lower Fees**: 2.9% + $0.30 vs PayPal 3.49% + $0.49

### Consequences

**Positive**:

- ✅ PCI DSS compliance (no security audits for us)
- ✅ Fast integration (Stripe.js + backend SDK)
- ✅ Subscription management (no custom billing logic)
- ✅ Webhooks (automatic status updates)
- ✅ Lower fees ($445/month vs $665/month for PayPal)

**Negative**:

- ❌ US-focused (higher fees for international cards, ~1-2% more)
- ❌ Vendor lock-in (migrating to another gateway is hard)

**Mitigation**:

- Abstract payment logic into service layer (easier to swap gateways later)
- Stripe is industry standard (unlikely to disappear)

### Related

- None

---

## ADR-007: JWT Authentication

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Security Architect  

### Context

Premium users need authentication for API access. We need a stateless authentication mechanism that works with serverless architecture (no session storage on server).

**Constraints**:

- Stateless (Lambda functions are ephemeral, no in-memory sessions)
- Secure (protection against XSS, CSRF, token theft)
- User-friendly (stay logged in for 7 days, no frequent re-logins)

### Options Considered

#### Option 1: JWT (JSON Web Tokens)

**Pros**:

- **Stateless** (no server-side session storage)
- **Self-contained** (token includes user ID, expiry)
- **Scalable** (no database lookup on every request)
- **Standard** (RFC 7519, widely supported)

**Cons**:

- **Cannot revoke** (once issued, valid until expiry)
- **Size** (JWT tokens are larger than session IDs)
- **XSS risk** (if stored in localStorage)

**Security Approach**:

- Store in **httpOnly cookies** (XSS protection)
- **Short-lived access token** (7 days) + long-lived refresh token (30 days)
- **SameSite=Strict** (CSRF protection)

#### Option 2: Session Cookies (Server-Side Sessions)

**Pros**:

- **Revocable** (delete session from database)
- **Smaller** (session ID is short)
- **Traditional** (well-understood)

**Cons**:

- **Stateful** (requires database lookup on every request)
- **Scalability** (Redis or database required for session storage)
- **Complexity** (Lambda must connect to Redis for every request)

#### Option 3: OAuth 2.0 (Delegated Access)

**Pros**:

- **Standard** (RFC 6749, widely adopted)
- **Third-party login** (Google, GitHub, etc.)

**Cons**:

- **Overkill** (we're not providing API access to third parties)
- **Complexity** (authorization server, scopes, grants)
- **Still need JWT or sessions** for our own tokens

### Decision

**We will use JWT** (Option 1) with **httpOnly cookies**.

**Rationale**:

1. **Stateless**: No database lookup on every request (faster, cheaper)
2. **Scalable**: Lambda functions don't need Redis for sessions
3. **Standard**: Well-documented, many libraries (jsonwebtoken, jose)
4. **Secure**: httpOnly cookies (XSS protection) + SameSite=Strict (CSRF protection)

**Security Measures**:

- Access token: 7 days expiry (stored in httpOnly cookie)
- Refresh token: 30 days expiry (stored in httpOnly cookie)
- Token rotation on refresh (prevent token theft)
- bcrypt password hashing (cost 12)

### Consequences

**Positive**:

- ✅ Stateless (no Redis, no session database)
- ✅ Fast (no database lookup on every request)
- ✅ Scalable (Lambda functions don't share state)
- ✅ Secure (httpOnly cookies prevent XSS, SameSite prevents CSRF)

**Negative**:

- ❌ Cannot revoke tokens (once issued, valid until expiry)
- ❌ Larger tokens (JWT ~200 bytes vs session ID ~16 bytes)

**Mitigation**:

- Short-lived access tokens (7 days, limits exposure if stolen)
- Refresh token rotation (detect stolen refresh tokens)
- If token revocation becomes critical, add Redis blacklist

### Related

- None

---

## ADR-008: Prisma ORM for Database Access

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, Database Designer  

### Context

Backend services need to query PostgreSQL database. We need an ORM or query builder that provides type safety, prevents SQL injection, and handles migrations.

**Constraints**:

- Type-safe queries (compile-time error checking)
- SQL injection prevention (parameterized queries)
- Database migrations (schema versioning)

### Options Considered

#### Option 1: Prisma

**Pros**:

- **Type-safe** (generated TypeScript types from schema)
- **Auto-complete** (IDE suggestions for queries)
- **Migrations** (Prisma Migrate, automatic SQL generation)
- **Great DX** (Prisma Studio GUI for data browsing)

**Cons**:

- **Abstraction layer** (can't write raw SQL easily, but can use `$queryRaw`)
- **Bundle size** (~2MB Prisma Client, but acceptable for Lambda)

#### Option 2: TypeORM

**Pros**:

- **Decorators** (Active Record pattern, familiar to Java devs)
- **Supports multiple databases** (PostgreSQL, MySQL, MongoDB)

**Cons**:

- **Less type-safe** (decorators make TypeScript inference harder)
- **Migrations complex** (manual SQL or TypeORM CLI)

#### Option 3: Knex.js (Query Builder)

**Pros**:

- **Raw SQL control** (write any SQL query)
- **Lightweight** (smaller bundle size)

**Cons**:

- **No type safety** (must manually type query results)
- **Migrations manual** (write SQL migration files)
- **No schema validation**

#### Option 4: Raw SQL (pg library)

**Pros**:

- **Full control** (write any SQL, no abstraction)
- **Smallest bundle** (just pg library)

**Cons**:

- **No type safety** (manual typing)
- **SQL injection risk** (must manually parameterize queries)
- **No migrations** (must use separate tool like dbmate)

### Decision

**We will use Prisma** (Option 1).

**Rationale**:

1. **Type Safety**: Generated TypeScript types from schema (catch errors at compile time)
2. **Migrations**: Prisma Migrate automates schema changes
3. **Developer Experience**: Auto-complete, Prisma Studio GUI, excellent docs
4. **SQL Injection Prevention**: Parameterized queries by default
5. **Bundle Size**: ~2MB is acceptable for Lambda (not frontend)

### Consequences

**Positive**:

- ✅ Type-safe queries (compile-time error checking)
- ✅ Auto-complete (IDE suggestions for tables, columns)
- ✅ Zero-downtime migrations (Prisma Migrate)
- ✅ Prisma Studio (GUI for browsing data)

**Negative**:

- ❌ Abstraction layer (can't write complex SQL easily, but `$queryRaw` available)
- ❌ Bundle size (~2MB, but acceptable for Lambda)

**Mitigation**:

- Use `prisma.$queryRaw` for complex queries if needed
- Use `prisma.$transaction` for multi-query transactions

### Related

- ADR-003: PostgreSQL for Data Storage

---

## ADR-009: Infrastructure as Code with Terraform

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, DevOps Engineer  

### Context

We need to provision AWS infrastructure (Lambda, RDS, CloudFront, etc.). We need a tool that manages infrastructure as code, supports versioning, and enables disaster recovery.

**Constraints**:

- Reproducible infrastructure (dev, staging, production)
- Version control (git history of infrastructure changes)
- Multi-environment support (staging, production)

### Options Considered

#### Option 1: Terraform (HashiCorp)

**Pros**:

- **Cloud-agnostic** (supports AWS, Azure, GCP)
- **Mature** (10+ years old, stable)
- **Large ecosystem** (thousands of modules)
- **State management** (remote state in S3)

**Cons**:

- **HCL syntax** (not native to TypeScript developers)
- **State management** (must configure S3 backend, DynamoDB lock)

#### Option 2: AWS CDK (Cloud Development Kit)

**Pros**:

- **TypeScript-native** (write infrastructure in TypeScript)
- **Type-safe** (compile-time error checking)
- **AWS-native** (first-class support from AWS)

**Cons**:

- **AWS-only** (vendor lock-in)
- **Less mature** (released 2019, still evolving)
- **Generated CloudFormation** (can be hard to debug)

#### Option 3: AWS CloudFormation (Native IaC)

**Pros**:

- **AWS-native** (no third-party dependencies)
- **Free** (no extra cost)

**Cons**:

- **YAML/JSON** (verbose, hard to read)
- **Slow** (CloudFormation stack updates are slow)
- **Limited** (some resources not supported, must use custom resources)

#### Option 4: Pulumi

**Pros**:

- **TypeScript-native** (like CDK)
- **Cloud-agnostic** (supports AWS, Azure, GCP)

**Cons**:

- **Less mature** than Terraform
- **Smaller ecosystem**

### Decision

**We will use Terraform** (Option 1).

**Rationale**:

1. **Cloud-Agnostic**: Can migrate to Azure/GCP later (though unlikely)
2. **Mature**: 10+ years, stable, widely adopted
3. **Ecosystem**: Thousands of modules, extensive documentation
4. **State Management**: Remote state in S3 (versioned, locked)
5. **Portability**: Team's Terraform knowledge transfers to other projects

### Consequences

**Positive**:

- ✅ Cloud-agnostic (not locked to AWS)
- ✅ Mature ecosystem (many modules, community support)
- ✅ Remote state (S3 + DynamoDB lock)
- ✅ Portable knowledge (Terraform skills apply to any cloud)

**Negative**:

- ❌ HCL syntax (learning curve for TypeScript devs)
- ❌ State management complexity (must configure S3 backend)

**Mitigation**:

- Provide Terraform training for team
- Use Terraform Cloud (managed state) if S3 state becomes complex

### Related

- None

---

## ADR-010: LocalStorage for Free Tier History

**Status**: Accepted  
**Date**: 2025-11-07  
**Deciders**: System Architect, UX Designer  

### Context

Free tier users need to store calculation history (last 100 calculations). We need to decide whether to store this client-side (localStorage, IndexedDB) or server-side (backend API).

**Constraints**:

- Free tier users should not require backend infrastructure (zero backend cost)
- Must work offline (PWA requirement)
- 100 calculations × 100 bytes/calc = 10KB (well within 5MB localStorage limit)

### Options Considered

#### Option 1: localStorage (Client-Side)

**Pros**:

- **Zero backend cost** (no database, no API requests)
- **Instant access** (no network latency)
- **Works offline** (PWA, no internet required)
- **Simple** (5MB limit is sufficient for 100 calcs)

**Cons**:

- **No cross-device sync** (history tied to one browser)
- **Cleared on browser reset** (user must accept data loss)
- **No backup** (if user clears data, history is lost)

#### Option 2: Backend API (Server-Side)

**Pros**:

- **Cross-device sync** (history available on all devices)
- **Backup** (data stored in database)
- **No data loss** (persists across browser resets)

**Cons**:

- **Backend cost** (database, API requests, Lambda invocations)
- **Network latency** (slower than localStorage)
- **Requires internet** (doesn't work offline)

#### Option 3: IndexedDB (Client-Side, More Storage)

**Pros**:

- **Larger storage** (50MB+ available)
- **Structured queries** (can query by date, operator)

**Cons**:

- **More complex** than localStorage (IndexedDB API is verbose)
- **Overkill** (10KB storage need doesn't justify complexity)

### Decision

**We will use localStorage** (Option 1) for free tier.

**Rationale**:

1. **Zero Backend Cost**: Free tier users don't generate server costs (critical for profitability)
2. **Instant Access**: No network latency (calculations load instantly)
3. **Offline Support**: Works without internet (PWA, airplane mode)
4. **Sufficient Storage**: 5MB limit >> 10KB need (50× headroom)
5. **Simple Implementation**: `localStorage.setItem()` / `getItem()` (no complex API)

**Premium tier users** get cloud storage (unlimited history, cross-device sync).

### Consequences

**Positive**:

- ✅ Zero backend cost for free users (maximize profit margin)
- ✅ Instant access (<1ms, no network)
- ✅ Works offline (PWA, no internet required)
- ✅ Simple implementation (10 lines of code)

**Negative**:

- ❌ No cross-device sync (history tied to one browser)
- ❌ Data loss risk (if user clears browser data)
- ❌ No backup (user must manually export)

**Mitigation**:

- Provide "Export History" button (download JSON file)
- Show upgrade prompt ("Get unlimited history + cloud sync")
- Document data loss risk in FAQ

### Related

- None

---

## Summary

| ADR | Decision | Status | Key Rationale |
|-----|----------|--------|---------------|
| 001 | Vanilla TypeScript | Accepted | Smallest bundle size (20-30KB vs 65-80KB for React) |
| 002 | AWS Lambda | Accepted | Zero cost at low usage, auto-scales to 100K users |
| 003 | PostgreSQL | Accepted | ACID transactions for payments, mature ecosystem |
| 004 | RESTful API | Accepted | Simple (5 resources), team familiarity, HTTP caching |
| 005 | Monorepo | Accepted | Shared TypeScript types, atomic commits |
| 006 | Stripe | Accepted | PCI DSS compliance, subscription management, lower fees |
| 007 | JWT | Accepted | Stateless (no Redis), scalable, secure with httpOnly cookies |
| 008 | Prisma ORM | Accepted | Type-safe queries, auto-complete, migrations |
| 009 | Terraform | Accepted | Cloud-agnostic, mature ecosystem, remote state |
| 010 | localStorage | Accepted | Zero backend cost, instant access, offline support |

---

**Document Status**: Living Document (updated as new decisions are made)  
**Next Review Date**: 2025-11-14  
**Owner**: System Architect  
**Approvers**: Technical Lead, Security Architect, CTO
