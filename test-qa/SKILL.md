---
name: test-qa
description: Run scaled testing against an implemented project — smoke tests, integration, end-to-end, visual UI verification, performance, and security — based on the stakes level. Use when the user wants to test a project, run QA, verify the build works, check the UI, check performance, run security tests, or validate after a review.
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

### Step 3: Detect Project Type

Determine if the project has a web UI:
- Check for frontend frameworks in dependencies (`react`, `vue`, `svelte`, `next`, `nuxt`, `angular`, etc.)
- Check for HTML templates, CSS files, or static assets
- If a web UI is detected, enable **visual UI testing** in Steps 5, 7, and the dedicated Step 8

This flag controls whether browser automation is used throughout the remaining steps.

### Step 4: Smoke Tests (all stakes levels)

Verify the basics work:
1. **Project starts** without errors
2. **Main entry points respond** — homepage loads, API root returns expected response, CLI runs without crashing
3. **Core happy path** — the single most important user flow works end to end (e.g., sign up → log in → perform main action)

**For web UI projects:** Use browser automation (browser-use subagent) for smoke tests:
- Open the app in a browser
- Take a screenshot of the homepage
- Check the browser console for errors (JS exceptions, failed network requests, 404s)
- Verify the page is not blank and key elements are visible
- If console errors exist at startup, report them immediately

If smoke tests fail, stop here. Report failures (with screenshots for UI projects) and recommend fixing before deeper testing.

### Step 5: Integration Tests (medium + high)

Test that components work together correctly:
1. **API endpoints** — call each endpoint from the plan's API Surface table. Verify correct responses, status codes, and error handling for invalid input.
2. **Database operations** — create, read, update, delete operations work. Data persists correctly. Relationships are maintained.
3. **Authentication flows** — login, logout, token refresh, protected routes reject unauthenticated requests.
4. **Third-party integrations** — external APIs connect and respond (or are properly mocked if keys aren't available).
5. **Shared interface contracts** — components consuming shared types behave correctly when given valid and invalid data.

Write test files for any integration tests that don't already exist. Use the test framework specified in the plan.

### Step 6: End-to-End Tests (medium + high)

Test complete user workflows:
1. Identify the key user journeys from the plan's functional requirements.
2. For each journey, simulate the full flow — from entry point to completion.
3. Verify the outcome matches the expected behavior.
4. Test with realistic data, not just "test123" inputs.

**For web UI projects:** Use browser automation for all E2E tests:
- Navigate each user journey in the browser (click links, fill forms, submit, verify results)
- Monitor the browser console throughout — capture every warning, error, and failed network request
- Take a screenshot at each major step in the journey
- Take a screenshot immediately on any failure or console error
- Verify visual outcomes (correct page loaded, success messages appear, data displays correctly)

For APIs, chain requests to simulate real usage patterns. For CLI tools, run full command sequences.

### Step 7: Edge Cases & Error Handling (medium + high)

Test what happens when things go wrong:
- Empty inputs, null values, missing required fields
- Extremely long strings, special characters, unicode
- Duplicate submissions (e.g., double-clicking a submit button)
- Expired or invalid auth tokens
- Network timeouts (if testable)
- Concurrent requests to the same resource

### Step 8: Visual UI Testing (medium + high, web UI projects only)

Skip this step for non-UI projects (APIs, CLIs, libraries).

**Console audit:**
1. Collect all browser console output captured during Steps 4-7
2. Categorize: errors (red), warnings (yellow), info
3. For each error — note which page/action triggered it

**Responsive testing (medium + high):**
Test the app at three viewport sizes:
- Mobile: 375x812
- Tablet: 768x1024
- Desktop: 1440x900

At each viewport:
1. Navigate to every key page
2. Take a screenshot
3. Check for layout breakage — overlapping elements, horizontal scroll, cut-off text, unreachable buttons
4. Verify navigation is usable (hamburger menu works on mobile, etc.)

**Accessibility basics (high only):**
1. Check for missing alt text on images
2. Check for missing form labels
3. Check color contrast on key text elements
4. Verify the page is navigable with keyboard (tab through interactive elements)
5. Check that focus states are visible

**Screenshot evidence:**
Save all screenshots to a `test-screenshots/` directory in the project, named descriptively:
- `smoke-homepage-desktop.png`
- `e2e-login-flow-step3-error.png`
- `responsive-dashboard-mobile.png`

Reference these in the test report.

### Step 9: Security Testing (high only)

Check for common vulnerabilities:
1. **Input validation** — attempt SQL injection, XSS, command injection on all user inputs
2. **Authentication** — test for session fixation, token leakage, brute force vulnerability
3. **Authorization** — can a regular user access admin endpoints? Can user A access user B's data?
4. **Data exposure** — do API responses leak sensitive fields (passwords, tokens, internal IDs)?
5. **Secrets** — scan code for hardcoded API keys, passwords, or connection strings
6. **Dependencies** — check for known vulnerabilities in installed packages (npm audit, pip audit, etc.)
7. **Headers** — verify security headers are set (CORS, CSP, HSTS) for web applications

### Step 10: Performance Testing (high only)

Verify the plan's performance targets:
1. **Response times** — measure API response times against NFR targets
2. **Load handling** — if the plan specifies concurrent user targets, simulate load and measure degradation
3. **Database queries** — check for N+1 queries, missing indexes, slow queries
4. **Bundle size** — for frontend apps, verify bundle size is reasonable
5. **Memory** — check for obvious memory leaks in long-running processes

Use the non-functional requirements from `BUILD_MANIFEST.md` as the benchmark. If no specific targets exist, use reasonable defaults and report the numbers.

### Step 11: Produce Test Report

Generate `TEST_REPORT.md` and save to the project root. Then regenerate `DASHBOARD.html` by running the dashboard skill silently to reflect the test results.

```markdown
# Test Report

> Tested on [date] | Stakes level: [Low/Medium/High]

## Summary
- **Smoke tests:** [pass/fail]
- **Integration tests:** [X/Y pass] (medium+high only)
- **E2E tests:** [X/Y pass] (medium+high only)
- **Visual UI:** [pass/issues found] (medium+high, web UI only)
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

## Visual UI (medium+high, web UI only)

### Console Errors
| Page/Action | Error | Severity |
|-------------|-------|----------|
| [page] | [error message] | Error/Warning |

### Responsive Testing
| Page | Mobile | Tablet | Desktop | Issues |
|------|--------|--------|---------|--------|
| [page] | Pass/Fail | Pass/Fail | Pass/Fail | [details] |

### Accessibility (high only)
| Check | Status | Notes |
|-------|--------|-------|
| Alt text | Pass/Fail | [details] |
| Form labels | Pass/Fail | [details] |
| Color contrast | Pass/Fail | [details] |
| Keyboard navigation | Pass/Fail | [details] |

### Screenshots
All screenshots saved to `test-screenshots/`. See [filename] for [description].

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

### Step 12: Present Findings

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
