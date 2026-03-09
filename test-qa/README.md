# Test/QA

Run scaled testing against an implemented project — from basic smoke tests to full security and performance audits — based on the stakes level you choose.

## What It Does

1. **Asks your stakes level** — low, medium, or high
2. **Runs smoke tests** (all levels) — project starts, main flows work
3. **Runs integration tests** (medium+) — API endpoints, database, auth, third-party integrations
4. **Runs E2E tests** (medium+) — complete user journeys end to end
5. **Runs security tests** (high) — injection, auth bypass, data exposure, dependency vulnerabilities
6. **Runs performance tests** (high) — response times, load handling, query efficiency
7. **Produces** a `TEST_REPORT.md` with results and recommended fixes

## Stakes Levels

| Level | What gets tested |
|-------|-----------------|
| **Low** | Smoke tests only — does it start and work? |
| **Medium** | + Integration, E2E, edge cases |
| **High** | + Security audit, performance benchmarks |

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\test-qa" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/test-qa ~/.cursor/skills/test-qa
```

## Usage

Open a project that has been implemented and reviewed, then say:
- "Run QA on this project"
- "Test everything — high stakes"
- "Quick smoke test, this is just a prototype"

## Part of the Pipeline

```
Presearch → Implement → Review → [Test/QA] → Ship
```
