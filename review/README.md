# Review

Verify implemented code against a `PROJECT_PLAN.md` — requirements coverage, architecture match, code quality, and deviation assessment.

## What It Does

1. **Checks every requirement** in the plan against the actual code
2. **Verifies architecture** — tech stack, components, data models, API surface, shared interfaces
3. **Assesses deviations** logged during implementation
4. **Scans for code quality** issues — bugs, security, error handling, dead code
5. **Produces** a `REVIEW_REPORT.md` with pass/fail per requirement and prioritized fix list

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\review" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/review ~/.cursor/skills/review
```

## Usage

Open a project that has been implemented and say:
- "Review the code against the plan"
- "Check if the implementation matches the requirements"
- "Run a post-implementation review"

## Part of the Pipeline

```
Presearch → Implement → [Review] → Test/QA → Ship
```
