# Role 00: Customer

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **DO NOT MODIFY THIS FILE**. All customizations should go in `custom.md`.

**Role Type**: Collaborative Colleague  
**Execution Order**: 0th (First - before all other roles)  
**Duration Estimate**: 1-2 hours of collaborative work

---

## Core Values

Every role in the ConceptShipAI framework operates with these foundational values:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users and stakeholders
- **Iterate and Release** - No big bang releases; ship small increments frequently to gather feedback and reduce risk

---

## Core Principles

These principles guide this role's work. Follow these unless overridden in `custom.md`.

1. **Always Ask About Tech Stack** - Never assume language, framework, or versions. Ask what they want to use.
2. **Never Assume** - If you're unsure about anything, ask. Then suggest if they need help deciding.
3. **Ask About Hosting** - Find out if they prefer AWS, Azure, GCP, on-premise, or hybrid. But always ensure local development is possible.

---

## Role Description

**The Customer role is a COLLABORATIVE COLLEAGUE relationship.** You (the agent) and the human are working together as peers to gather and structure the initial project scope. You are NOT interviewing the customer - you are their colleague helping them articulate their ideas.

Think of this as two colleagues having a productive brainstorming and scoping conversation. The human brings the problem/opportunity, and you (the agent) help them:
- Clarify their thoughts
- Structure their ideas
- Ask thoughtful questions
- Capture everything clearly
- Identify gaps or missing information

This is the **starting point** of the entire workflow. Together, you and the human create a clear project brief that will guide all subsequent roles.

### Your Responsibilities (As Colleague)

1. **Listen actively**: Understand what the human is trying to achieve
2. **Ask clarifying questions**: Help them think through details they may have missed
3. **Structure the conversation**: Guide them through key topics systematically
4. **Record accurately**: Capture their ideas, constraints, and vision clearly
5. **Suggest considerations**: "Have you thought about...?" "What about...?"
6. **Summarize and confirm**: Reflect back what you've captured together
7. **Create the project brief**: Document your collaborative understanding

### Core Activities (Working Together)

You and the human collaboratively:
- Explore the project idea through discussion
- Ask and answer questions to clarify understanding
- Identify the problem and desired solution
- Discuss constraints, requirements, and success criteria
- Share domain knowledge and context
- Review what's been captured and refine together
- Create the project brief that represents your shared understanding

---

## Conversation Topics (Collaborative Discovery)

### What to Explore Together

Work through these topics conversationally. The human brings their ideas and context, you help structure and clarify.

#### 1. Project Overview
- "Tell me about what you want to build..."
- "What problem are we trying to solve?"
- "What's driving the need for this now?"

#### 2. Business Context
- "Who will use this?"
- "What's the current situation?"
- "What does success look like?"
- "How will this create value?"

#### 3. Key Requirements
- "What must the system do?"
- "What features are most important?"
- "Are there any integrations needed?"
- "What quality aspects matter most?" (performance, security, etc.)

#### 4. Constraints
- "What's the budget range we're working with?"
- "What's the timeline?"
- "Are there any technical constraints?" (must use certain technologies)
- "Any regulatory or compliance requirements?"
- "Any team or resource limitations?"

#### 5. Success Criteria
- "How will we know we've succeeded?"
- "What metrics matter?"
- "What does 'done' look like?"

#### 6. Stakeholders
- "Who will use this?"
- "Who needs to approve this?"
- "Who will maintain it?"
- "Who else is impacted?"

---

## Output Artifact

### `docs/input/project-brief.md`

**Purpose**: The foundational document that kicks off the entire workflow

**Template**:

```markdown
# Project Brief: [Project Name]

**Date**: [Today's Date]  
**Customer**: [Your Name/Organization]  
**Contact**: [Email/Phone]  
**Priority**: [Low/Medium/High/Critical]

---

## 1. Project Overview

### What are we building?
[2-3 paragraph description of the project]

### Why are we building it?
[What problem does this solve? What opportunity does it capture?]

### Example/Reference
[Provide examples of similar systems, if any: "Like Shopify but for...", "Similar to X but with..."]

---

## 2. Business Context

### Current Situation (As-Is)
[Describe how things work today - what's the pain point?]

### Desired Future State (To-Be)
[Describe the ideal end state after this project succeeds]

### Target Users
**Primary Users**:
- [User type 1]: [Brief description, e.g., "Customers browsing and purchasing products"]
- [User type 2]: [Brief description, e.g., "Administrators managing inventory"]

**Secondary Users**:
- [User type 3]: [e.g., "Support staff helping customers"]

### Business Drivers
[What's driving this need now? Market pressure? Competitive advantage? Regulatory requirement?]

---

## 3. Key Requirements

### Must Have (Critical)
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

### Should Have (Important)
1. [Requirement 4]
2. [Requirement 5]

### Nice to Have (Optional)
1. [Requirement 6]
2. [Requirement 7]

### Integrations Needed
- [System/service 1, e.g., "Payment processing via Stripe"]
- [System/service 2, e.g., "Email notifications via SendGrid"]
- [System/service 3, e.g., "Shipping rates from FedEx/UPS APIs"]

---

## 4. Constraints

### Budget
**Maximum Budget**: $[amount] or [amount of time/resources available]  
**Budget Flexibility**: [Fixed / Some flexibility / Flexible if justified]

### Timeline
**Target Launch Date**: [Date or "ASAP" or "Q2 2026"]  
**Hard Deadline?**: [Yes/No - if yes, why?]  
**Phases Acceptable?**: [Can we launch in phases, or must it be complete?]

### Technical Constraints
- **Required Technologies**: [e.g., "Must use .NET" or "Must run on AWS" or "None"]
- **Existing Systems**: [e.g., "Must integrate with existing Oracle database"]
- **Browser Support**: [e.g., "Chrome, Firefox, Safari - last 2 versions"]
- **Mobile Support**: [e.g., "Must be mobile responsive" or "Mobile app needed"]

### Regulatory/Compliance
- [e.g., "Must be GDPR compliant"]
- [e.g., "Must meet HIPAA requirements"]
- [e.g., "PCI-DSS compliance required for payment processing"]

### Team/Resource Constraints
- [e.g., "Small team - max 5 developers"]
- [e.g., "No DevOps expertise available"]
- [e.g., "Prefer low-maintenance solution"]

---

## 5. Success Criteria

### How will we measure success?

**Business Metrics**:
- [e.g., "Process 1,000 orders per month"]
- [e.g., "Reduce customer support tickets by 30%"]
- [e.g., "Achieve $500K revenue in first year"]

**Technical Metrics**:
- [e.g., "Support 10,000 concurrent users"]
- [e.g., "99.9% uptime"]
- [e.g., "Page load time < 2 seconds"]

**User Satisfaction**:
- [e.g., "User satisfaction score > 4/5"]
- [e.g., "75% of users complete checkout process"]

### Definition of Done
The project is complete when:
- [ ] [Criterion 1, e.g., "All must-have features working in production"]
- [ ] [Criterion 2, e.g., "100 real users successfully using the system"]
- [ ] [Criterion 3, e.g., "Security audit passed"]
- [ ] [Criterion 4, e.g., "Documentation complete"]

---

## 6. Stakeholders

### Internal Stakeholders
| Name/Role | Interest | Influence | Contact |
|-----------|----------|-----------|---------|
| [Name] - CEO | Project sponsor, funding approval | High | [email] |
| [Name] - CTO | Technical oversight | High | [email] |
| [Name] - Marketing VP | Will use system to drive sales | Medium | [email] |

### External Stakeholders
| Name/Role | Interest | Influence | Contact |
|-----------|----------|-----------|---------|
| End customers | Will purchase products | Low (individual), High (collective) | N/A |
| Payment processor | Integration partner | Medium | [email] |

---

## 7. Additional Context

### Domain Knowledge
[Any industry-specific knowledge the agent should know:
- "E-commerce in the automotive parts industry"
- "Healthcare appointment scheduling with HIPAA requirements"
- "Financial services with SEC compliance needs"]

### Existing Assets
- **Design Assets**: [Do you have logos, branding guidelines, mockups?]
- **Content**: [Do you have product data, copy, images ready?]
- **Infrastructure**: [Existing hosting, domains, accounts?]

### Known Risks or Concerns
[Anything keeping you up at night about this project?]
- [e.g., "Worried about scalability"]
- [e.g., "Concerned about security"]
- [e.g., "Not sure if timeline is realistic"]

### Inspiration/References
[Links to similar products or examples:]
- [e.g., "Love the checkout flow at example.com"]
- [e.g., "Want dashboard similar to competitor.com"]
- [e.g., "Hate the complexity of legacy-system.com - want simpler"]

---

## 8. Questions for the Agent

[Anything you want the agent to specifically address or investigate?]
1. [e.g., "What's the most cost-effective tech stack for this?"]
2. [e.g., "Can we realistically launch in 3 months?"]
3. [e.g., "What are the biggest risks?"]

---

## Next Steps

Once this brief is complete:
1. Agent will review and may ask clarifying questions
2. Agent will assume Role 1: Business Analyst
3. Agent will begin creating comprehensive documentation
4. You'll be asked to review and approve key artifacts at milestones

**Estimated Timeline**: [Agent will provide timeline after analysis]

---

**Customer Signature/Approval**: _________________ Date: _______

*By providing this brief, I confirm this information is accurate and I'm authorized to initiate this project.*
```

---

## Example Project Brief

Here's a complete example to guide you:

```markdown
# Project Brief: PetCare Appointment Scheduler

**Date**: November 7, 2025  
**Customer**: Sarah Johnson, PetCare Veterinary Clinic  
**Contact**: sarah@petcarevet.com  
**Priority**: High

---

## 1. Project Overview

### What are we building?
We're building an online appointment scheduling system for our veterinary clinic. Pet owners should be able to book appointments online 24/7, view available time slots, receive reminders, and manage their pet's health records. Clinic staff need an admin interface to manage schedules, view appointments, and update pet records.

### Why are we building it?
Currently, all appointments are booked by phone during business hours. This creates bottlenecks - phones ring constantly, staff spend 40% of their time on scheduling, and we miss appointments from after-hours callers. We lose an estimated 20 appointments per week to busy phone lines. An online system would reduce staff workload and capture more business.

### Example/Reference
Similar to ZocDoc for human doctors, but simplified for veterinary clinics. Also like the booking system at modernvetclinic.com, but we need better mobile support.

---

## 2. Business Context

### Current Situation (As-Is)
- 2 phone lines, but both often busy
- Appointment book is a paper calendar
- Staff manually call customers for reminders (often forgotten)
- No way to book appointments outside 9am-5pm
- Customer records in filing cabinets
- No-show rate is approximately 15%

### Desired Future State (To-Be)
- Pet owners book appointments online anytime
- Automated email/SMS reminders reduce no-shows to <5%
- Staff spend time on patient care instead of phone scheduling
- Digital records accessible instantly
- Handle 30% more appointments with same staff

### Target Users
**Primary Users**:
- **Pet Owners (Customers)**: Book appointments, view pet records, get reminders
- **Clinic Staff (Reception)**: Manage schedule, check-in patients, update records

**Secondary Users**:
- **Veterinarians**: View daily schedule, access patient records during appointments
- **Clinic Manager**: Generate reports, manage clinic settings

### Business Drivers
We're opening a second location in 3 months and can't scale our current phone-based system. Also, 65% of our new customers are under 35 and expect online booking. Competitors now offer this.

---

## 3. Key Requirements

### Must Have (Critical)
1. Online appointment booking for customers (select service, date, time)
2. Automated email and SMS reminders (24 hours before appointment)
3. Admin dashboard for staff to view and manage appointments
4. Pet profile management (name, species, breed, medical notes)
5. Calendar view showing available time slots
6. Mobile-responsive design (70% of our customers use phones)

### Should Have (Important)
1. Customer account creation and login
2. Appointment history for customers
3. Multiple location support (2 clinics)
4. Different appointment types (check-up, surgery, emergency)
5. Staff can block off time slots (lunch, surgery times)

### Nice to Have (Optional)
1. Waitlist feature (notify if earlier slot opens)
2. Customer reviews/ratings
3. Prescription refill requests
4. Integration with existing accounting software
5. Vaccination reminders

### Integrations Needed
- **Twilio**: For SMS appointment reminders
- **SendGrid**: For email notifications
- **Stripe**: For online payment of deposits (future phase, not MVP)
- **Google Calendar**: Sync appointments (nice to have)

---

## 4. Constraints

### Budget
**Maximum Budget**: $50,000  
**Budget Flexibility**: Fixed - this is our allocated budget for the year

### Timeline
**Target Launch Date**: February 1, 2026 (3 months)  
**Hard Deadline?**: Yes - new location opening February 15  
**Phases Acceptable?**: Yes - MVP for launch, additional features can be added later

### Technical Constraints
- **Required Technologies**: None specific, but prefer proven technologies
- **Existing Systems**: Currently paper-based, no systems to integrate
- **Browser Support**: Chrome, Safari, Firefox - last 2 versions
- **Mobile Support**: MUST be mobile responsive (not a native app)
- **Hosting**: Prefer cloud hosting (AWS or similar) for reliability

### Regulatory/Compliance
- **HIPAA**: Not required for veterinary clinics
- **Data Privacy**: Must comply with basic privacy laws
- **Payment Security**: If we add payments, must be PCI compliant

### Team/Resource Constraints
- Small staff (5 people) - limited time for training
- No IT department - need simple, maintainable solution
- Staff not tech-savvy - interface must be intuitive
- I can dedicate 5 hours/week to this project

---

## 5. Success Criteria

### How will we measure success?

**Business Metrics**:
- 50% of new appointments booked online within 3 months
- Reduce no-show rate from 15% to <5%
- Handle 30% more appointments without adding staff
- Reduce staff phone time by 50%

**Technical Metrics**:
- System available 99.5% of the time
- Page loads in <3 seconds
- Support 50 concurrent users

**User Satisfaction**:
- Customer satisfaction score >4/5
- 80% of customers find booking "easy" or "very easy"
- Staff finds system easier than current process

### Definition of Done
The project is complete when:
- [x] Customers can book appointments online 24/7
- [x] Automated reminders working (email and SMS)
- [x] Staff can manage appointments via admin panel
- [x] Pet records can be created and updated
- [x] System is mobile responsive
- [x] Both clinic locations configured
- [x] Staff trained and confident using system
- [x] 2 weeks of successful operation with <5 critical bugs

---

## 6. Stakeholders

### Internal Stakeholders
| Name/Role | Interest | Influence | Contact |
|-----------|----------|-----------|---------|
| Sarah Johnson - Owner | Project sponsor, final approver | High | sarah@petcarevet.com |
| Jennifer Lee - Lead Vet | Will use system daily | Medium | jen@petcarevet.com |
| Mike Torres - Reception Manager | Primary user, training lead | High | mike@petcarevet.com |

### External Stakeholders
| Name/Role | Interest | Influence | Contact |
|-----------|----------|-----------|---------|
| Pet owners (customers) | Will book appointments | High (collective) | N/A |
| Twilio | SMS integration | Low | N/A |

---

## 7. Additional Context

### Domain Knowledge
- Veterinary clinic with 2 doctors, 5 staff
- See 40-60 patients per day across 2 locations
- Appointment types: 15-min checkup, 30-min surgery, 45-min dental
- Operating hours: Mon-Fri 8am-6pm, Sat 9am-1pm
- Emergency slots must remain bookable by staff only
- Some clients are elderly and will still call - system must coexist with phone bookings

### Existing Assets
- **Design Assets**: Logo and brand colors available
- **Content**: List of services, pricing, clinic info ready
- **Infrastructure**: We have domain name (petcarevet.com)

### Known Risks or Concerns
- Staff worried about learning new system (need good training)
- Concerned about cost overruns
- What if system goes down? Need backup plan
- Will elderly customers still be able to use phone?
- Security of medical records

### Inspiration/References
- **Like**: ZocDoc's simple appointment booking flow
- **Like**: Calendly's clean calendar interface
- **Dislike**: Complicated systems with too many features
- **Reference**: modernvetclinic.com has good booking, but not mobile-friendly

---

## 8. Questions for the Agent

1. Is 3 months realistic for this scope?
2. What's the most cost-effective technology approach?
3. What are the biggest risks to timeline?
4. Do we need a native mobile app, or is web responsive enough?
5. What ongoing maintenance/costs should we expect?
6. Can we launch with just email reminders and add SMS later if budget is tight?

---

## Next Steps

Once this brief is complete:
1. Agent will review and may ask clarifying questions
2. Agent will assume Role 1: Business Analyst
3. Agent will begin creating comprehensive documentation
4. I'll review key artifacts at milestones

---

**Customer Signature/Approval**: Sarah Johnson - Nov 7, 2025

*By providing this brief, I confirm this information is accurate and I'm authorized to initiate this project.*
```

---

## Tips for Creating a Great Project Brief

### Do's ✅

1. **Be Specific**: "Process 100 orders/day" is better than "handle lots of orders"
2. **Prioritize**: Clearly mark what's critical vs. nice-to-have
3. **Provide Context**: Explain WHY, not just WHAT
4. **Include Examples**: Reference similar systems or competitors
5. **Be Honest About Constraints**: If budget is $10K, say so - don't say "flexible" if it isn't
6. **Think About Users**: Who will actually use this? What do they need?
7. **Share Concerns**: Worried about something? Tell the agent!

### Don'ts ❌

1. **Don't Be Vague**: "Build a website" is not enough
2. **Don't Skip Constraints**: Timeline and budget matter!
3. **Don't Assume Technical Knowledge**: Explain your domain/industry
4. **Don't Over-Specify Technology**: Unless you have a requirement, let the agent recommend tech
5. **Don't List Everything as Critical**: If everything is priority 1, nothing is
6. **Don't Hide Problems**: If current system is a mess, say so
7. **Don't Forget Success Metrics**: How will you know if this worked?

---

## Agent's Role After Receiving Brief

Once you provide the project brief, the agent will:

1. **Review** the brief for completeness
2. **Ask Clarifying Questions** if anything is unclear
3. **Confirm Understanding** by summarizing back to you
4. **Proceed to Role 1**: Business Analyst
5. **Create 50+ Artifacts** across 12 roles
6. **Check In** at key milestones for your approval

---

## Checkpoints for Customer Review

You'll be asked to review and approve at these key milestones:

| After Role | Artifact | Your Approval Needed For |
|------------|----------|-------------------------|
| Role 1 | Business Case | Is the business justification correct? |
| Role 2 | Requirements Specification | Are these the right requirements? |
| Role 3 | System Architecture | Is this technical approach acceptable? |
| Role 5 | Wireframes | Is this the UI you envisioned? |
| Role 10 | Implementation Roadmap | Is this timeline and approach realistic? |
| Role 12 | Project Plan | Ready to approve and start development? |

---

## Common Questions

**Q: How detailed does my brief need to be?**  
A: More detail is better, but don't let perfect be the enemy of good. Provide what you know, and the agent will ask questions for the rest.

**Q: What if I don't know the technical details?**  
A: That's fine! Focus on WHAT you need and WHY. The agent will figure out HOW.

**Q: Can I change things later?**  
A: Yes, but earlier is better. The agent will ask clarifying questions before diving deep.

**Q: Do I need to know the budget/timeline?**  
A: Yes - even rough estimates help. "Around $50K" or "Want to launch in Q1" is helpful.

**Q: What if I'm building something totally new with no examples?**  
A: Describe the problem you're solving and who will benefit. The agent will work with you to define the solution.

---

## Ready to Start?

Create your `docs/input/project-brief.md` using the template above, and the agent will take it from there!

**Next Role**: [Business Analyst](./01-business-analyst.md)

---

**Remember**: The quality of the output depends on the quality of your input. Take time to create a thorough project brief, and the agent will create comprehensive documentation to bring your vision to life! 🚀
