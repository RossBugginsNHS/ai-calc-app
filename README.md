# ConceptShipAI - From Idea to Shipped Software

> **Transform your ideas into working software through conversational AI**

ConceptShipAI is a structured framework that guides AI agents through 20 specialized roles to deliver complete software projects. From initial concept to shipped software, multiple AI personas act as your collaborative team - working with you like skilled colleagues to scope, design, build, and deliver your entire project.

## What is ConceptShipAI?

ConceptShipAI provides:
- **23 Specialist Roles**: From Customer Intake to Full-Stack Development
- **Conversational Planning**: Talk through your idea; the AI asks the right questions
- **Complete Implementation**: AI writes actual code, tests, and deploys
- **Comprehensive Artifacts**: 50+ documents covering every aspect of software development
- **Work Tracking**: Built-in system for managing features, stories, and assignments
- **Team Topologies**: Roles organized as Stream-Aligned, Enabling, Complicated-Subsystem, and Platform teams
- **Agile Core Values**: Built-in principles for iterative, value-driven delivery

## Quick Start for Humans

### 1. Start a New Project

```bash
# Clone or use this template
git clone https://github.com/nhsdigital/ai conceptshipai-myproject
cd conceptshipai-myproject
```

### 2. Customize for Your Organization (Optional)

Edit `agent-custom.md` with your preferences:
- Preferred technologies (Python, Node.js, AWS, Azure, etc.)
- Organizational standards and compliance requirements
- Communication style preferences
- Budget and timeline constraints

### 3. Talk to Your AI

Start a conversation with your AI agent. It will:
1. **Introduce itself** as your colleague in the Customer Intake role
2. **Work with you collaboratively** to understand your project
3. **Ask clarifying questions** to help you think through your idea
4. **Create a project brief** capturing what you discussed
5. **Transition through specialist roles** (Business Analyst, Architect, Designer, etc.)
6. **Generate comprehensive documentation** for each aspect of your project

### 4. Use Work Tracking (Optional)

If you want to track features and user stories for implementation:
- The AI can create features and user stories in `docs/work/`
- Each item gets a unique ID (e.g., `00001-user-authentication`)
- Track assignments, status changes, and blockers
- See [`docs/work-tracking-instructions.md`](docs/work-tracking-instructions.md) for details

## The 23 Roles

ConceptShipAI uses **Team Topologies** to organize roles:

### Stream-Aligned Teams (Deliver Value)
| Role | Name | Purpose |
|------|------|---------|
| 0 | **Customer** | Articulate vision, priorities, and constraints |
| 10 | **Technical Lead** | Plan implementation roadmap and coding standards |
| 11 | **Documentation Writer** | Create user guides and technical docs |
| 12 | **Delivery Manager** | Orchestrate delivery, manage backlog, and track progress |
| 13 | **User Researcher** | Conduct user research and validate designs |
| 20 | **Frontend Developer** | Implement user interfaces and client-side logic |
| 21 | **Backend Developer** | Implement APIs, business logic, and server-side code |
| 22 | **Full-Stack Developer** | Implement complete features from UI to database |

### Enabling Teams (Build Capability)
| Role | Name | Purpose |
|------|------|---------|
| 1 | **Business Analyst** | Analyze business needs and stakeholder requirements |
| 2 | **Requirements Engineer** | Define detailed functional and non-functional requirements |
| 5 | **UX/UI Designer** | Design user experience and interfaces |
| 8 | **DevOps Engineer** | Plan infrastructure, deployment, and CI/CD |
| 9 | **Test Architect** | Define testing strategies and quality assurance |
| 14 | **Information Governance Officer** | Ensure data governance and privacy compliance |
| 15 | **Safety Officer** | Ensure system safety in high-risk domains |
| 16 | **Accessibility Specialist** | Ensure WCAG compliance and inclusive design |
| 17 | **Performance Engineer** | Define performance requirements and optimization |
| 18 | **Compliance Officer** | Ensure regulatory compliance (GDPR, HIPAA, etc.) |
| 19 | **FinOps Analyst** | Optimize cloud costs and financial efficiency |

### Complicated-Subsystem Teams (Deep Expertise)
| Role | Name | Purpose |
|------|------|---------|
| 3 | **System Architect** | Design system architecture and technology choices |
| 6 | **Database Designer** | Design database schemas and data models |
| 7 | **API Designer** | Specify API contracts and integration patterns |

### Platform Team (Enable Self-Service)
| Role | Name | Purpose |
|------|------|---------|
| 4 | **Security Architect** | Design security controls and threat mitigations |

## What You Get

When you use ConceptShipAI, you get:

### 📋 Planning & Requirements
- Project brief with vision, goals, and constraints
- Business analysis with stakeholder mapping
- Detailed functional and non-functional requirements
- User research findings and personas

### 🏗️ Architecture & Design
- System architecture diagrams and technology choices
- Security architecture with threat models and controls
- UX/UI designs with wireframes and prototypes
- Database schema and data models
- API specifications and integration patterns

### � Implementation & Code
- **Working code** written by AI developers (frontend, backend, full-stack)
- **Tests** at all layers (unit, integration, E2E)
- **Database migrations** and schema implementations
- **Deployed features** ready for user testing
- **Iterative delivery** with continuous feedback

### 🚀 Infrastructure & Deployment
- Infrastructure as code (IaC)
- CI/CD pipeline implementations
- Testing strategy and automated quality gates
- Cost optimization and FinOps guidance

### 📚 Documentation & Governance
- User guides and technical documentation
- Project timeline, risks, and dependencies
- Information governance and data privacy compliance
- Accessibility compliance (WCAG)
- Safety requirements (for high-risk domains)

### 📊 Work Tracking
- Feature and user story management
- Unique IDs for all work items (format: `00001-feature-name`)
- Assignment tracking and status updates
- Audit logs for complete history

## Project Structure

```text
your-project/
├── docs/                          # All documentation
│   ├── artifacts/                 # Outputs from each role
│   │   ├── 00-customer/
│   │   ├── 01-business-analyst/
│   │   ├── ...                    # Planning roles 00-19
│   │   └── 19-finops-analyst/
│   ├── work/                      # Work tracking
│   │   ├── assignments.md
│   │   ├── recently-changed.md
│   │   ├── backlog/
│   │   ├── features/
│   │   │   ├── todo/
│   │   │   ├── in-progress/
│   │   │   ├── done/
│   │   │   └── blocked/
│   │   └── templates/
│   ├── roles/                     # Role definitions (00-22)
│   │   ├── 00-customer/
│   │   │   ├── default.md         # Standard behavior
│   │   │   └── custom.md          # Your customizations
│   │   ├── ...                    # Planning roles 00-19
│   │   ├── 20-frontend-developer/ # Implementation roles
│   │   ├── 21-backend-developer/
│   │   └── 22-full-stack-developer/
│   ├── handovers/                 # Session continuity
│   │   ├── handover.md
│   │   └── handover-histories/
│   ├── history/                   # Interaction logs
│   └── work-tracking-instructions.md
├── agent.md                       # AI agent instructions
├── agent-custom.md                # Your customizations
└── DEVELOPMENT-TRACKER.md         # For template development
```

## Customization

### Global Customizations (`/agent-custom.md`)

Customize the agent's behavior for your organization:
- Preferred technologies and frameworks
- Organizational standards and constraints
- Communication preferences
- Compliance requirements
- Budget and resource limitations

### Role-Specific Customizations (`docs/roles/*/custom.md`)

Each role can be customized with:
- Persona names for more engaging interactions
- Role-specific preferences and standards
- Custom templates and examples
- Industry-specific guidelines (e.g., healthcare, finance)

**Note**: The core instructions in `agent.md` and role `default.md` files are READ-ONLY. All customizations go in the appropriate `custom.md` files.

### 🎭 Role Personas
Each role can have a custom persona name for more engaging interactions.

## How It Works

### Conversational Planning

**You don't write a requirements document.** Instead, you have a natural conversation:

```
You: "Hi, I want to build a task management app for my team"

AI (as your colleague): "Great! Let's work together to scope this out. Tell me more 
                         about what you're envisioning - what problem are you solving?"

You: "Our team struggles to track tasks across multiple projects..."

AI: "I see. So multi-project task management. Have you thought about how many users? 
     What's most important - simplicity or advanced features?"

[Conversation continues - the AI helps you think through your idea]

AI: "Perfect. I've captured what we discussed in a project brief. Now I'll switch 
     to Business Analyst mode to analyze the business needs..."
```

### Role Transitions

The AI moves through roles sequentially:
1. **Completes current role** - Creates all artifacts for that specialty
2. **Announces transition** - "I've completed Business Analyst. Ready for Requirements Engineer?"
3. **Creates handover** (optional) - For long sessions, preserves context for next chat
4. **Starts next role** - Introduces itself in the new role and continues

### Session Continuity

**Short project?** Complete all roles in one conversation.

**Long project?** The AI creates handovers so you can:
- Take breaks between roles
- Start fresh chat sessions
- Resume exactly where you left off
- No context lost

## Customization

### Global Preferences (`agent-custom.md`)

Tell the AI about your organization:

- **Technologies**: "We use Python, PostgreSQL, and AWS"
- **Standards**: "All APIs must follow RESTful conventions"
- **Compliance**: "We're HIPAA-compliant healthcare"
- **Communication**: "Keep explanations simple - non-technical audience"
- **Budget**: "Looking for cost-effective solutions"

### Role-Specific (`docs/roles/*/custom.md`)

Each role can be customized:

- **Persona names**: Give roles friendly names for more engaging conversations
- **Industry focus**: Healthcare, finance, government-specific guidance
- **Templates**: Custom artifact templates for your organization
- **Standards**: Role-specific standards (e.g., "Security Architect: Always include OWASP Top 10")

**Example** (`docs/roles/03-system-architect/custom.md`):
```markdown
# Persona
Marcus Chen - Principal System Architect

# Preferences
- Always consider serverless-first architecture
- Prefer managed services over self-hosted
- Cloud provider: AWS
- Default to microservices for scalability
```

## Core Values (Built Into Every Role)

Every ConceptShipAI role operates with these Agile principles:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users
- **Iterate and Release** - No big bang; ship small increments frequently

## Tips for Success

### Getting the Best Results

**✅ Do:**
- **Be specific** about your constraints (budget, timeline, team size)
- **Ask questions** if something is unclear
- **Review artifacts** at each role transition
- **Provide feedback** - the AI learns from your preferences
- **Use handovers** for sessions longer than 1-2 hours
- **Customize** `agent-custom.md` with your organization's preferences

**❌ Don't:**
- **Rush through roles** - details matter for good planning
- **Hide constraints** - the AI needs to know your limitations
- **Skip reviews** - catch issues early
- **Let sessions go too long** - use handovers to break up work

### Skipping or Revisiting Roles

**Don't need a role?** (e.g., "No database required")
- Tell the AI you want to skip it
- It will create a minimal note explaining why
- Moves to the next role

**Need to go back?** (e.g., Requirements changed after architecture)
- Tell the AI you need to revisit an earlier role
- It will update the artifacts
- Documents why the revision was needed
- Continues forward

### Working with the AI

Think of it as your collaborative colleague, not a chatbot:
- It asks thoughtful questions to help you think things through
- It suggests considerations you might have missed
- It confirms understanding before proceeding
- It's okay to say "I don't know" - you'll figure it out together

## Work Tracking (Optional)

If you want to track implementation work:

### When to Use
- You're planning to implement the project
- You want to track features and user stories
- You need assignment and progress tracking
- You're working with a team

### How It Works
1. **Project Manager role** creates features with unique IDs (e.g., `00001-user-authentication`)
2. **Features** are broken into user stories (e.g., `00042-login-with-email`)
3. **Status tracking**: todo → in-progress → done → blocked
4. **Audit logs**: Every change is recorded (status, assignments, blockers)
5. **Recently changed**: Quick view of last 30 days of activity

See [`docs/work-tracking-instructions.md`](docs/work-tracking-instructions.md) for complete details.

## Frequently Asked Questions

### "How long does a full planning session take?"
- **Quick project**: 30-60 minutes (core roles only)
- **Standard project**: 2-4 hours (most roles)
- **Complex project**: Multiple sessions with handovers

### "Can I use only some roles?"
Yes! Skip roles that aren't relevant. The AI will document why they were skipped.

### "Can I add custom roles?"
Yes! Create new role definitions in `docs/roles/` and update the workflow sequence.

### "What if I need to change something later?"
Go back to any role, update the artifacts, and document the change in history.

### "Can multiple people use this for one project?"
Yes! The handover system allows different people to work on different roles. Commit changes to git for team coordination.

### "Does ConceptShipAI actually write code and build software?"

Yes! ConceptShipAI is designed for full software delivery - from planning through implementation. The AI agents can write code, configure infrastructure, and deliver working software.

### "What AI agents work with this?"
Any AI agent that can read files, write files, and follow instructions - Claude, GPT-4, etc.

## Getting Help

- **Agent Instructions**: [`agent.md`](agent.md) - Complete guide for the AI
- **Role Details**: `docs/roles/[role-name]/default.md` - Each role's responsibilities
- **Work Tracking**: [`docs/work-tracking-instructions.md`](docs/work-tracking-instructions.md)
- **Team Topologies**: [`docs/roles-and-artifacts.md`](docs/roles-and-artifacts.md)

## Contributing

This is an NHS Digital project. Contributions welcome! See [CHANGELOG.md](CHANGELOG.md) for recent changes.

## License

[License TBD]

---

**Ready to start planning your project?** Just say "Hi" to your AI agent and describe your idea! 🚀
