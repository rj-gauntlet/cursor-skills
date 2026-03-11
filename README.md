# Cursor Skills

Personal collection of [Cursor](https://cursor.sh) agent skills — a full pipeline from product requirements to production deployment, plus standalone tools for UI design.

## Pipeline

```
PRD Generator → Presearch → Implement → Review → Test/QA → Ship
    Idea           PRD        Plan       Code     Verified    Live
```

## Available Skills

### Pipeline Skills

| Skill | Description |
|-------|-------------|
| [prd-generator](prd-generator/) | Turn a rough idea into a structured PRD through guided conversation with market research and proactive feature suggestions. |
| [presearch](presearch/) | Ingest a PRD and collaboratively build architecture, strategy, implementation plan, cost analysis, and phased schedule. |
| [implement](implement/) | Take a PROJECT_PLAN.md and systematically build working code, phase by phase, with tests and progress tracking. |
| [review](review/) | Verify implemented code against the plan — requirements coverage, architecture, code quality. |
| [test-qa](test-qa/) | Run scaled testing (smoke, integration, E2E, visual UI, security, performance) based on project stakes. |
| [ship](ship/) | Deploy to production — CI/CD setup, hosting config, pre-deploy checks, and release notes. |

### Standalone Skills

| Skill | Description |
|-------|-------------|
| [dashboard](dashboard/) | Generate a live interactive HTML dashboard from pipeline artifacts — progress, phases, tasks, requirements, app status. Auto-called by pipeline skills. |
| [stunner](stunner/) | Transform a working app into a visually stunning product. Mockup-driven — shows you visual designs for approval before writing any code. |

## Pipeline Artifacts

Each skill produces artifacts that feed into the next:

| Artifact | Produced by | Consumed by |
|----------|-------------|-------------|
| `PRD.md` | prd-generator | presearch |
| `PROJECT_PLAN.md` | presearch | implement, review, test-qa |
| `IMPLEMENTATION_LOG.md` | implement | review |
| `BUILD_MANIFEST.md` | implement | review, test-qa, ship |
| `REVIEW_REPORT.md` | review | test-qa, ship |
| `TEST_REPORT.md` | test-qa | ship |
| `RELEASE.md` | ship | — |
| `DASHBOARD.html` | dashboard | — (opened in browser) |
| `DESIGN_SYSTEM.md` | stunner | — |
| `UI_CHANGELOG.md` | stunner | — |

## Installing a Skill

Each skill folder contains its own install instructions. To add any skill to your Cursor setup:

1. Navigate to the skill folder above
2. Follow the instructions in its README

Or manually:

```bash
# Clone the repo and copy just the skill you want
git clone https://github.com/rj-gauntlet/cursor-skills.git /tmp/cursor-skills
cp -r /tmp/cursor-skills/SKILL_NAME ~/.cursor/skills/
rm -rf /tmp/cursor-skills
```

```powershell
# PowerShell equivalent
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\SKILL_NAME" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

Replace `SKILL_NAME` with the skill folder name (e.g., `presearch`, `implement`, `review`, `test-qa`, `ship`). Install them individually or grab the whole repo for the full pipeline.

## Adding New Skills

Create a new folder with a `SKILL.md` file:

```
skill-name/
├── SKILL.md          # Required
├── README.md         # Install instructions for sharing
└── [other files]     # Templates, scripts, references
```
