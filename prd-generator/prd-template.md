# PRD Template

Use this template when generating the final `PRD.md`. Replace all bracketed placeholders with specifics from the conversation. Remove sections that don't apply. The structured tables are designed to be directly parseable by the presearch skill.

---

```markdown
# [Product Name] — Product Requirements Document

> Generated on [date]

## 1. Product Overview

### Vision
[One-paragraph summary of what the product is, the problem it solves, and why it matters]

### Problem Statement
[Clear description of the problem this product addresses — who has it, how they experience it, what the cost of not solving it is]

### Target Audience
[Who this is for — be specific, not "everyone"]

### Value Proposition
[What makes this product uniquely valuable compared to alternatives]

---

## 2. Competitive Landscape

| Product | Approach | Strengths | Weaknesses |
|---------|----------|-----------|------------|
| [Competitor 1] | [how they solve the problem] | [what they do well] | [gaps or limitations] |
| [Competitor 2] | [how they solve the problem] | [what they do well] | [gaps or limitations] |
| [Competitor 3] | [how they solve the problem] | [what they do well] | [gaps or limitations] |

### Opportunity
[What gap in the market this product fills — what it does that competitors don't or can't]

---

## 3. Users

| User Type | Description | Primary Goal | Key Pain Points |
|-----------|-------------|-------------|-----------------|
| [Primary user] | [who they are, context] | [what they want to accomplish] | [frustrations with current solutions] |
| [Secondary user] | [who they are, context] | [what they want to accomplish] | [frustrations with current solutions] |

### User Journeys

#### [Primary User] — [Key Journey Name]
1. [Discovery — how they find the product]
2. [Onboarding — first experience]
3. [Core action — the main thing they do]
4. [Outcome — what success looks like]

---

## 4. Functional Requirements

| ID | Domain | Requirement | Priority | User Type | Source |
|----|--------|-------------|----------|-----------|--------|
| FR-01 | [domain] | [requirement description] | Must-have | [user type] | User |
| FR-02 | [domain] | [requirement description] | Must-have | [user type] | User |
| FR-03 | [domain] | [requirement description] | Should-have | [user type] | Suggested |
| FR-04 | [domain] | [requirement description] | Could-have | [user type] | Suggested |

**Source column:** "User" = explicitly requested. "Suggested" = agent-recommended based on research or product type norms.

### Priority Summary
- **Must-have:** [count] requirements
- **Should-have:** [count] requirements
- **Could-have:** [count] requirements
- **Won't-have:** [count] requirements (deferred)

### Won't-Have (Explicitly Deferred)

| ID | Requirement | Reason for Deferral |
|----|-------------|-------------------|
| WH-01 | [feature] | [why it's out of scope for now] |

---

## 5. Non-Functional Requirements

| ID | Category | Requirement | Target | Priority |
|----|----------|-------------|--------|----------|
| NFR-01 | Performance | [requirement] | [specific metric] | Must-have |
| NFR-02 | Security | [requirement] | [standard or level] | Must-have |
| NFR-03 | Scalability | [requirement] | [metric] | Should-have |
| NFR-04 | Accessibility | [requirement] | [standard] | Should-have |

---

## 6. Platform & Constraints

### Platforms
| Platform | Required | Notes |
|----------|----------|-------|
| Web | Yes/No | [browser support, responsive requirements] |
| iOS | Yes/No | [native or PWA] |
| Android | Yes/No | [native or PWA] |
| Desktop | Yes/No | [Electron, native, or web-based] |
| API | Yes/No | [public or internal] |

### Constraints
- [Hard technical constraints — required tech, existing systems to integrate with]
- [Business constraints — budget, timeline, team size]
- [Regulatory constraints — compliance requirements]

---

## 7. Success Metrics

| Metric | Target | How to Measure |
|--------|--------|---------------|
| [metric name] | [specific target] | [measurement method] |
| [metric name] | [specific target] | [measurement method] |

---

## 8. Key Decisions

Decisions made during the PRD creation process:

| Decision | Choice | Reasoning |
|----------|--------|-----------|
| [decision area] | [what was chosen] | [why] |

---

## 9. Open Questions

- [Any unresolved questions that need answers before or during planning]

---

## 10. Next Steps

1. Run presearch on this PRD to generate a `PROJECT_PLAN.md`
2. [Any other immediate next steps]
```

---

## Template Usage Notes

- **Structured tables are critical.** The presearch skill parses the Functional Requirements and Non-Functional Requirements tables directly. Use the exact column format shown above.
- **Source column matters.** Distinguishing "User" from "Suggested" requirements helps the user quickly see what was their idea vs. what the agent recommended.
- **Priorities use MoSCoW.** Must-have / Should-have / Could-have / Won't-have — consistent with the presearch skill's expectation.
- **Be specific in targets.** "Fast" is not a target. "Page load under 2 seconds" is.
- **Won't-have is not delete.** Deferred features are documented with a reason, not removed. They may come back in a future version.
