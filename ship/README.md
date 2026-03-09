# Ship

Deploy a tested project to production — CI/CD setup, hosting configuration, pre-deploy checks, and release notes.

## What It Does

1. **Reads** the project plan, build manifest, and test report
2. **Configures** CI/CD pipeline (GitHub Actions, GitLab CI, etc.)
3. **Sets up** hosting (platform-specific config files)
4. **Runs** a pre-deploy checklist (secrets, env vars, build, security headers)
5. **Deploys** with step-by-step guidance
6. **Produces** a `RELEASE.md` with what was shipped and production links

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\ship" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/ship ~/.cursor/skills/ship
```

## Usage

Open a project that has passed testing and say:
- "Ship this to production"
- "Deploy the project"
- "Set up CI/CD and deploy"

## Part of the Pipeline

```
Presearch → Implement → Review → Test/QA → [Ship]
```
