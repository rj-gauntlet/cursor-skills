# Presearch

Ingest a product requirements document (PRD) and collaboratively walk through the requirements to develop a high-level architecture, strategy, cost analysis, implementation plan, and phased schedule.

## What It Does

1. **Ingests** your PRD (any format — markdown, text, PDF, inline)
2. **Walks through** each requirement interactively, presenting options with pros/cons and recommendations
3. **Develops** system architecture, tech stack, and strategy collaboratively
4. **Builds** a phased milestone schedule based on your timeline
5. **Analyzes costs** — development effort, operational costs at scale, and alternative cost comparisons
6. **Produces** a detailed `PROJECT_PLAN.md` saved to your project

## Install

Copy the `presearch` folder into your Cursor skills directory:

**PowerShell:**
```powershell
git clone https://github.com/YOUR_USERNAME/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\presearch" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

**Bash:**
```bash
git clone https://github.com/YOUR_USERNAME/cursor-skills.git /tmp/cursor-skills
cp -r /tmp/cursor-skills/presearch ~/.cursor/skills/
rm -rf /tmp/cursor-skills
```

**Or with degit (no full clone needed):**
```bash
npx degit YOUR_USERNAME/cursor-skills/presearch ~/.cursor/skills/presearch
```

## Usage

Open any project in Cursor and say something like:

- "Here's my PRD, help me plan this project"
- "Run presearch on requirements.md"
- "I want to plan the architecture based on this spec"

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Core workflow instructions (6 phases) |
| `plan-template.md` | Template for the output `PROJECT_PLAN.md` |
