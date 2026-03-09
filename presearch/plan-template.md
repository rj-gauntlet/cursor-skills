# Plan Document Template

Use this template when generating the final `PROJECT_PLAN.md`. Replace all bracketed placeholders with specifics from the collaborative session. Remove any sections that don't apply.

---

```markdown
# [Project Name] — Project Plan

> Generated from PRD review on [date]

## 1. Product Overview

### Vision
[One-paragraph summary of what the product is and why it matters]

### Target Users
[Who this is for and their key needs]

### Key Outcomes
- [Outcome 1]
- [Outcome 2]
- [Outcome 3]

---

## 2. Requirements Summary

### Functional Requirements

| ID | Domain | Requirement | Priority |
|----|--------|-------------|----------|
| FR-01 | [domain] | [requirement] | Must-have |
| FR-02 | [domain] | [requirement] | Should-have |

### Non-Functional Requirements

| ID | Category | Requirement | Target |
|----|----------|-------------|--------|
| NFR-01 | Performance | [requirement] | [metric] |
| NFR-02 | Security | [requirement] | [standard] |

### Assumptions
- [Assumption 1]
- [Assumption 2]

### Open Questions
- [Any unresolved questions from the session]

---

## 3. Architecture

### System Overview

[Text-based architecture diagram using Mermaid or ASCII]

### Component Breakdown

#### [Component Name]
- **Responsibility:** [what it does]
- **Key interfaces:** [APIs it exposes or consumes]
- **Technology:** [specific tech choice and version]

[Repeat for each major component]

### Data Models

#### [Entity Name]
```
[Schema or model definition with field types]
```

[Repeat for each key entity]

### API Surface

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | /api/[resource] | [description] | [yes/no] |
| POST | /api/[resource] | [description] | [yes/no] |

### Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Frontend | [tech] | [why] |
| Backend | [tech] | [why] |
| Database | [tech] | [why] |
| Hosting | [tech] | [why] |

### Detected Stack Constraints
[If an existing project was detected, list the language, framework, and dependencies that constrained recommendations. If greenfield, state "Greenfield — no existing constraints."]

### Shared Interfaces

Types, utilities, or contracts that multiple features depend on. These determine build order — features listed as dependents cannot start until the interface is created.

| Interface | Location | Purpose | Depended on by |
|-----------|----------|---------|----------------|
| [Type/utility name] | [file path] | [what it defines] | [Feature 1, Feature 3] |
| [Type/utility name] | [file path] | [what it defines] | [Feature 2, Feature 4, Feature 5] |

---

## 4. Strategy

### Build vs. Buy
| Capability | Decision | Rationale |
|-----------|----------|-----------|
| [capability] | Build / Buy / Open-source | [why] |

### MVP Scope
[What's in the MVP and what's explicitly deferred]

### Iteration Approach
[How the product will evolve after MVP — user feedback loops, metrics to watch]

### Deployment Strategy
[How and where the product will be deployed, CI/CD approach]

---

## 5. Project Structure

```
[project-name]/
├── [folder]/
│   ├── [subfolder]/
│   └── [file]
├── [folder]/
└── [config files]
```

---

## 6. Implementation Plan

### Timeline
- **Start date:** [date or TBD]
- **Target completion:** [date or TBD]
- **Total estimated duration:** [duration]

### Phase 1: [Phase Name] — [duration]

**Goal:** [what this phase achieves]

**Deliverables:**
- [ ] [Deliverable 1]
- [ ] [Deliverable 2]

**Key Tasks:**
1. [Task with enough detail to act on]
2. [Task with enough detail to act on]

**Success Criteria:**
- [How to know this phase is done]

**Risks:**
- [Risk and mitigation]

---

### Phase 2: [Phase Name] — [duration]

[Same structure as Phase 1]

---

### Phase [N]: [Phase Name] — [duration]

[Same structure as Phase 1]

---

## 7. Cost Analysis

### Development Costs

| Phase | Effort Estimate | Paid Tools / Licenses | Phase Cost |
|-------|----------------|----------------------|------------|
| [Phase 1] | [person-weeks] | [tools and costs] | [total] |
| [Phase 2] | [person-weeks] | [tools and costs] | [total] |
| **Total** | | | **[total]** |

*Assumptions: [hourly/daily rate used, team size, etc.]*

### Operational Costs at Scale

| Component | 100 users/mo | 1K users/mo | 10K users/mo | 100K users/mo |
|-----------|-------------|-------------|--------------|---------------|
| Compute | [cost] | [cost] | [cost] | [cost] |
| Database | [cost] | [cost] | [cost] | [cost] |
| Storage | [cost] | [cost] | [cost] | [cost] |
| Bandwidth | [cost] | [cost] | [cost] | [cost] |
| Third-party APIs | [cost] | [cost] | [cost] | [cost] |
| Auth / Identity | [cost] | [cost] | [cost] | [cost] |
| **Monthly Total** | **[total]** | **[total]** | **[total]** | **[total]** |

*Assumptions: [avg requests per user, data per user, traffic pattern, pricing source]*

### Alternative Cost Comparison

For each major technology decision, compare the chosen option against alternatives:

#### [Decision Area — e.g., Database]

| Option | Monthly Cost @ 1K users | Monthly Cost @ 100K users | Notes |
|--------|------------------------|--------------------------|-------|
| **[Chosen option]** | [cost] | [cost] | Selected — [rationale] |
| [Alternative A] | [cost] | [cost] | [key tradeoff] |
| [Alternative B] | [cost] | [cost] | [key tradeoff] |

[Repeat for each major decision: hosting, auth, storage, etc.]

### Cost Summary

| Category | Low Estimate | High Estimate |
|----------|-------------|---------------|
| Total development | [amount] | [amount] |
| Monthly ops (at [target scale]) | [amount] | [amount] |
| Annual ops (at [target scale]) | [amount] | [amount] |

---

## 8. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|-----------|------------|
| [risk] | High/Med/Low | High/Med/Low | [mitigation] |

---

## 9. Next Steps

1. [Immediate next action]
2. [Second action]
3. [Third action]
```

---

## Template Usage Notes

- **Depth:** Fill in all sections with specific, actionable detail drawn from the session — data model fields, library versions, concrete task descriptions.
- **Adaptability:** Remove sections that don't apply (e.g., API Surface for a CLI tool, or Deployment Strategy for a library).
- **Diagrams:** Prefer Mermaid syntax for architecture diagrams so they render in markdown viewers.
- **Priorities:** Use Must-have / Should-have / Could-have / Won't-have (MoSCoW) for requirements unless the user prefers a different scheme.
