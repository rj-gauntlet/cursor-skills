# Dashboard

Generate a live interactive HTML dashboard from pipeline artifacts showing project progress, phases, tasks, requirements, and app status.

## What It Does

1. **Parses** your pipeline artifacts (`PROJECT_PLAN.md`, `IMPLEMENTATION_LOG.md`, `BUILD_MANIFEST.md`, etc.)
2. **Generates** a self-contained `DASHBOARD.html` with a dark-themed tabbed interface
3. **Shows live status** — JavaScript pings your local and production URLs to show real-time up/down indicators
4. **Includes a start command button** — copies the dev server start command to your clipboard

## Dashboard Sections

- **Pipeline tracker** — visual progress through Presearch → Implement → Review → Test/QA → Ship
- **Phases tab** — task lists with progress bars, status chips, deviation flags
- **Requirements tab** — MoSCoW-grouped requirements with completion tracking
- **Artifacts tab** — which pipeline documents exist and when they were last updated
- **Activity tab** — reverse-chronological feed of project events

## Auto-Updates

Each pipeline skill (implement, review, test-qa, ship) regenerates the dashboard at every checkpoint. You just refresh the browser tab.

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\dashboard" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/dashboard ~/.cursor/skills/dashboard
```

## Usage

Open any project with pipeline artifacts and say:
- "Generate a project dashboard"
- "Show me the project status"
- "Update the dashboard"

Or it runs automatically when other pipeline skills reach checkpoints.
