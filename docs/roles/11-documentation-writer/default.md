# Role 11: Documentation Writer

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: Documentation & Knowledge Management  
**Execution Order**: 11th  
**Duration Estimate**: 8-10% of total project planning time

---

## Core Values

Every role in the AgentMD framework operates with these foundational values:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users and stakeholders
- **Iterate and Release** - No big bang releases; ship small increments frequently to gather feedback and reduce risk

---

## Core Principles

These principles guide this role's work. Follow these unless overridden in `custom.md`.

1. **Use Mermaid for Diagrams** - All diagrams must be in Mermaid format.
2. **Use Markdown** - All documentation must be in Markdown.
3. **Code Blocks Must Specify Language** - Always include language identifiers in code blocks.
4. **Architecture Decision Records (ADRs)** - Document all significant decisions.
5. **README-Driven Development** - Every project needs a comprehensive README.
6. **OpenAPI/AsyncAPI Specifications** - API documentation must be complete and up-to-date.
7. **Changelog Maintenance** - Keep CHANGELOG.md updated with every release.

---

## Role Description

The Documentation Writer creates comprehensive, user-friendly documentation for all audiences - from end users to developers to operations teams. This role ensures that all stakeholders have the information they need to understand, use, develop, and maintain the system. Good documentation reduces support burden and enables self-service.

### Key Responsibilities

1. **User Documentation**: Create end-user guides and tutorials
2. **Developer Documentation**: Write technical documentation for developers
3. **API Documentation**: Complete API reference documentation
4. **Operations Documentation**: Create runbooks and operational guides
5. **README Files**: Write clear, comprehensive README files
6. **Knowledge Base**: Build searchable knowledge base
7. **Deployment Guides**: Step-by-step deployment instructions
8. **Troubleshooting Guides**: Common issues and solutions

### Core Activities

- Write user-facing documentation
- Create developer onboarding guides
- Document API endpoints with examples
- Create deployment runbooks
- Write troubleshooting guides
- Document system architecture for new team members
- Create video tutorials (optional)
- Build FAQ sections
- Maintain documentation changelog
- Organize documentation structure

---

## Input Artifacts

### Required Inputs

**All previous artifacts**, with special focus on:
- `docs/requirements/user-stories.md` → User guide structure
- `docs/design/api-specification.md` → API documentation
- `docs/architecture/system-architecture.md` → Architecture overview
- `docs/implementation/development-setup.md` → Developer guide
- `docs/planning/deployment-strategy.md` → Deployment guide
- `docs/design/wireframes.md` → User interface documentation

---

## Output Artifacts

The Documentation Writer produces five comprehensive documentation sets:

### 1. `docs/implementation/readme.md`

**Purpose**: Project overview and quick start

**Contents**:

```markdown
# Project Name

> Brief one-sentence description of what this project does

[![Build Status](https://img.shields.io/github/workflow/status/org/repo/CI)](link)
[![Coverage](https://img.shields.io/codecov/c/github/org/repo)](link)
[![License](https://img.shields.io/github/license/org/repo)](LICENSE)
[![Version](https://img.shields.io/npm/v/package-name)](link)

## Overview

[Project Name] is a [brief description of what the system does and why it exists]. It provides [key features/capabilities] for [target users].

## Key Features

- ✨ **User Authentication**: Secure registration and login with JWT
- 🛍️ **Product Catalog**: Browse and search products with advanced filtering
- 🛒 **Shopping Cart**: Add products and manage cart
- 💳 **Payment Processing**: Secure payment via Stripe/PayPal
- 📦 **Order Management**: Track orders from placement to delivery
- 📱 **Responsive Design**: Works seamlessly on desktop, tablet, and mobile
- 🔒 **Security**: Industry-standard security practices
- ⚡ **Performance**: Optimized for speed and scalability

## Quick Start

### Prerequisites

- Node.js 18+ or Python 3.11+
- PostgreSQL 15+
- Redis 7+
- Docker (optional, for local development)

### Installation

```bash
# Clone repository
git clone https://github.com/company/project.git
cd project

# Install dependencies
npm install  # or: pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your configuration

# Setup database
npm run migrate

# Start development server
npm run dev
```

Visit http://localhost:3000

## Documentation

- 📖 [User Guide](docs/implementation/user-guide.md) - How to use the application
- 👨‍💻 [Developer Guide](docs/implementation/developer-guide.md) - Development setup and contribution
- 🔌 [API Documentation](docs/implementation/api-documentation.md) - Complete API reference
- 🚀 [Deployment Guide](docs/implementation/deployment-guide.md) - How to deploy to production

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌──────────────┐
│   Client    │────▶│   API       │────▶│   Database   │
│  (React)    │     │  (Node.js)  │     │ (PostgreSQL) │
└─────────────┘     └─────────────┘     └──────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    Redis    │
                    │   (Cache)   │
                    └─────────────┘
```

See [System Architecture](docs/architecture/system-architecture.md) for detailed architecture.

## Technology Stack

**Frontend**:
- React 18 with TypeScript
- Redux for state management
- Material-UI components
- Axios for API calls

**Backend**:
- Node.js with Express
- PostgreSQL database
- Redis for caching
- JWT authentication

**Infrastructure**:
- Docker for containerization
- Kubernetes for orchestration
- AWS for cloud hosting
- GitHub Actions for CI/CD

## Development

### Running Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch
```

### Code Quality

```bash
# Lint code
npm run lint

# Format code
npm run format

# Type check
npm run typecheck
```

### Database Migrations

```bash
# Create new migration
npm run migrate:create migration_name

# Run migrations
npm run migrate

# Rollback last migration
npm run migrate:rollback
```

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## Support

- 📧 Email: support@example.com
- 💬 Slack: [Community Slack](https://slack.example.com)
- 🐛 Issues: [GitHub Issues](https://github.com/org/repo/issues)
- 📚 Docs: [Documentation Site](https://docs.example.com)

## Team

- **Project Lead**: Name ([@github](https://github.com/user))
- **Tech Lead**: Name ([@github](https://github.com/user))
- **Contributors**: [All Contributors](https://github.com/org/repo/graphs/contributors)

## Acknowledgments

- Thanks to all contributors
- Inspired by [similar projects]
- Built with [technologies]

---

Made with ❤️ by [Company/Team Name]
```

### 2. `docs/implementation/user-guide.md`

**Purpose**: End-user documentation

**Contents**:

```markdown
# User Guide

Welcome to [Project Name]! This guide will help you get started and make the most of all features.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Account Management](#account-management)
3. [Browsing Products](#browsing-products)
4. [Shopping Cart](#shopping-cart)
5. [Checkout & Payment](#checkout--payment)
6. [Order Management](#order-management)
7. [Account Settings](#account-settings)
8. [Troubleshooting](#troubleshooting)
9. [FAQs](#faqs)

## Getting Started

### Creating an Account

1. Click **Sign Up** in the top right corner
2. Enter your email address
3. Choose a username
4. Create a secure password (min. 8 characters, must include uppercase, lowercase, and number)
5. Click **Create Account**
6. Check your email for verification link
7. Click the link to verify your account

**Tips**:
- Use a strong, unique password
- Enable two-factor authentication for extra security
- Keep your email address up to date

### Logging In

1. Click **Login** in the top right corner
2. Enter your email and password
3. Click **Sign In**

**Forgot your password?**
1. Click **Forgot Password** on login page
2. Enter your email address
3. Check your email for reset link
4. Click link and create new password

## Browsing Products

### Viewing Products

The product catalog displays all available products. You can:
- **Browse** all products on the homepage
- **Search** for specific items using the search bar
- **Filter** by category, price range, or rating
- **Sort** by newest, price (low to high), or popularity

### Product Details

Click any product to see:
- Product images (click to enlarge)
- Detailed description
- Price and availability
- Customer reviews and ratings
- Specifications
- Shipping information

## Shopping Cart

### Adding Items to Cart

1. On product page, click **Add to Cart**
2. Cart icon updates with item count
3. Continue shopping or click cart to review

### Managing Your Cart

In your cart, you can:
- **Update quantity**: Use +/- buttons
- **Remove items**: Click X or "Remove"
- **Save for later**: Move items to wishlist
- **Apply coupon codes**: Enter code in coupon field

**Cart shows**:
- Item subtotal
- Estimated shipping
- Tax (if applicable)
- Total amount

## Checkout & Payment

### Checkout Process

1. Review items in cart
2. Click **Proceed to Checkout**
3. Select or add shipping address
4. Choose shipping method
5. Select payment method
6. Review order summary
7. Click **Place Order**

### Payment Methods

We accept:
- 💳 Credit/debit cards (Visa, Mastercard, Amex)
- 🅿️ PayPal
- 🍎 Apple Pay
- 📱 Google Pay

**Payment Security**:
- All payments are encrypted (SSL/TLS)
- We never store full card numbers
- PCI-DSS compliant
- Fraud detection enabled

### Order Confirmation

After successful payment:
- Order confirmation page displays
- Confirmation email sent to your inbox
- Order appears in Order History

## Order Management

### Viewing Orders

1. Click your profile icon
2. Select **Order History**
3. View all past and current orders

### Order Status

Track your order through these statuses:
- 📝 **Pending**: Order received, awaiting processing
- 📦 **Processing**: Order being prepared
- 🚚 **Shipped**: Order shipped, tracking available
- ✅ **Delivered**: Order delivered
- ❌ **Cancelled**: Order cancelled

### Tracking Shipments

1. Go to **Order History**
2. Click order to view details
3. Click **Track Shipment**
4. View real-time tracking information

### Cancelling Orders

You can cancel orders that haven't shipped:
1. Go to **Order History**
2. Click order to cancel
3. Click **Cancel Order**
4. Confirm cancellation
5. Refund processed within 5-7 business days

**Note**: Cannot cancel orders already shipped

## Account Settings

### Profile Information

Update your profile:
1. Click profile icon → **Settings**
2. Update name, email, phone
3. Add profile photo
4. Click **Save Changes**

### Addresses

Manage shipping addresses:
1. Go to **Settings** → **Addresses**
2. Add, edit, or remove addresses
3. Set default shipping address

### Payment Methods

Manage saved payment methods:
1. Go to **Settings** → **Payment Methods**
2. Add new card or PayPal account
3. Remove old payment methods
4. Set default payment method

**Security Note**: We only store last 4 digits of cards

### Notifications

Control email notifications:
1. Go to **Settings** → **Notifications**
2. Toggle notification preferences:
   - Order confirmations
   - Shipping updates
   - Promotional emails
   - Product recommendations

## Troubleshooting

### Common Issues

**Problem**: Can't log in
- **Solution**: Use "Forgot Password" to reset
- Check email is correct
- Try different browser
- Clear browser cache

**Problem**: Payment declined
- **Solution**: Check card details are correct
- Ensure sufficient funds
- Contact your bank
- Try different payment method

**Problem**: Order not showing up
- **Solution**: Check confirmation email
- Allow 5 minutes for system update
- Check spam folder for confirmation
- Contact support if still not visible

**Problem**: Shipping address wrong
- **Solution**: Contact support immediately if order not yet shipped
- Cannot change address after shipping

### Contact Support

Need help?
- 📧 **Email**: support@example.com (24-48 hour response)
- 💬 **Live Chat**: Available Mon-Fri 9am-5pm EST
- 📞 **Phone**: 1-800-123-4567
- 🐛 **Report Bug**: support@example.com

## FAQs

**Q: How long does shipping take?**  
A: Standard shipping: 5-7 business days. Express: 2-3 business days.

**Q: What is your return policy?**  
A: 30-day return policy. Items must be unused in original packaging.

**Q: Do you ship internationally?**  
A: Currently shipping to US and Canada only.

**Q: How do I use a coupon code?**  
A: Enter code at checkout in the "Coupon Code" field.

**Q: Is my payment information secure?**  
A: Yes, we use industry-standard encryption and never store full card numbers.

**Q: Can I change my order after placing it?**  
A: Contact support immediately. Changes possible if order hasn't shipped.

**Q: How do I delete my account?**  
A: Contact support@example.com to request account deletion.

---

*Last updated: November 7, 2025*
```

### 3. `docs/implementation/developer-guide.md`

**Purpose**: Developer onboarding and contribution guide

**Contents**:

```markdown
# Developer Guide

Welcome to the [Project Name] development team! This guide will help you get set up and contributing quickly.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Development Workflow](#development-workflow)
3. [Code Organization](#code-organization)
4. [Development Guidelines](#development-guidelines)
5. [Testing](#testing)
6. [Debugging](#debugging)
7. [Common Tasks](#common-tasks)
8. [Troubleshooting](#troubleshooting)

## Getting Started

### Prerequisites

Install required software:
- **Node.js** 18+ (LTS version recommended)
- **PostgreSQL** 15+
- **Redis** 7+
- **Docker** (optional, for easier local development)
- **Git**
- **VS Code** (recommended IDE)

### Initial Setup

1. **Clone Repository**
```bash
git clone https://github.com/company/project.git
cd project
```

2. **Install Dependencies**
```bash
npm install
```

3. **Environment Configuration**
```bash
# Copy example environment file
cp .env.example .env

# Edit .env with your local configuration
# Required variables:
# - DATABASE_URL
# - REDIS_URL
# - JWT_SECRET
```

4. **Database Setup**
```bash
# Create database
createdb project_dev

# Run migrations
npm run migrate

# Seed development data
npm run seed
```

5. **Start Development Server**
```bash
npm run dev
```

Visit http://localhost:3000 - you should see the application running!

### IDE Setup (VS Code)

Recommended extensions:
- ESLint
- Prettier
- GitLens
- Docker
- Thunder Client (API testing)

Install extensions:
```bash
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
```

## Development Workflow

### Branch Strategy

We use GitFlow:
- `main`: Production code
- `develop`: Integration branch
- `feature/*`: New features
- `fix/*`: Bug fixes
- `hotfix/*`: Production hotfixes

### Creating a Feature

1. **Create Branch**
```bash
git checkout develop
git pull origin develop
git checkout -b feature/your-feature-name
```

2. **Make Changes**
- Write code following our [coding standards](./coding-standards.md)
- Write tests for new functionality
- Update documentation as needed

3. **Commit Changes**
```bash
git add .
git commit -m "feat(scope): description"
```

Follow [commit message conventions](./coding-standards.md#git-commit-messages)

4. **Push and Create PR**
```bash
git push origin feature/your-feature-name
```

Create Pull Request on GitHub:
- Target: `develop` branch
- Fill out PR template
- Request reviews from 2+ team members

5. **Code Review**
- Address review comments
- Ensure CI passes
- Get approvals

6. **Merge**
- Squash and merge to develop
- Delete feature branch

## Code Organization

### Project Structure

```
project/
├── src/
│   ├── controllers/      # Request handlers
│   ├── models/           # Data models
│   ├── services/         # Business logic
│   ├── middleware/       # Express middleware
│   ├── routes/           # API routes
│   ├── utils/            # Utility functions
│   ├── validators/       # Input validation
│   └── __tests__/        # Test files
├── migrations/           # Database migrations
├── config/               # Configuration files
├── public/               # Static assets
├── docs/                 # Documentation
└── scripts/              # Utility scripts
```

### Architecture Pattern

We follow a **layered architecture**:

```
Request → Route → Controller → Service → Repository → Database
                     ↓
               Middleware (auth, validation, error handling)
```

**Example Flow**:
1. **Route** (`src/routes/users.js`): Defines HTTP endpoint
2. **Controller** (`src/controllers/UserController.js`): Handles HTTP request/response
3. **Service** (`src/services/UserService.js`): Contains business logic
4. **Repository/Model** (`src/models/User.js`): Database access

## Development Guidelines

### Code Style

- Follow [ESLint rules](.eslintrc.js)
- Use Prettier for formatting
- Write meaningful variable names
- Keep functions small (<50 lines)
- Comment complex logic

**Run linter**:
```bash
npm run lint
npm run lint:fix  # Auto-fix issues
```

### Creating a New API Endpoint

Example: Add `GET /api/v1/users/:id` endpoint

1. **Define Route** (`src/routes/users.js`):
```javascript
router.get('/:id', authenticate, UserController.getUser);
```

2. **Create Controller** (`src/controllers/UserController.js`):
```javascript
async getUser(req, res, next) {
  try {
    const user = await UserService.getUserById(req.params.id);
    res.json({ success: true, data: user });
  } catch (error) {
    next(error);
  }
}
```

3. **Implement Service** (`src/services/UserService.js`):
```javascript
async getUserById(id) {
  const user = await User.findById(id);
  if (!user) {
    throw new NotFoundError('User', id);
  }
  return user;
}
```

4. **Write Tests** (`src/controllers/__tests__/UserController.test.js`):
```javascript
describe('GET /api/v1/users/:id', () => {
  it('should return user when found', async () => {
    const response = await request(app)
      .get('/api/v1/users/1')
      .set('Authorization', `Bearer ${token}`);
    
    expect(response.status).toBe(200);
    expect(response.body.data.id).toBe(1);
  });
});
```

## Testing

### Running Tests

```bash
# All tests
npm test

# Specific file
npm test -- src/services/__tests__/UserService.test.js

# Watch mode
npm run test:watch

# Coverage
npm run test:coverage
```

### Writing Tests

Follow **AAA pattern** (Arrange, Act, Assert):

```javascript
it('should create user with valid data', async () => {
  // Arrange
  const userData = {
    email: 'test@example.com',
    password: 'SecurePass123'
  };
  
  // Act
  const user = await UserService.createUser(userData);
  
  // Assert
  expect(user.email).toBe(userData.email);
  expect(user.id).toBeDefined();
});
```

**Test Coverage Goals**:
- Overall: 80%+
- New code: 90%+
- Critical paths: 100%

## Debugging

### Using VS Code Debugger

1. Set breakpoints in code
2. Press F5 or click "Run and Debug"
3. Select "Node.js" configuration
4. Application starts in debug mode

### Logging

Use structured logging:

```javascript
import logger from './utils/logger';

logger.info('User created', { userId: user.id });
logger.error('Failed to create user', { error, email });
```

**Log Levels**:
- `error`: Errors and exceptions
- `warn`: Warnings
- `info`: Important events
- `debug`: Detailed debugging (dev only)

### Database Queries

Enable query logging in development:

```javascript
// config/database.js
logging: process.env.NODE_ENV === 'development' ? console.log : false
```

## Common Tasks

### Adding a Database Migration

```bash
# Create migration
npm run migrate:create add_column_to_users

# Edit migration file in migrations/
# Run migration
npm run migrate

# Rollback if needed
npm run migrate:rollback
```

### Adding a New Model

1. Create model file: `src/models/Product.js`
2. Define schema and relationships
3. Export model
4. Add to `src/models/index.js`

### Adding Environment Variable

1. Add to `.env.example` with placeholder
2. Update `.env` with actual value
3. Access in code: `process.env.VARIABLE_NAME`
4. Document in this guide

## Troubleshooting

### Port Already in Use

```bash
# Find process using port 3000
lsof -i :3000

# Kill process
kill -9 <PID>
```

### Database Connection Issues

```bash
# Check PostgreSQL is running
pg_isready

# Restart PostgreSQL
brew services restart postgresql  # macOS
sudo systemctl restart postgresql # Linux
```

### Redis Connection Issues

```bash
# Check Redis is running
redis-cli ping  # Should return "PONG"

# Start Redis
redis-server
```

### Clear Node Modules

```bash
rm -rf node_modules package-lock.json
npm install
```

## Additional Resources

- [API Documentation](./api-documentation.md)
- [Coding Standards](./coding-standards.md)
- [Architecture Overview](../architecture/system-architecture.md)
- [Deployment Guide](./deployment-guide.md)

## Getting Help

- 💬 **Team Slack**: #dev-team channel
- 📧 **Tech Lead**: techlead@example.com
- 📚 **Internal Wiki**: [wiki.company.com](https://wiki.company.com)
- 🐛 **Report Issues**: GitHub Issues

Welcome to the team! 🎉
```

### 4. `docs/implementation/api-documentation.md`

**Purpose**: Complete API reference (generated from OpenAPI spec with examples)

**Contents**:

```markdown
# API Documentation

Complete API reference for [Project Name] API v1.

**Base URL**: `https://api.example.com/v1`

## Authentication

All API requests (except public endpoints) require authentication using JWT Bearer tokens.

### Getting a Token

**Endpoint**: `POST /auth/login`

**Request**:
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response**:
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
    "expiresIn": 3600
  }
}
```

### Using the Token

Include token in Authorization header:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### Example (curl):
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://api.example.com/v1/users/me
```

## Rate Limiting

- **Authenticated**: 1000 requests/hour
- **Unauthenticated**: 100 requests/hour

Rate limit headers included in response:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1699358400
```

## Error Handling

All errors follow consistent format:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {},
    "requestId": "req_abc123"
  }
}
```

[Continue with full API endpoint documentation based on api-specification.md...]
```

### 5. `docs/implementation/deployment-guide.md`

**Purpose**: Production deployment instructions

**Contents**: *(Based on deployment-strategy.md and infrastructure-as-code.md)*

```markdown
# Deployment Guide

Step-by-step guide to deploy [Project Name] to production.

## Prerequisites

- AWS account with appropriate permissions
- Kubernetes cluster provisioned
- Domain name configured
- SSL certificates obtained
- CI/CD pipeline configured

## Deployment Checklist

- [ ] Environment variables configured
- [ ] Database migrations ready
- [ ] SSL certificates installed
- [ ] DNS configured
- [ ] Monitoring enabled
- [ ] Backup strategy in place
- [ ] Rollback plan documented
- [ ] Team notified of deployment

## Production Deployment

### Step 1: Prepare Infrastructure

```bash
# Initialize Terraform
cd infrastructure/
terraform init

# Review plan
terraform plan -var-file=prod.tfvars

# Apply infrastructure
terraform apply -var-file=prod.tfvars
```

[Continue with detailed deployment steps...]
```

---

## Quality Criteria

Before completing this role, ensure:

- [ ] README is clear and comprehensive
- [ ] User guide covers all features
- [ ] Developer guide enables quick onboarding
- [ ] API documentation is complete with examples
- [ ] Deployment guide is step-by-step and tested
- [ ] All documentation uses consistent terminology
- [ ] Screenshots/diagrams included where helpful
- [ ] Documentation is version controlled
- [ ] Links between documents work correctly
- [ ] Troubleshooting sections are comprehensive

---

## Transition to Next Role

The Documentation Writer's outputs inform:

**To Project Manager**:
- All documentation → Final review
- Deployment guide → Launch planning

**To End Users**:
- User guide → Customer success

**To Developers**:
- Developer guide → Team onboarding
- API docs → Integration development

---

## Tips for Success

1. **Write for Your Audience**: Adjust technical level appropriately
2. **Use Examples**: Code examples are worth 1000 words
3. **Keep It Updated**: Documentation that's wrong is worse than none
4. **Test Instructions**: Follow your own guides to find gaps
5. **Use Visuals**: Diagrams, screenshots, and videos help
6. **Be Consistent**: Use same terminology throughout
7. **Link Liberally**: Connect related documentation
8. **Think Questions**: Anticipate what users will ask
9. **Version Documentation**: Match docs to software versions
10. **Get Feedback**: Ask users if docs are helpful

---

**Previous Role**: [Technical Lead](./10-technical-lead.md)  
**Next Role**: [Project Manager](./12-project-manager.md)
