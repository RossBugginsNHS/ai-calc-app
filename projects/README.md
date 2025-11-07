# Projects

This directory contains all software projects developed using the AgentMD framework.

## Structure

Each project should follow this standard structure:

```
projects/
└── [project-name]/
    ├── README.md           # Project overview and documentation
    ├── CHANGELOG.md        # Version history and changes
    ├── src/                # Source code
    ├── tests/              # Test suites (unit, integration, e2e)
    ├── docs/               # Additional documentation
    ├── infra/              # Infrastructure as Code
    └── .gitignore          # Project-specific ignores
```

## Project Structure Guidelines

### src/
Contains all application source code:
- Organized by domain/feature (DDD)
- Clear separation of concerns
- Follow chosen language conventions

### tests/
All test files organized by type:
- `unit/` - Unit tests (TDD)
- `integration/` - Integration tests
- `e2e/` - End-to-end tests
- `performance/` - Load/performance tests

### docs/
Additional project documentation:
- Architecture diagrams (Mermaid)
- API documentation (OpenAPI/AsyncAPI)
- Architecture Decision Records (ADRs)
- Runbooks and operational guides

### infra/
Infrastructure as Code:
- Container definitions (Dockerfile, docker-compose.yml)
- Kubernetes manifests (if applicable)
- Terraform/CloudFormation/ARM templates
- CI/CD pipeline configurations
- Environment configurations

## Creating a New Project

When the agent completes the planning workflow (Roles 0-12), it should:

1. Create a new project folder: `projects/[project-name]/`
2. Initialize the standard structure
3. Populate with artifacts from `docs/artifacts/`
4. Set up initial infrastructure templates
5. Create project README with setup instructions

## Example Projects

This directory will contain projects such as:
- `projects/example-api/` - RESTful API microservice
- `projects/example-web/` - Frontend web application
- `projects/example-worker/` - Background worker service
- `projects/shared/` - Shared libraries and utilities
