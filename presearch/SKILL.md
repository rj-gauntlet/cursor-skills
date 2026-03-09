---
name: presearch
description: Ingest a product requirements document (PRD) and collaboratively walk through the requirements to develop a high-level architecture, strategy, implementation plan, cost analysis, and phased schedule. Also supports refining an existing PROJECT_PLAN.md when requirements change. Use when the user wants to plan a project, review a PRD, create a project plan, define architecture from requirements, kick off a new product build, or update an existing project plan.
---

# Presearch — PRD to Project Plan

Turn a product requirements document into a validated architecture, strategy, and phased implementation plan through interactive collaboration.

## Modes

### Standard Mode (default)
Full interactive walkthrough — the agent presents options, discusses tradeoffs, and the user makes decisions at each step.

### Quick Mode
The user asks for speed (e.g., "just use your best judgment", "quick mode", "make the decisions for me"). The agent runs the same phases but auto-selects its recommended option at every decision point without pausing for confirmation. At the end, present a summary of all decisions made and give the user a chance to override any of them before producing the final document.

### Refinement Mode
Triggered when a `PROJECT_PLAN.md` already exists in the project and the user describes a change (new requirement, removed feature, budget change, tech constraint, updated PRD, etc.):

1. **Read the existing plan** to understand what was already decided.
2. **Identify affected sections** — determine which parts of the plan the change impacts (e.g., a new feature touches Architecture, Schedule, and Cost but not Product Overview).
3. **Walk through only the affected areas** using the same interactive style (options, pros/cons, recommendations). Leave unaffected sections intact.
4. **Update the document** — rewrite the affected sections in `PROJECT_PLAN.md`, preserving everything else.
5. **Present a change summary** — list what was modified and why.

## Trigger

The user provides or points to a PRD (any format: markdown, text, PDF, docx, or inline) and asks for help planning, architecting, or scheduling the project. Or the user describes changes to an existing plan.

## Workflow

### Phase 0: Detect Context

Before starting the walkthrough:

1. **Check for an existing plan.** If `PROJECT_PLAN.md` exists in the project root and the user is describing changes, switch to Refinement Mode.
2. **Detect existing project stack.** Scan the working directory for project markers (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pubspec.yaml`, `Gemfile`, `pom.xml`, etc.). If found, read them to extract the language, framework, and existing dependencies. These become hard constraints — all recommendations must fit the existing stack. Report what was detected and confirm with the user.
3. **Detect mode.** If the user asks for quick mode, note it and auto-select recommendations throughout. Otherwise, proceed with standard interactive mode.

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
2. **Recommend a tech stack** — for each layer, present 3-4 options with pros/cons and a clear recommendation. Justify choices based on requirements and constraints gathered in Phase 2. Respect existing stack constraints from Phase 0.
3. **Define the strategy** — build vs. buy decisions, MVP scope, iteration approach, and deployment strategy. Present options where multiple valid approaches exist.
4. **Identify shared interfaces** — call out types, utilities, or contracts that multiple features will depend on (e.g., a `User` type used by Auth, Profiles, and Billing; an API client wrapper used everywhere). Note which features depend on each shared interface — this determines build order in Phase 4.
5. **Review with the user** — present the above at moderate depth and iterate based on feedback. Continue until the user is satisfied.

**Web research:** Use WebSearch and WebFetch tools to verify recommendations with current information — API docs, SDK versions, pricing pages, framework guides. Prefer official documentation. Flag anything based solely on model knowledge with a note to verify.

### Phase 4: Plan & Schedule

1. **Ask for the target timeline or deadline.** If the user doesn't have one, collaboratively estimate a realistic one.
2. **Break the work into phased milestones** (default format). Each phase should include:
   - Phase name and goal
   - Key deliverables
   - Success criteria
   - Estimated duration
3. **Respect build order from shared interfaces.** Features that depend on shared types/utilities must come after the phase that creates them (typically a bootstrap/setup phase).
4. **Present the phased plan** and iterate with the user. Adjust scope, ordering, or timing as needed.

If the user requests a different scheduling format (e.g., sprints), adapt accordingly.

### Phase 5: Cost Analysis

Build a cost-of-development and cost-of-operation breakdown to support budgetary decision-making:

1. **Development costs** — estimate effort (person-hours or person-weeks) per phase, plus any paid tools, licenses, or services needed during development.
2. **Operational costs at scale** — project monthly/annual running costs at multiple user tiers (e.g., 100, 1K, 10K, 100K users). Cover compute, storage, bandwidth, third-party APIs, and managed services.
3. **Alternative cost comparison** — for each major tech decision made in Phase 3, show what the costs would look like with the runner-up alternatives. Present as a comparison table so the stakeholder can weigh budget against tradeoffs.
4. **Review with the user** — walk through the cost projections, adjust assumptions (e.g., traffic patterns, data volume, team rates), and iterate until the numbers feel realistic.

**Web research:** Use WebSearch to look up current pricing for cloud providers, APIs, and services rather than relying on potentially outdated model knowledge. Cite pricing sources.

### Phase 6: Produce the Plan Document

Once the user is satisfied with the architecture, strategy, schedule, and cost analysis:

1. **Generate the detailed plan document** using the template in [plan-template.md](plan-template.md).
2. **Fill in all sections** with the specifics discussed, elevating to full detail:
   - Data models and key schemas
   - API surface and key interfaces
   - Shared interfaces with dependent features listed
   - File/folder structure recommendations
   - Specific libraries and versions where relevant
   - Detailed phase breakdowns with task-level items
   - Full cost tables with scale projections and alternative comparisons
   - Detected stack constraints (if any)
3. **Save the document** to the project root as `PROJECT_PLAN.md` (or ask the user where they'd like it).
4. **Present a summary** of what was created and suggest next steps.

## Interaction Guidelines

- **Be collaborative, not prescriptive.** Present recommendations, explain tradeoffs, and let the user decide.
- **Pace the conversation.** Don't dump everything at once. Move through phases sequentially, confirming alignment before proceeding.
- **Track progress.** Use the todo list to track which phases are complete.
- **Stay grounded.** Base all recommendations on the actual PRD content and user responses, not generic boilerplate.
- **Ask, don't assume.** When requirements are ambiguous, ask rather than guessing.
- **Adapt scope.** For small projects, phases can be lighter. For large ones, spend more time in each phase.
- **Verify with web research.** For tech stack recommendations, pricing, and API details, use WebSearch/WebFetch to get current information. Flag when relying on model knowledge alone.
- **Respect existing stacks.** If Phase 0 detected an existing project, never recommend technologies that conflict with it.

## Additional Resources

- For the output document template, see [plan-template.md](plan-template.md)
