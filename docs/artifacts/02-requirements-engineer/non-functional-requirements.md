# Non-Functional Requirements: Basic Calculator Web App

**Date Created**: 2025-11-07  
**Role**: Requirements Engineer  
**Status**: Initial Draft  
**Version**: 1.0

---

## Table of Contents

1. [Performance Requirements](#performance-requirements)
2. [Scalability Requirements](#scalability-requirements)
3. [Security Requirements](#security-requirements)
4. [Accessibility Requirements](#accessibility-requirements)
5. [Usability Requirements](#usability-requirements)
6. [Reliability Requirements](#reliability-requirements)
7. [Compatibility Requirements](#compatibility-requirements)
8. [Maintainability Requirements](#maintainability-requirements)
9. [Privacy & Compliance Requirements](#privacy--compliance-requirements)

---

## Performance Requirements

### NFR-PERF-001: Page Load Time

**Requirement**: The calculator page SHALL load and become interactive within:
- **2 seconds** on 4G mobile connection (Fast 3G - 1.6 Mbps, 150ms RTT)
- **1 second** on broadband connection
- **3 seconds** maximum on 3G connection

**Measurement**: Time to Interactive (TTI) via Lighthouse

**Priority**: Must Have

**Rationale**: Fast page load is critical for retention and user satisfaction

**Verification Method**: Lighthouse performance testing, WebPageTest

---

### NFR-PERF-002: Calculation Response Time

**Requirement**: Calculation results SHALL be displayed within:
- **50 milliseconds** for basic operations
- **100 milliseconds** for complex chained operations

**Measurement**: Time from button click/key press to result display update

**Priority**: Must Have

**Rationale**: Instant feedback essential for calculator usability

**Verification Method**: Performance profiling, automated timing tests

---

### NFR-PERF-003: History Loading

**Requirement**: Calculation history SHALL:
- Load and display **within 200ms** for up to 100 items
- Support **infinite scroll** with lazy loading (load 20 items at a time)
- Maintain **60 FPS** during scroll

**Measurement**: Chrome DevTools Performance panel, scroll FPS

**Priority**: Should Have

**Rationale**: Smooth history interaction enhances premium value

**Verification Method**: Performance profiling with large datasets

---

### NFR-PERF-004: Bundle Size

**Requirement**: The initial JavaScript bundle SHALL be:
- **≤ 50 KB** (gzipped) for core calculator
- **≤ 100 KB** (gzipped) total including all features
- Use code splitting for premium features

**Measurement**: Webpack bundle analyzer

**Priority**: Must Have

**Rationale**: Small bundle size enables fast loading, especially mobile

**Verification Method**: Build size monitoring, bundle analysis

---

### NFR-PERF-005: Memory Usage

**Requirement**: The application SHALL:
- Use **≤ 50 MB** of browser memory for free tier
- Use **≤ 100 MB** of browser memory for premium tier with full history
- Not cause memory leaks during extended use (8 hours continuous)

**Measurement**: Chrome DevTools Memory profiler

**Priority**: Should Have

**Rationale**: Prevent browser slowdown and crashes

**Verification Method**: Memory profiling, long-running session tests

---

## Scalability Requirements

### NFR-SCALE-001: Concurrent Users

**Requirement**: The system SHALL support:
- **1,000 concurrent users** at launch (Month 1)
- **10,000 concurrent users** by Month 6
- **100,000 concurrent users** by Year 2

**Measurement**: Load testing with concurrent user simulation

**Priority**: Must Have

**Rationale**: Support business growth targets

**Verification Method**: Load testing (JMeter, k6)

---

### NFR-SCALE-002: Database Scalability

**Requirement**: The database SHALL:
- Support **10,000 premium users** with average 500 calculations each (5M records)
- Scale to **100,000 premium users** (50M records) with acceptable performance
- Query response time **< 100ms** for p95 of history queries

**Measurement**: Database query performance monitoring

**Priority**: Must Have

**Rationale**: Support premium user growth without degradation

**Verification Method**: Database load testing, query performance analysis

---

### NFR-SCALE-003: API Rate Limiting

**Requirement**: The API SHALL implement rate limiting:
- **100 requests/minute** per user for authenticated endpoints
- **20 requests/minute** per IP for unauthenticated endpoints
- Graceful degradation with clear error messages

**Measurement**: Rate limit testing

**Priority**: Should Have

**Rationale**: Prevent abuse, ensure fair resource usage

**Verification Method**: API load testing with rate limit validation

---

## Security Requirements

### NFR-SEC-001: Authentication Security

**Requirement**: The system SHALL:
- Hash passwords using **bcrypt** with cost factor ≥ 12
- Enforce password requirements: ≥ 8 characters, 1 uppercase, 1 lowercase, 1 number
- Implement **HTTPS only** for all connections (redirect HTTP → HTTPS)
- Use secure session tokens (httpOnly, secure, SameSite cookies)
- Implement **CSRF protection** for all state-changing operations

**Priority**: Must Have

**Rationale**: Protect user accounts and data

**Verification Method**: Security audit, penetration testing

---

### NFR-SEC-002: Payment Security

**Requirement**: Payment processing SHALL:
- Use **Stripe Elements** (never handle card data directly)
- Comply with **PCI DSS SAQ A** requirements
- Store only Stripe customer IDs, never card details
- Use HTTPS for all payment-related communications

**Priority**: Must Have

**Rationale**: Secure payment handling, regulatory compliance

**Verification Method**: PCI compliance audit, Stripe integration review

---

### NFR-SEC-003: Data Encryption

**Requirement**: The system SHALL:
- Encrypt calculation history at rest using **AES-256**
- Use **TLS 1.3** (minimum TLS 1.2) for data in transit
- Encrypt database backups
- Implement proper key rotation policies

**Priority**: Must Have (for Premium)

**Rationale**: Protect user data, build trust

**Verification Method**: Encryption audit, SSL Labs testing

---

### NFR-SEC-004: Input Sanitization

**Requirement**: The system SHALL:
- Sanitize all user inputs to prevent **XSS attacks**
- Validate all inputs on both client and server side
- Use parameterized queries to prevent **SQL injection**
- Implement **Content Security Policy (CSP)** headers

**Priority**: Must Have

**Rationale**: Prevent common web vulnerabilities

**Verification Method**: Security testing, OWASP ZAP scanning

---

### NFR-SEC-005: Vulnerability Management

**Requirement**: The system SHALL:
- Scan dependencies for vulnerabilities **weekly** (npm audit, Snyk)
- Apply security patches within **7 days** of disclosure
- Maintain security changelog
- Conduct security reviews before major releases

**Priority**: Must Have

**Rationale**: Proactive security posture

**Verification Method**: Automated vulnerability scanning, patch management tracking

---

## Accessibility Requirements

### NFR-ACCESS-001: WCAG 2.1 Level AA Compliance

**Requirement**: The application SHALL comply with **WCAG 2.1 Level AA** standards:

**Perceivable**:
- Text contrast ratio ≥ 4.5:1 for normal text (≥ 3:1 for large text)
- All UI components perceivable by screen readers
- No reliance on color alone to convey information

**Operable**:
- All functionality available via keyboard
- No keyboard traps
- Focus indicators visible (≥ 3:1 contrast)
- Timing adjustable (if any timed content)

**Understandable**:
- Clear, consistent navigation
- Error messages clear and helpful
- Predictable UI behavior

**Robust**:
- Valid HTML5 markup
- ARIA attributes used correctly
- Compatible with assistive technologies

**Priority**: Must Have

**Rationale**: Legal requirement for educational market, ethical imperative

**Verification Method**: Lighthouse accessibility audit, WAVE tool, manual screen reader testing

---

### NFR-ACCESS-002: Keyboard Navigation

**Requirement**: The system SHALL support complete keyboard navigation:
- **Tab** order follows logical visual flow
- All interactive elements reachable via keyboard
- **Escape** key dismisses modals/dialogs
- **Enter/Space** activates focused buttons
- No keyboard traps

**Priority**: Must Have

**Rationale**: Required for WCAG compliance, power user efficiency

**Verification Method**: Manual keyboard-only navigation testing

---

### NFR-ACCESS-003: Screen Reader Support

**Requirement**: The system SHALL be fully usable with screen readers:
- All buttons have descriptive labels (not just "button 7")
- Current calculation state announced
- Calculation results announced immediately
- Proper ARIA live regions for dynamic content
- Semantic HTML structure

**Priority**: Must Have

**Rationale**: WCAG requirement, inclusive design

**Verification Method**: Testing with NVDA, JAWS, VoiceOver

---

### NFR-ACCESS-004: Visual Requirements

**Requirement**: The system SHALL:
- Support browser zoom up to **200%** without loss of functionality
- Use minimum font size of **16px** for body text
- Provide sufficient spacing between interactive elements (≥ 44x44px touch targets)
- Support high contrast mode
- No content flashing more than 3 times per second

**Priority**: Must Have

**Rationale**: Support users with visual impairments

**Verification Method**: Visual testing at various zoom levels, contrast checking tools

---

## Usability Requirements

### NFR-USE-001: Learnability

**Requirement**: A new user SHALL be able to:
- Perform their first calculation within **10 seconds** of page load
- Understand how to view history within **30 seconds**
- Complete a premium upgrade within **2 minutes** (if motivated)

**Measurement**: User testing with first-time users

**Priority**: Must Have

**Rationale**: Low learning curve drives adoption

**Verification Method**: Usability testing with 5+ first-time users

---

### NFR-USE-002: Error Prevention

**Requirement**: The system SHALL:
- Prevent invalid input sequences (e.g., two decimal points)
- Provide clear visual feedback for button presses
- Show operation in progress before pressing equals
- Confirm destructive actions (clear history, cancel subscription)

**Priority**: Must Have

**Rationale**: Reduce user frustration, prevent mistakes

**Verification Method**: Usability testing, error tracking

---

### NFR-USE-003: User Feedback

**Requirement**: The system SHALL provide feedback:
- **Immediate** visual feedback for button clicks (<100ms)
- **Clear** error messages with guidance ("Cannot divide by zero. Try another operation.")
- **Success** confirmations for important actions ("History cleared", "Subscribed to Premium")
- Loading indicators for operations >500ms

**Priority**: Must Have

**Rationale**: Users need to understand system state

**Verification Method**: UX review, user testing

---

### NFR-USE-004: Help & Documentation

**Requirement**: The system SHALL provide:
- Inline help hints (tooltips) for unclear features
- "How to Use" documentation accessible from main page
- FAQ covering common questions
- Contact support option for premium users

**Priority**: Should Have

**Rationale**: Support self-service, reduce support burden

**Verification Method**: Documentation review, support ticket analysis

---

## Reliability Requirements

### NFR-REL-001: Availability

**Requirement**: The system SHALL achieve:
- **99.9% uptime** (≤ 43.8 minutes downtime/month)
- **99.95% uptime** for premium features (≤ 21.6 minutes downtime/month)
- Scheduled maintenance windows announced 48 hours in advance

**Measurement**: Uptime monitoring (Pingdom, UptimeRobot)

**Priority**: Must Have

**Rationale**: User trust, premium service expectations

**Verification Method**: Uptime monitoring, incident tracking

---

### NFR-REL-002: Data Integrity

**Requirement**: The system SHALL:
- **Never lose** premium user calculation history
- Perform automated database backups **every 6 hours**
- Maintain **30-day backup retention**
- Test backup restoration **monthly**
- Implement transaction logging for audit trail

**Priority**: Must Have

**Rationale**: Data loss unacceptable for paid service

**Verification Method**: Backup testing, disaster recovery drills

---

### NFR-REL-003: Error Rate

**Requirement**: The system SHALL maintain:
- **< 0.1%** calculation error rate
- **< 1%** API error rate (5xx errors)
- **< 2%** client-side error rate (JavaScript exceptions)

**Measurement**: Error logging and monitoring

**Priority**: Must Have

**Rationale**: Reliability builds trust and retention

**Verification Method**: Error monitoring (Sentry, LogRocket)

---

### NFR-REL-004: Graceful Degradation

**Requirement**: The system SHALL:
- Function without JavaScript (show upgrade message)
- Function with localStorage disabled (session-only history with warning)
- Handle network failures gracefully (show offline indicator)
- Preserve user input during transient errors

**Priority**: Should Have

**Rationale**: Resilience to browser/network issues

**Verification Method**: Testing with disabled features

---

## Compatibility Requirements

### NFR-COMPAT-001: Browser Support

**Requirement**: The system SHALL support the following browsers (last 2 major versions):

**Desktop**:
- ✅ Chrome 118+ (95%+ compatibility)
- ✅ Firefox 119+ (95%+ compatibility)
- ✅ Safari 17+ (95%+ compatibility)
- ✅ Edge 118+ (95%+ compatibility)

**Mobile**:
- ✅ Chrome Mobile (Android)
- ✅ Safari Mobile (iOS 16+)
- ✅ Samsung Internet

**Not Supported**:
- ❌ Internet Explorer (deprecated)
- ❌ Opera Mini (limited JS support)

**Priority**: Must Have

**Rationale**: Cover 98%+ of user base

**Verification Method**: Cross-browser testing (BrowserStack)

---

### NFR-COMPAT-002: Device Support

**Requirement**: The system SHALL function on:
- Desktop computers (Windows, macOS, Linux)
- Tablets (iPad, Android tablets)
- Smartphones (iOS, Android)
- Minimum screen size: **320px width** (iPhone SE)

**Priority**: Must Have

**Rationale**: Multi-device accessibility

**Verification Method**: Device testing, responsive design testing

---

### NFR-COMPAT-003: Operating System

**Requirement**: The system SHALL be OS-agnostic:
- No OS-specific code or dependencies
- Web-based, no installation required
- Compatible with all major OSes via browser

**Priority**: Must Have

**Rationale**: Maximum accessibility

**Verification Method**: Testing across OS platforms

---

## Maintainability Requirements

### NFR-MAINT-001: Code Quality

**Requirement**: The codebase SHALL maintain:
- **TypeScript strict mode** enabled
- **ESLint** compliance (no errors, warnings reviewed)
- **Prettier** formatting enforced
- **80%+ test coverage** for critical paths
- **90%+ test coverage** for core calculator logic

**Priority**: Must Have

**Rationale**: Long-term maintainability, reduce bugs

**Verification Method**: Static analysis, coverage reports

---

### NFR-MAINT-002: Documentation

**Requirement**: The codebase SHALL include:
- **README** with setup instructions
- **Architecture documentation** (system design, data flow)
- **API documentation** (endpoints, parameters, responses)
- **Component documentation** (Storybook or similar)
- **Inline code comments** for complex logic

**Priority**: Must Have

**Rationale**: Enable team collaboration, onboarding

**Verification Method**: Documentation review

---

### NFR-MAINT-003: Build & Deploy

**Requirement**: The system SHALL support:
- **One-command** local development setup
- **Automated** CI/CD pipeline (test, build, deploy)
- **< 10 minute** build and deploy time
- **Rollback capability** for failed deployments
- **Environment parity** (dev, staging, production)

**Priority**: Must Have

**Rationale**: Rapid iteration, reliable deployments

**Verification Method**: CI/CD pipeline review

---

### NFR-MAINT-004: Modularity

**Requirement**: The system SHALL be architectured with:
- **Clear separation of concerns** (UI, business logic, data)
- **Reusable components** (button, display, history panel)
- **Pluggable architecture** for future features (scientific calculator)
- **Dependency injection** where appropriate

**Priority**: Should Have

**Rationale**: Extensibility for future features

**Verification Method**: Architecture review

---

## Privacy & Compliance Requirements

### NFR-PRIV-001: Data Minimization

**Requirement**: The system SHALL:
- Collect **only** data necessary for functionality
- **Not** collect: IP addresses (beyond logs), user agent strings for tracking, browsing history
- **Collect** (free tier): No personal data (localStorage only)
- **Collect** (premium): Email, subscription status, encrypted calculation history

**Priority**: Must Have

**Rationale**: Privacy-first positioning, GDPR principle

**Verification Method**: Data audit, privacy review

---

### NFR-PRIV-002: GDPR Compliance

**Requirement**: The system SHALL provide:
- **Clear consent** for data collection (opt-in for emails)
- **Right to access** (users can view all their data)
- **Right to erasure** (users can delete their account and all data)
- **Right to portability** (users can export data)
- **Privacy by design** (default to minimal data collection)
- **Data processing records** (maintain processing activity register)

**Priority**: Must Have

**Rationale**: Legal requirement (EU), user trust

**Verification Method**: GDPR compliance audit

---

### NFR-PRIV-003: Cookie Policy

**Requirement**: The system SHALL:
- **Not use** third-party cookies
- **Only use** essential cookies (session management)
- Display cookie banner **only if** non-essential cookies are added in future
- Respect "Do Not Track" browser settings

**Priority**: Must Have

**Rationale**: Privacy positioning, GDPR compliance

**Verification Method**: Cookie audit

---

### NFR-PRIV-004: Third-Party Services

**Requirement**: The system SHALL:
- Limit third-party services to: **Stripe** (payments), **CDN** (static assets), **Email provider** (transactional emails)
- **Not use**: Google Analytics, Facebook Pixel, or similar tracking
- Document all third-party data processors
- Ensure all processors are GDPR-compliant

**Priority**: Must Have

**Rationale**: Privacy promise, data protection

**Verification Method**: Third-party service audit

---

## Performance Budgets Summary

| Metric | Target | Maximum |
|--------|--------|---------|
| **Page Load (TTI)** | 1s (broadband) | 3s (3G) |
| **Calculation Response** | 50ms | 100ms |
| **Bundle Size (gzipped)** | 50 KB | 100 KB |
| **Memory Usage** | 50 MB | 100 MB |
| **API Response (p95)** | 100ms | 500ms |
| **Lighthouse Performance** | 90+ | 80+ |
| **Lighthouse Accessibility** | 100 | 95+ |

---

## Browser Support Matrix

| Browser | Version | Support Level | Notes |
|---------|---------|---------------|-------|
| Chrome | 118+ | Full | Primary development target |
| Firefox | 119+ | Full | Test all releases |
| Safari | 17+ | Full | Test iOS Safari especially |
| Edge | 118+ | Full | Chromium-based |
| Chrome Mobile | Latest 2 | Full | Android primary mobile browser |
| Safari Mobile | iOS 16+ | Full | iOS primary mobile browser |
| Samsung Internet | 22+ | Full | Popular on Android |
| IE 11 | - | None | Not supported |

---

## Compliance Checklist

- ✅ **WCAG 2.1 Level AA** - Accessibility compliance
- ✅ **GDPR** - Data protection and privacy
- ✅ **PCI DSS SAQ A** - Payment security (via Stripe)
- ✅ **SOC 2 Type II** - Should pursue after 6 months (for enterprise sales)
- ⬜ **COPPA** - Not applicable (not targeting children <13)
- ⬜ **HIPAA** - Not applicable (no healthcare data)

---

**Requirements Engineer Sign-off**: Non-functional requirements complete and ready for System Architect review.
