# Agent Instructions: Project Structure & Code Implementation

> **⚠️ READ-ONLY FILE**: This file contains instructions for project structure and code implementation.  
> **All customizations go in `/agent-custom.md`**

This file is automatically loaded by AI agents when working with implementation roles (20-22: Frontend, Backend, Full-Stack Developers).

---

## Where Code Lives

**All actual code implementations go in the `/projects/` folder.**

This keeps ConceptShipAI framework files (docs, roles, agent.md) separate from your actual project code.

**Structure:**
```
/
├── agent.md                    # Core agent instructions
├── docs/                       # Framework documentation & artifacts
└── projects/                   # YOUR CODE GOES HERE ✅
    └── [your-projects]/
```

---

## Project Structure Options

The **Delivery Manager (Role 12)** decides project structure based on the **System Architecture** artifact (from Role 3).

### Simple Applications

For simple, single-component applications:

```
projects/
└── calculator-app/
    ├── src/
    │   ├── components/
    │   ├── utils/
    │   └── App.jsx
    ├── tests/
    ├── public/
    ├── .github/workflows/
    ├── package.json
    └── README.md
```

**Example projects:**
- Calculator web app
- Todo list application
- Simple landing page with contact form
- Single-page React/Vue application

### Multi-Component Applications

For applications with separate frontend/backend/infrastructure:

```
projects/
├── frontend/
│   ├── src/
│   ├── tests/
│   ├── package.json
│   └── README.md
├── backend/
│   ├── src/
│   ├── tests/
│   ├── requirements.txt
│   └── README.md
└── infrastructure/
    ├── terraform/
    ├── kubernetes/
    └── README.md
```

**Example projects:**
- Web application with API backend
- Mobile app with server-side logic
- SaaS product with separate frontend/backend
- Applications requiring infrastructure as code

### Microservices Architecture

For microservices or domain-driven design:

```
projects/
├── user-domain/
│   ├── user-service/
│   │   ├── src/
│   │   ├── tests/
│   │   └── README.md
│   └── authentication-service/
│       ├── src/
│       ├── tests/
│       └── README.md
├── order-domain/
│   ├── order-service/
│   │   ├── src/
│   │   └── tests/
│   └── payment-service/
│       ├── src/
│       └── tests/
└── shared/
    ├── common-lib/
    └── event-schemas/
```

**Example projects:**
- E-commerce platform with multiple services
- Enterprise applications with bounded contexts
- Distributed systems with service mesh
- Applications requiring independent scaling

**Key Principle:** The architecture determines the structure. The Delivery Manager has full flexibility to organize projects based on what the System Architect designed.

---

## Project Initialization

When the **Delivery Manager (Role 12)** begins the implementation phase, they must initialize the project structure **before** assigning stories to implementation roles.

### Initialization Process

Follow these steps **in order**:

#### 1. Review System Architecture

Read `docs/artifacts/03-system-architect/system-architecture.md`:
- What tech stack was chosen? (React, Next.js, Python/FastAPI, Go, Node.js, etc.)
- Single application or multiple services?
- Any framework-specific requirements?
- What databases/caching/messaging systems?

#### 2. Decide Project Structure

Based on architecture, choose structure:
- **Simple app?** → One folder: `projects/[app-name]/`
- **Frontend + Backend?** → Two folders: `projects/frontend/`, `projects/backend/`
- **Microservices?** → Nested: `projects/[domain]/[service]/`
- **Monorepo?** → Workspaces: `projects/packages/[package-name]/`

#### 3. Initialize Projects (Tech-Specific Commands)

Use the appropriate command for your tech stack:

##### React (Create React App)

```bash
npx create-react-app projects/[name]
cd projects/[name]
npm install
```

##### React (Vite - Recommended for modern React)

```bash
npm create vite@latest projects/[name] -- --template react
cd projects/[name]
npm install
```

##### Next.js

```bash
npx create-next-app projects/[name]
cd projects/[name]
npm install
```

##### Python/FastAPI

```bash
mkdir -p projects/[name]/src projects/[name]/tests
cd projects/[name]
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install fastapi uvicorn pytest pytest-cov
pip freeze > requirements.txt
```

##### Go

```bash
mkdir -p projects/[name]
cd projects/[name]
go mod init github.com/[org]/[name]
# Create src/ and tests/ directories as needed
mkdir -p cmd pkg internal
```

##### Node.js/Express

```bash
mkdir -p projects/[name]/src projects/[name]/tests
cd projects/[name]
npm init -y
npm install express
npm install --save-dev jest supertest nodemon
```

##### Vanilla JavaScript/HTML/CSS

```bash
mkdir -p projects/[name]/src projects/[name]/tests projects/[name]/public
cd projects/[name]
npm init -y
npm install --save-dev jest @testing-library/dom
# Create index.html, main.js, styles.css manually
```

##### TypeScript (Node.js)

```bash
mkdir -p projects/[name]/src projects/[name]/tests
cd projects/[name]
npm init -y
npm install typescript @types/node ts-node
npm install --save-dev jest @types/jest ts-jest
npx tsc --init
```

#### 4. Create Foundation Files

**Always create these base files:**

##### `.gitignore`

```gitignore
# Dependencies
node_modules/
venv/
__pycache__/

# Build outputs
dist/
build/
*.pyc

# Environment
.env
.env.local

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*
```

##### `README.md`

```markdown
# [Project Name]

## Description
[Brief description from project brief]

## Tech Stack
- [Frontend: React, Vue, etc.]
- [Backend: Python/FastAPI, Node.js/Express, etc.]
- [Database: PostgreSQL, MongoDB, etc.]

## Setup

### Prerequisites
- [Node.js 18+, Python 3.11+, etc.]

### Installation
```bash
# Installation commands
npm install
# or
pip install -r requirements.txt
```

### Running Locally
```bash
# Development commands
npm run dev
# or
uvicorn src.main:app --reload
```

### Testing
```bash
# Test commands
npm test
# or
pytest
```

## Architecture
See: `/docs/artifacts/03-system-architect/system-architecture.md`

## Documentation
See: `/docs/artifacts/11-documentation-writer/`
```

##### Package Manager Config

**For JavaScript/TypeScript (`package.json`):**
```json
{
  "name": "[project-name]",
  "version": "0.1.0",
  "scripts": {
    "dev": "[dev command]",
    "build": "[build command]",
    "test": "jest",
    "lint": "eslint src/"
  }
}
```

**For Python (`requirements.txt`):**
```
fastapi==0.104.1
uvicorn==0.24.0
pytest==7.4.3
pytest-cov==4.1.0
```

**For Go (`go.mod`):**
```
module github.com/[org]/[name]

go 1.21
```

##### CI/CD Configuration (if defined by DevOps Engineer)

Create `.github/workflows/` with deployment workflows based on `docs/artifacts/08-devops-engineer/` specifications.

#### 5. Commit Initial Structure

```bash
git add projects/[name]
git commit -m "Initialize [name] project - [tech stack]

- Structure: [simple/multi-component/microservices]
- Framework: [React/Next.js/FastAPI/Go/etc.]
- Initialized with: [create-react-app/vite/manual/etc.]
- Base files: .gitignore, README.md, package.json

Story: Project initialization
Timestamp: [UTC timestamp]"
```

#### 6. Document Initialization

Add note to project plan or create a work item:

```markdown
## Project Initialization Complete

**Structure**: projects/[name]/
**Tech Stack**: [framework/language versions]
**Initialized**: [YYYY-MM-DD HH:mm UTC]
**Commands Used**:
- `[initialization commands]`

**Files Created**:
- `.gitignore` - Excludes build artifacts and dependencies
- `README.md` - Project overview and setup
- `package.json` / `requirements.txt` / `go.mod` - Dependencies
- `src/` - Source code directory
- `tests/` - Test directory

**Next Steps**:
- Ready for story assignments
- Implementation roles can begin work
```

#### 7. Create Initial Work Items (Optional)

The Delivery Manager may want to create some initial technical stories:

- Setup linting and formatting
- Configure CI/CD pipeline
- Setup test framework
- Create base components/modules

#### 8. Assign First Story

**Only after project is initialized** should you assign stories to implementation roles (20-22).

Update `docs/work/assignments.md`:
```markdown
## Active Assignments

| Story ID | Title | Assigned To | Status |
|----------|-------|-------------|--------|
| 00001 | Setup project linting | Frontend Developer | assigned |
```

---

## Code File Creation

**CRITICAL: Implementation roles (20-22) MUST use the `create_file` tool to create all code files.**

### Why Use `create_file`?

- ✅ AI can actually create files in the project
- ✅ Clear record of what was created
- ✅ Integrates with version control
- ❌ Never just describe code - actually create it!

### File Path Conventions

All code files go under `/projects/[project-name]/`:

| File Type | Location | Example |
|-----------|----------|---------|
| Source code | `projects/[name]/src/` | `projects/app/src/components/Button.jsx` |
| Unit tests | `projects/[name]/tests/` | `projects/app/tests/Button.test.js` |
| Integration tests | `projects/[name]/tests/integration/` | `projects/app/tests/integration/api.test.js` |
| E2E tests | `projects/[name]/e2e/` or `cypress/` | `projects/app/e2e/login.spec.js` |
| Configuration | `projects/[name]/` (root) | `projects/app/tsconfig.json` |
| CI/CD | `projects/[name]/.github/workflows/` | `projects/app/.github/workflows/test.yml` |
| Documentation | `projects/[name]/docs/` | `projects/app/docs/api.md` |

### Examples

#### Frontend Component

```javascript
// Call the create_file tool with:
// Path: "projects/calculator-app/src/components/Calculator.jsx"
// Content: [full component code]

import React, { useState } from 'react';
import './Calculator.css';

export const Calculator = () => {
  // ... component implementation
};
```

#### Backend API Endpoint

```python
# Call the create_file tool with:
# Path: "projects/api-service/src/routes/users.py"
# Content: [full endpoint code]

from fastapi import APIRouter, HTTPException

router = APIRouter()

@router.post("/users")
async def create_user(user: UserCreate):
    # ... endpoint implementation
```

#### Test File

```javascript
// Call the create_file tool with:
// Path: "projects/calculator-app/tests/Calculator.test.js"
// Content: [full test code]

import { render, screen, fireEvent } from '@testing-library/react';
import { Calculator } from '../src/components/Calculator';

describe('Calculator', () => {
  test('displays initial value', () => {
    // ... test implementation
  });
});
```

#### CI/CD Workflow

```yaml
# Call the create_file tool with:
# Path: "projects/calculator-app/.github/workflows/deploy.yml"
# Content: [full workflow config]

name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      # ... workflow steps
```

---

## Test Driven Development (TDD) Workflow

**CRITICAL: All implementation roles MUST follow TDD. Tests are written BEFORE implementation code.**

### TDD Tool Usage Pattern

Implementation roles should follow this pattern **for every story**:

#### Step 1: Receive Story Assignment

- Delivery Manager assigns story via `docs/work/assignments.md`
- Implementation role reads story from `docs/work/features/[id]/stories/[id]-story.md`

#### Step 2: Extract Test Cases

- Read acceptance criteria from story
- Each acceptance criterion becomes a test case
- Identify edge cases and error scenarios

#### Step 3: Write Tests FIRST (RED 🔴)

```javascript
// Example: Frontend component test
// Path: "projects/app/tests/LoginForm.test.jsx"

import { render, screen, fireEvent } from '@testing-library/react';
import { LoginForm } from '../src/components/LoginForm';

describe('LoginForm', () => {
  test('renders email and password fields', () => {
    render(<LoginForm />);
    expect(screen.getByLabelText('Email')).toBeInTheDocument();
    expect(screen.getByLabelText('Password')).toBeInTheDocument();
  });
  
  test('shows error for invalid email', () => {
    render(<LoginForm />);
    fireEvent.change(screen.getByLabelText('Email'), {
      target: { value: 'invalid-email' }
    });
    fireEvent.click(screen.getByRole('button', { name: 'Login' }));
    expect(screen.getByText('Invalid email address')).toBeInTheDocument();
  });
  
  test('calls onSubmit with valid credentials', async () => {
    const mockSubmit = jest.fn();
    render(<LoginForm onSubmit={mockSubmit} />);
    // ... test implementation
  });
});
```

**Use `create_file` tool to create test file**

#### Step 4: Run Tests - Should FAIL

```bash
cd projects/app
npm test

# Expected: ❌ FAIL - Module not found '../src/components/LoginForm'
# This is CORRECT! Tests fail because implementation doesn't exist yet.
```

#### Step 5: Commit Tests (RED)

```bash
git add projects/app/tests/LoginForm.test.jsx
git commit -m "Add tests for login form - Story 00042 (TDD RED)"
```

#### Step 6: Write Minimal Code (GREEN 🟢)

```javascript
// Now create implementation
// Path: "projects/app/src/components/LoginForm.jsx"

import React, { useState } from 'react';

export const LoginForm = ({ onSubmit }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    // Validate email
    if (!email.includes('@')) {
      setError('Invalid email address');
      return;
    }
    
    // Call onSubmit
    onSubmit({ email, password });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="email">Email</label>
      <input
        id="email"
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      
      <label htmlFor="password">Password</label>
      <input
        id="password"
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      
      {error && <div className="error">{error}</div>}
      
      <button type="submit">Login</button>
    </form>
  );
};
```

**Use `create_file` tool to create implementation file**

#### Step 7: Run Tests - Should PASS

```bash
cd projects/app
npm test

# Expected: ✅ PASS - All tests pass!
```

#### Step 8: Commit Implementation (GREEN)

```bash
git add projects/app/src/components/LoginForm.jsx
git commit -m "Implement login form component - Story 00042 (TDD GREEN)"
```

#### Step 9: Refactor (Keep GREEN ♻️)

- Extract reusable logic
- Improve naming
- Add comments
- Optimize performance

**Keep running tests after each refactor to ensure they still pass!**

```bash
git add projects/app/src/components/LoginForm.jsx
git commit -m "Refactor login form for clarity - Story 00042 (TDD REFACTOR)"
```

#### Step 10: Update Story Audit Log

Add entry to `docs/work/features/[id]/stories/[id]-story.md`:

```markdown
## Audit Log

[2025-11-07 14:30 UTC] ASSIGNMENT: Assigned to Frontend Developer
[2025-11-07 14:45 UTC] STATUS: in-progress - Tests created (RED)
[2025-11-07 15:00 UTC] STATUS: in-progress - Implementation complete (GREEN)
[2025-11-07 15:10 UTC] STATUS: in-progress - Refactored for clarity
[2025-11-07 15:15 UTC] STATUS: in-review - PR #42 created
```

### TDD Commit Message Format

**Always use clear TDD phase indicators:**

```bash
# When writing tests
git commit -m "Add tests for [feature] - Story [ID] (TDD RED)"

# When implementing to pass tests
git commit -m "Implement [feature] - Story [ID] (TDD GREEN)"

# When refactoring
git commit -m "Refactor [feature] for [reason] - Story [ID] (TDD REFACTOR)"
```

---

## Git Strategy for Projects

### What Goes in Git

**✅ Commit these:**
- All source code (`projects/*/src/`)
- All tests (`projects/*/tests/`)
- Configuration files (`package.json`, `requirements.txt`, etc.)
- `.gitignore` files
- `README.md` and documentation
- CI/CD workflows (`.github/workflows/`)
- Database migrations

**❌ Never commit these:**
- Dependencies (`node_modules/`, `venv/`, `vendor/`)
- Build outputs (`dist/`, `build/`, `__pycache__/`)
- Environment files (`.env`, `.env.local`)
- IDE settings (`.vscode/`, `.idea/`)
- Log files (`*.log`)
- OS files (`.DS_Store`, `Thumbs.db`)

### .gitignore Files

**Root `.gitignore`** (global ignores):
```gitignore
# Global ignores
.DS_Store
Thumbs.db
*.log
.env
```

**Project `.gitignore`** (project-specific):
```gitignore
# JavaScript/TypeScript
node_modules/
dist/
build/
.next/
.nuxt/

# Python
venv/
__pycache__/
*.pyc
.pytest_cache/

# Go
vendor/
*.exe
*.test

# Environment
.env.local
.env.development
.env.production
```

### Commit Workflow

```bash
# 1. Make changes (write tests, implement code, refactor)
# 2. Stage changes
git add projects/[name]/

# 3. Commit with clear message
git commit -m "[Type] [Description] - Story [ID] (TDD [Phase])

[Optional longer description]

Files modified:
- projects/app/src/components/Button.jsx
- projects/app/tests/Button.test.jsx"

# 4. Push to remote (if working in branches)
git push origin feature/story-00042
```

### Branch Strategy (Optional)

For teams or complex projects:

```bash
# Create feature branch for story
git checkout -b feature/story-00042-login-form

# Make changes, commit with TDD phases
git commit -m "Add tests for login form - Story 00042 (TDD RED)"
git commit -m "Implement login form - Story 00042 (TDD GREEN)"

# Push and create PR
git push origin feature/story-00042-login-form
# Create PR on GitHub/GitLab
```

---

## Integration with Work Tracking

Work tracking in `docs/work/` tracks features and stories **across all projects**.

### Cross-Referencing

**In story files** (`docs/work/features/[id]/stories/[id]-story.md`):

```markdown
# Story 00042: Implement User Login

**Project**: projects/frontend/
**Feature**: 00015-user-authentication
**Assigned To**: Frontend Developer (Role 20)

## Acceptance Criteria
- [ ] Login form component created at `projects/frontend/src/components/LoginForm.jsx`
- [ ] Form validation with tests at `projects/frontend/tests/LoginForm.test.jsx`
- [ ] API integration with `projects/backend/src/routes/auth.py`

## Files Modified
- `projects/frontend/src/components/LoginForm.jsx` (created)
- `projects/frontend/tests/LoginForm.test.jsx` (created)
- `projects/frontend/src/api/auth.js` (created)

## Audit Log
[2025-11-07 14:30 UTC] ASSIGNMENT: Assigned to Frontend Developer
[2025-11-07 14:45 UTC] STATUS: in-progress - Created LoginForm.jsx
[2025-11-07 15:00 UTC] STATUS: done - Tests passing, PR merged
```

### Multi-Project Tracking

For complex systems with multiple projects:

```markdown
# Feature 00015: User Authentication System

**Projects Involved**:
- `projects/frontend/` - Login UI
- `projects/backend/` - Auth API
- `projects/database/` - User schema

**Stories**:
- Story 00042: Login form (frontend) - ✅ Complete
- Story 00043: Auth API endpoint (backend) - 🔄 In Progress
- Story 00044: User table migration (database) - ⏳ Not Started
```

---

## See Also

- **[agent.md](./agent.md)** - Core orchestration and role system
- **[agent-work-tracking.md](./agent-work-tracking.md)** - Work tracking, features, stories, assignments
- **[agent-handovers.md](./agent-handovers.md)** - Handover process and templates

---

**Remember**: This file contains instructions for WHERE and HOW to create code. The WHAT and WHY come from the planning artifacts in `docs/artifacts/`.
