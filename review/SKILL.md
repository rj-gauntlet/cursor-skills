---
name: review
description: Review implemented code against a PROJECT_PLAN.md to verify all requirements are met, architecture matches the plan, and code quality meets standards. Use when the user wants to review code against a plan, verify implementation completeness, check if the build matches requirements, or run a post-implementation review.
---

# Review — Verify Implementation Against Plan

Read the project plan, implementation log, and build manifest, then systematically verify that the code matches what was planned.

## Trigger

The user has a project with `PROJECT_PLAN.md`, `IMPLEMENTATION_LOG.md`, and `BUILD_MANIFEST.md` (typically after running the implement skill) and asks for a review.

## Workflow

### Step 1: Load Context

Read all three documents:
- `PROJECT_PLAN.md` — the source of truth for what should exist
- `IMPLEMENTATION_LOG.md` — what actually happened during implementation
- `BUILD_MANIFEST.md` — structured summary of what was built

If any are missing, warn the user but continue with what's available. `PROJECT_PLAN.md` is required — abort without it.

### Step 2: Requirements Verification

Walk through every requirement in the plan:

#### Functional Requirements
For each FR in the requirements table:
1. Locate the code that implements it (use the build manifest's file list as a starting point)
2. Read the relevant files
3. Assess: does the code actually fulfill the requirement?
4. Verdict: **Pass**, **Partial** (implemented but incomplete), or **Missing**

#### Non-Functional Requirements
For each NFR:
1. Check if the code structurally supports it (e.g., caching layers for performance, input sanitization for security)
2. Verdict: **Pass**, **Needs Testing** (structurally sound but needs runtime verification by test-qa), or **Missing**

### Step 3: Architecture Verification

Compare the implemented architecture against the plan:

1. **Tech stack** — are the planned technologies actually used? Any unauthorized substitutions beyond logged deviations?
2. **Component structure** — do the components from the architecture diagram exist? Do they have the responsibilities described?
3. **Data models** — do the implemented models match the planned schemas? Missing fields, extra fields, wrong types?
4. **API surface** — do the endpoints match the plan? Correct methods, paths, auth requirements?
5. **Shared interfaces** — are they in the locations specified by the plan? Are all dependent features importing from them (no duplicated types)?
6. **Project structure** — does the file/folder layout match the plan?

### Step 4: Deviation Assessment

Review every deviation logged in `IMPLEMENTATION_LOG.md`:
1. Was the deviation justified?
2. Does it create downstream problems for unbuilt features?
3. Should the plan be updated to reflect the new reality?

### Step 5: Code Quality Scan

Read through the implemented code and check for:
- Obvious bugs or logic errors
- Security issues (hardcoded secrets, SQL injection, XSS, unvalidated input)
- Error handling gaps (unhandled promise rejections, missing try/catch, swallowed errors)
- Dead code or unused imports
- Inconsistent patterns (e.g., some files use async/await, others use callbacks)
- Missing edge case handling flagged in the plan's Risks section

### Step 6: Success Criteria Check

For each phase's success criteria in the plan:
1. Can the criteria be verified by reading code? If so, verify it.
2. Does it require running the code? Mark as **Needs Testing** for the test-qa skill.

### Step 7: Produce Review Report

Generate `REVIEW_REPORT.md` and save to the project root. Then launch the dashboard skill as a non-blocking subagent to regenerate `DASHBOARD.html` with the review results. Don't wait for it to complete.

```markdown
# Review Report

> Reviewed on [date] against PROJECT_PLAN.md

## Summary
- **Requirements:** [X/Y] pass, [N] partial, [M] missing
- **Architecture:** [pass/issues found]
- **Code quality:** [issues count by severity]
- **Overall verdict:** Ready for testing / Needs fixes before testing

## Requirements Status

### Functional Requirements
| ID | Requirement | Status | Notes |
|----|-------------|--------|-------|
| FR-01 | [requirement] | Pass / Partial / Missing | [details] |

### Non-Functional Requirements
| ID | Requirement | Status | Notes |
|----|-------------|--------|-------|
| NFR-01 | [requirement] | Pass / Needs Testing / Missing | [details] |

## Architecture Review
- **Tech stack:** [assessment]
- **Components:** [assessment]
- **Data models:** [assessment]
- **API surface:** [assessment]
- **Shared interfaces:** [assessment]

## Deviation Assessment
| Deviation | Justified | Impact | Action Needed |
|-----------|-----------|--------|---------------|
| [deviation] | Yes/No | None/Low/High | [what to do] |

## Code Quality Issues

### Critical (must fix before testing)
- [ ] [issue + file + line]

### Warnings (should fix)
- [ ] [issue + file + line]

### Suggestions (optional improvements)
- [ ] [issue + file + line]

## Success Criteria
| Phase | Criteria | Status |
|-------|----------|--------|
| [phase] | [criteria] | Verified / Needs Testing / Not Met |

## Recommended Actions
1. [Most important fix]
2. [Second fix]
3. [etc.]

## Next Step
[If all critical issues resolved:] Ready for test-qa.
[If critical issues exist:] Fix the items above, then re-run review.
```

### Step 8: Present Findings

Walk the user through the report interactively:
1. Start with the overall verdict
2. Highlight critical issues that block testing
3. Discuss any deviations that need decisions
4. Confirm next steps — fix issues, re-review, or proceed to test-qa

## Interaction Guidelines

- **Be specific.** Cite file names and line numbers when flagging issues.
- **Distinguish severity.** Not every issue is a blocker. Clearly separate critical/warning/suggestion.
- **Respect deviations.** If a deviation was user-approved during implementation, don't flag it as an issue unless it caused a downstream problem.
- **Don't rewrite code.** The review skill identifies issues. Fixes happen in a separate implement cycle or manually by the user.
- **Focus on the plan.** Review against what was planned, not against ideal code. A feature that works correctly but isn't in the plan is not a pass — it's scope creep.
