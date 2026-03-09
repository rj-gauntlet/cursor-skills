---
name: test-qa
description: Run scaled testing against an implemented project — smoke tests, integration, end-to-end, performance, and security — based on the stakes level. Use when the user wants to test a project, run QA, verify the build works, check performance, run security tests, or validate after a review.
---

# Test/QA — Scaled Testing

Run the project, verify it works, and test it at a depth that matches the stakes.

## Trigger

The user has implemented code (typically after the implement and review skills) and wants to verify it works correctly at runtime. A `REVIEW_REPORT.md` should exist confirming no critical issues block testing. If it doesn't exist, warn the user and recommend running review first, but proceed if they insist.

## Workflow

### Step 1: Assess Stakes

Ask the user one question:

> What are the stakes for this project?
> - **Low** — personal project or prototype (basic smoke tests)
> - **Medium** — users will depend on this (integration + E2E + basic security)
> - **High** — production, paid, or regulated (full suite including performance and security)

This determines the depth of every subsequent step.

### Step 2: Load Context

Read the project artifacts:
- `PROJECT_PLAN.md` — requirements, success criteria, non-functional requirements
- `BUILD_MANIFEST.md` — how to run/test, dependencies, env vars, NFR targets
- `REVIEW_REPORT.md` — items marked "Needs Testing", known issues

Verify the project can run:
1. Check that dependencies are installed. If not, install them.
2. Check that required env vars exist in `.env`. Warn about missing ones.
3. Attempt to start the project. If it fails to start, stop and report the error.

### Step 3: Smoke Tests (all stakes levels)

Verify the basics work:
1. **Project starts** without errors
2. **Main entry points respond** — homepage loads, API root returns expected response, CLI runs without crashing
3. **Core happy path** — the single most important user flow works end to end (e.g., sign up → log in → perform main action)

If smoke tests fail, stop here. Report failures and recommend fixing before deeper testing.

### Step 4: Integration Tests (medium + high)

Test that components work together correctly:
1. **API endpoints** — call each endpoint from the plan's API Surface table. Verify correct responses, status codes, and error handling for invalid input.
2. **Database operations** — create, read, update, delete operations work. Data persists correctly. Relationships are maintained.
3. **Authentication flows** — login, logout, token refresh, protected routes reject unauthenticated requests.
4. **Third-party integrations** — external APIs connect and respond (or are properly mocked if keys aren't available).
5. **Shared interface contracts** — components consuming shared types behave correctly when given valid and invalid data.

Write test files for any integration tests that don't already exist. Use the test framework specified in the plan.

### Step 5: End-to-End Tests (medium + high)

Test complete user workflows:
1. Identify the key user journeys from the plan's functional requirements.
2. For each journey, simulate the full flow — from entry point to completion.
3. Verify the outcome matches the expected behavior.
4. Test with realistic data, not just "test123" inputs.

For web applications, use browser automation if available. For APIs, chain requests to simulate real usage patterns. For CLI tools, run full command sequences.

### Step 6: Edge Cases & Error Handling (medium + high)

Test what happens when things go wrong:
- Empty inputs, null values, missing required fields
- Extremely long strings, special characters, unicode
- Duplicate submissions (e.g., double-clicking a submit button)
- Expired or invalid auth tokens
- Network timeouts (if testable)
- Concurrent requests to the same resource

### Step 7: Security Testing (high only)

Check for common vulnerabilities:
1. **Input validation** — attempt SQL injection, XSS, command injection on all user inputs
2. **Authentication** — test for session fixation, token leakage, brute force vulnerability
3. **Authorization** — can a regular user access admin endpoints? Can user A access user B's data?
4. **Data exposure** — do API responses leak sensitive fields (passwords, tokens, internal IDs)?
5. **Secrets** — scan code for hardcoded API keys, passwords, or connection strings
6. **Dependencies** — check for known vulnerabilities in installed packages (npm audit, pip audit, etc.)
7. **Headers** — verify security headers are set (CORS, CSP, HSTS) for web applications

### Step 8: Performance Testing (high only)

Verify the plan's performance targets:
1. **Response times** — measure API response times against NFR targets
2. **Load handling** — if the plan specifies concurrent user targets, simulate load and measure degradation
3. **Database queries** — check for N+1 queries, missing indexes, slow queries
4. **Bundle size** — for frontend apps, verify bundle size is reasonable
5. **Memory** — check for obvious memory leaks in long-running processes

Use the non-functional requirements from `BUILD_MANIFEST.md` as the benchmark. If no specific targets exist, use reasonable defaults and report the numbers.

### Step 9: Produce Test Report

Generate `TEST_REPORT.md` and save to the project root.

```markdown
# Test Report

> Tested on [date] | Stakes level: [Low/Medium/High]

## Summary
- **Smoke tests:** [pass/fail]
- **Integration tests:** [X/Y pass] (medium+high only)
- **E2E tests:** [X/Y pass] (medium+high only)
- **Security:** [issues found / clean] (high only)
- **Performance:** [meets targets / issues] (high only)
- **Overall verdict:** Ready to ship / Needs fixes

## Smoke Tests
| Test | Status | Notes |
|------|--------|-------|
| Project starts | Pass/Fail | [details] |
| Main entry responds | Pass/Fail | [details] |
| Core happy path | Pass/Fail | [details] |

## Integration Tests
| Area | Tests | Passed | Failed | Notes |
|------|-------|--------|--------|-------|
| API endpoints | [N] | [N] | [N] | [details] |
| Database | [N] | [N] | [N] | [details] |
| Auth | [N] | [N] | [N] | [details] |

## E2E Tests
| User Journey | Status | Notes |
|-------------|--------|-------|
| [journey] | Pass/Fail | [details] |

## Edge Cases & Error Handling
| Test | Status | Notes |
|------|--------|-------|
| [edge case] | Pass/Fail | [details] |

## Security (high only)
| Check | Status | Severity | Notes |
|-------|--------|----------|-------|
| SQL injection | Pass/Fail | Critical/Warning | [details] |
| XSS | Pass/Fail | Critical/Warning | [details] |
| Auth bypass | Pass/Fail | Critical/Warning | [details] |

## Performance (high only)
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| [metric] | [target] | [measured] | Pass/Fail |

## Non-Functional Requirements Status
| ID | Requirement | Target | Result | Status |
|----|-------------|--------|--------|--------|
| NFR-01 | [requirement] | [target] | [actual] | Met / Not Met |

## Failed Tests — Details
[For each failed test, provide: what failed, expected vs. actual, reproduction steps, suggested fix]

## Recommended Actions
1. [Most critical fix]
2. [Second fix]
3. [etc.]

## Next Step
[If all critical tests pass:] Ready for ship.
[If failures exist:] Fix the items above, then re-run test-qa.
```

### Step 10: Present Findings

Walk the user through the results:
1. Overall verdict — pass or needs work
2. Any critical failures with details
3. Security concerns (if high stakes)
4. Performance numbers vs. targets (if high stakes)
5. Recommended next steps — fix and re-test, or proceed to ship

## Interaction Guidelines

- **Run the code, don't just read it.** The review skill checks code by reading. This skill checks by executing.
- **Be honest about limitations.** If you can't fully test something (e.g., no API keys for a third-party service), say so. Don't report a pass you can't verify.
- **Scale to stakes.** Low-stakes projects get quick smoke tests, not a full security audit. Respect the user's time.
- **Write lasting tests.** Integration and E2E tests written during this phase should be saved to the test directory so they can be re-run later, not thrown away.
- **Don't fix, just report.** Like the review skill, test-qa identifies problems. Fixes go through a separate implement cycle.
- **Use real-ish data.** Test with realistic inputs, not just "foo" and "bar". Edge cases should include things real users might actually do.
