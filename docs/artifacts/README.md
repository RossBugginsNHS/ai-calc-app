# Artifacts Folder

This folder contains all artifacts created by each role during the software development planning process.

## Structure

Each role has its own subfolder:

- `00-customer/` - Customer intake artifacts (project brief)
- `01-business-analyst/` - Business analysis artifacts
- `02-requirements-engineer/` - Requirements specifications
- `03-system-architect/` - Architecture design documents
- `04-security-architect/` - Security design and controls
- `05-ux-ui-designer/` - User experience and interface design
- `06-database-designer/` - Database schemas and data models
- `07-api-designer/` - API specifications
- `08-devops-engineer/` - Infrastructure and CI/CD configurations
- `09-test-architect/` - Testing strategies and plans
- `10-technical-lead/` - Implementation roadmaps and coding standards
- `11-documentation-writer/` - User guides and technical documentation
- `12-project-manager/` - Project plans and timelines

## Usage

The agent automatically creates artifacts in the appropriate role folder as it progresses through the workflow. Each artifact is timestamped and includes metadata about when and why it was created.

## Artifact Naming

Artifacts follow naming conventions from the role definitions in `docs/roles/`.

Example:
- `01-business-analyst/business-requirements.md`
- `03-system-architect/system-architecture.md`
- `10-technical-lead/implementation-roadmap.md`
