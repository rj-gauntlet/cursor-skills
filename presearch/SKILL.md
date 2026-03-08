---
name: presearch
description: Ingest a product requirements document (PRD) and collaboratively walk through the requirements to develop a high-level architecture, strategy, implementation plan, and phased schedule. Use when the user wants to plan a project, review a PRD, create a project plan, define architecture from requirements, or kick off a new product build.
---

# Presearch — PRD to Project Plan

Turn a product requirements document into a validated architecture, strategy, and phased implementation plan through interactive collaboration.

## Trigger

The user provides or points to a PRD (any format: markdown, text, PDF, docx, or inline) and asks for help planning, architecting, or scheduling the project.

## Workflow

### Phase 1: Ingest & Understand

1. **Read the PRD.** Accept any file format. If the user pastes content inline, treat that as the PRD.
2. **Summarize back** the core product vision, target users, and key outcomes in 3-5 bullet points. Ask the user to confirm or correct.
3. **Extract requirements.** Build a numbered list of functional and non-functional requirements. Group them into logical domains (e.g., Auth, Data, UI, Integrations). Present the list and ask the user to validate, reprioritize, or flag gaps.

### Phase 2: Clarify & Discover

Walk through the requirements interactively at a moderate level of detail. For each domain:

1. **Surface assumptions** — state what you're assuming and ask the user to confirm.
2. **Identify ambiguities** — call out vague or missing requirements and ask targeted questions.
3. **Flag risks & dependencies** — highlight technical risks, external dependencies, or scope concerns.
4. **Discuss constraints** — ask about budget, team size, existing tech stack, must-use platforms, compliance needs, or other constraints.

**Options-driven discussion:** For each significant requirement or decision point, present 3-4 viable options in this format:

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A** | [approach] | [advantages] | [drawbacks] |
| **B** | [approach] | [advantages] | [drawbacks] |
| **C** | [approach] | [advantages] | [drawbacks] |

**Recommendation:** State which option you recommend and why, then let the user choose.

Use the AskQuestion tool when available to keep the conversation structured. Otherwise ask conversationally, batching related questions together (no more than 3-5 at a time to avoid overwhelming the user).

### Phase 3: Architecture & Strategy

Collaboratively develop the architecture. Continue the options-driven approach for major architectural decisions (e.g., database choice, frontend framework, hosting platform, auth strategy):

1. **Propose a high-level system architecture** — major components, their responsibilities, and how they interact. Include a text-based diagram (e.g., Mermaid or ASCII).
2. **Recommend a tech stack** — for each layer, present 3-4 options with pros/cons and a clear recommendation. Justify choices based on requirements and constraints gathered in Phase 2.
3. **Define the strategy** — build vs. buy decisions, MVP scope, iteration approach, and deployment strategy. Present options where multiple valid approaches exist.
4. **Review with the user** — present the above at moderate depth and iterate based on feedback. Continue until the user is satisfied.

### Phase 4: Plan & Schedule

1. **Ask for the target timeline or deadline.** If the user doesn't have one, collaboratively estimate a realistic one.
2. **Break the work into phased milestones** (default format). Each phase should include:
   - Phase name and goal
   - Key deliverables
   - Success criteria
   - Estimated duration
3. **Present the phased plan** and iterate with the user. Adjust scope, ordering, or timing as needed.

If the user requests a different scheduling format (e.g., sprints), adapt accordingly.

### Phase 5: Cost Analysis

Build a cost-of-development and cost-of-operation breakdown to support budgetary decision-making:

1. **Development costs** — estimate effort (person-hours or person-weeks) per phase, plus any paid tools, licenses, or services needed during development.
2. **Operational costs at scale** — project monthly/annual running costs at multiple user tiers (e.g., 100, 1K, 10K, 100K users). Cover compute, storage, bandwidth, third-party APIs, and managed services.
3. **Alternative cost comparison** — for each major tech decision made in Phase 3, show what the costs would look like with the runner-up alternatives. Present as a comparison table so the stakeholder can weigh budget against tradeoffs.
4. **Review with the user** — walk through the cost projections, adjust assumptions (e.g., traffic patterns, data volume, team rates), and iterate until the numbers feel realistic.

Use concrete pricing where possible (cloud provider calculators, published API pricing). Flag estimates that are rough and explain the assumptions behind them.

### Phase 6: Produce the Plan Document

Once the user is satisfied with the architecture, strategy, schedule, and cost analysis:

1. **Generate the detailed plan document** using the template in [plan-template.md](plan-template.md).
2. **Fill in all sections** with the specifics discussed, elevating to full detail:
   - Data models and key schemas
   - API surface and key interfaces
   - File/folder structure recommendations
   - Specific libraries and versions where relevant
   - Detailed phase breakdowns with task-level items
   - Full cost tables with scale projections and alternative comparisons
3. **Save the document** to the project root as `PROJECT_PLAN.md` (or ask the user where they'd like it).
4. **Present a summary** of what was created and suggest next steps.

## Interaction Guidelines

- **Be collaborative, not prescriptive.** Present recommendations, explain tradeoffs, and let the user decide.
- **Pace the conversation.** Don't dump everything at once. Move through phases sequentially, confirming alignment before proceeding.
- **Track progress.** Use the todo list to track which phases are complete.
- **Stay grounded.** Base all recommendations on the actual PRD content and user responses, not generic boilerplate.
- **Ask, don't assume.** When requirements are ambiguous, ask rather than guessing.
- **Adapt scope.** For small projects, phases can be lighter. For large ones, spend more time in each phase.

## Additional Resources

- For the output document template, see [plan-template.md](plan-template.md)
