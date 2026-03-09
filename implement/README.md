# Implement

Take a `PROJECT_PLAN.md` and systematically turn it into working code, phase by phase, with tests, quality checks, and progress tracking.

## What It Does

1. **Reads** your project plan and sets up the environment
2. **Scaffolds** the project structure, dependencies, and configs
3. **Builds** each phase sequentially — writing tests alongside every feature
4. **Checks** linting, type checking, and test results after each phase
5. **Tracks** progress by checking off deliverables in the plan
6. **Produces** an `IMPLEMENTATION_LOG.md` and `BUILD_MANIFEST.md` for downstream skills

## Modes

- **Standard** — phase-gated, pauses for approval between phases
- **Quick** — builds everything autonomously, summarizes at the end

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\implement" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/implement ~/.cursor/skills/implement
```

## Usage

Open a project that has a `PROJECT_PLAN.md` and say:
- "Implement the project plan"
- "Build this project from the plan"
- "Quick mode — just build everything"

## Part of the Pipeline

```
Presearch → [Implement] → Review → Test/QA → Ship
```
