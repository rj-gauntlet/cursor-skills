# PRD Generator

Turn a rough idea into a structured product requirements document through guided conversation with market research.

## What It Does

1. **Captures** your idea in whatever form — a sentence, bullet points, a competitor reference, a problem statement
2. **Researches** the competitive landscape — existing solutions, gaps, common features
3. **Shapes** the vision through structured questions — problem, users, scope, constraints
4. **Maps features** — proposes a full feature set, proactively suggests features you may not have thought of, prioritizes with MoSCoW
5. **Defines** non-functional requirements — performance, security, scalability, accessibility
6. **Produces** a structured `PRD.md` ready to feed directly into the presearch skill

## Pipeline Position

```
[PRD Generator] → Presearch → Implement → Review → Test/QA → Ship
     Idea            Plan        Code      Verified              Live
```

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\prd-generator" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/prd-generator ~/.cursor/skills/prd-generator
```

## Usage

Open any project and say:
- "I have an idea for a budgeting app"
- "I want to build something like Notion but for recipes"
- "Help me write a PRD for a plant watering reminder app"
- "Generate requirements for a project management dashboard"
