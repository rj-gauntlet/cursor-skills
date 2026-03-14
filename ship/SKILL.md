---
name: ship
description: Deploy a tested project to production — set up CI/CD, configure hosting, deploy, and produce release notes. Use when the user wants to deploy, ship, go live, set up CI/CD, push to production, or create a release.
---

# Ship — Deploy to Production

Take a tested, reviewed project and get it running in production with CI/CD, hosting, and release documentation.

## Trigger

The user has a project that has passed review and test-qa (a `TEST_REPORT.md` exists with a passing verdict) and wants to deploy it. If `TEST_REPORT.md` doesn't exist or has critical failures, warn the user and recommend running test-qa first. Proceed if they insist.

## Workflow

### Step 1: Load Context

Read the project artifacts:
- `PROJECT_PLAN.md` — deployment strategy, hosting decisions, env vars
- `BUILD_MANIFEST.md` — dependencies, run commands, environment variables
- `TEST_REPORT.md` — confirm tests passed
- `REVIEW_REPORT.md` — confirm no critical issues

Extract the deployment strategy from the plan. If the plan doesn't specify one, proceed to Step 2.

### Step 2: Deployment Questionnaire

If the plan's deployment strategy is clear, confirm it with the user and skip ahead. Otherwise, ask:

1. **Where are you deploying?**
   Present options relevant to the tech stack (e.g., Vercel/Netlify for frontend, Railway/Fly.io/AWS for backend, etc.) with cost and complexity tradeoffs.

2. **Do you need CI/CD?**
   - Yes — set up automated pipeline (GitHub Actions, GitLab CI, etc.)
   - Not yet — manual deployment for now

3. **What's the domain situation?**
   - Custom domain ready
   - Will use provider's default domain for now
   - Need help choosing/purchasing a domain

4. **Environment variables** — confirm all production values are ready for the required env vars.

### Step 3: CI/CD Setup (if requested)

Create a CI/CD pipeline configuration:

1. **Choose the platform** based on where the code is hosted (GitHub Actions for GitHub, GitLab CI for GitLab, etc.)
2. **Pipeline stages:**
   - Install dependencies
   - Run linter / type checker
   - Run tests
   - Build the project
   - Deploy (to staging first if applicable, then production)
3. **Write the config file** (e.g., `.github/workflows/deploy.yml`)
4. **Add deployment secrets** — list the secrets that need to be configured in the CI/CD platform's settings. Don't write actual secret values to files.

### Step 4: Hosting Configuration

Set up the deployment target:

1. **Create hosting config files** required by the platform (e.g., `vercel.json`, `fly.toml`, `Dockerfile`, `docker-compose.yml`, `Procfile`)
2. **Configure build settings** — build command, output directory, start command
3. **Environment variables** — document which vars need to be set in the hosting platform's dashboard
4. **Database provisioning** — if the plan includes a database, document the setup steps for the production database (connection string, migrations, seed data)

### Step 5: Pre-Deploy Checklist

Run through a verification checklist before deploying:

- [ ] All tests passing
- [ ] No hardcoded development URLs or localhost references in production code
- [ ] Environment variables documented and ready to configure
- [ ] `.env` is in `.gitignore`
- [ ] No secrets in the codebase
- [ ] Build completes without errors
- [ ] Database migrations ready (if applicable)
- [ ] CORS configured for production domain (if applicable)
- [ ] HTTPS enforced (if applicable)

Present the checklist results. If any items fail, flag them and help fix before proceeding.

### Step 6: Deploy

Guide the user through deployment:

1. **If CI/CD is set up:** Push to the deployment branch and monitor the pipeline.
2. **If manual:** Walk through the deployment commands step by step.
3. **Verify the deployment** — check that the production URL responds correctly.
4. **Run a quick smoke test** on the live deployment — does the main flow work?

If deployment fails, diagnose the error and help fix it.

### Step 7: Post-Deploy

1. **Regenerate `DASHBOARD.html`** — launch the dashboard skill as a non-blocking subagent to show the shipped state with production URL live. Don't wait for it to complete.
2. **Generate release notes** — `RELEASE.md` summarizing what was shipped:

```markdown
# Release — [version or date]

## What's New
- [Feature 1 — brief description]
- [Feature 2 — brief description]

## Technical Details
- **Stack:** [tech stack summary]
- **Hosting:** [where it's deployed]
- **CI/CD:** [pipeline summary]

## Known Limitations
- [Anything not yet built, or known issues]

## Links
- **Production:** [URL]
- **Repository:** [URL]
- **Plan:** PROJECT_PLAN.md
```

3. **Update `PROJECT_PLAN.md`** — mark the overall status as shipped with the deployment date.
4. **Suggest monitoring** — recommend logging, error tracking, and uptime monitoring tools appropriate for the stack and stakes level.

## Interaction Guidelines

- **Never deploy without confirmation.** Always pause before the actual deploy step, even in quick mode. Deploying to production is irreversible enough to warrant explicit approval.
- **Secrets stay out of files.** Never write API keys, passwords, or tokens to config files. Always instruct the user to set them in the hosting platform's environment settings.
- **Prefer simple over clever.** If the project can deploy with a single command and a Dockerfile, don't set up a complex multi-stage pipeline.
- **Document everything.** A deployment that only the agent knows how to repeat is a bad deployment. Every step should be reproducible by the user.
- **Rollback plan.** If the hosting platform supports it, note how to roll back to a previous version if something goes wrong.
