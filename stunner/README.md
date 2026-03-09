# Stunner

Transform a functionally complete project into a visually stunning application. Generates mockup images for approval before writing any code.

## What It Does

1. **Audits** your existing UI — screenshots every page, maps components, identifies the product type
2. **Presents 5-6 aesthetic directions** as visual mockups (bold & vibrant, minimal, dark & premium, warm & organic, plus 2-3 creative wildcards)
3. **Iterates** on the direction with you — combine ideas, adjust vibes, keep going until you love it
4. **Suggests brand vibe** — personality, app name styling, favicon concept, color naming, UI copy tone
5. **Builds a design system** — colors, typography, spacing, shadows, borders, animations, component states
6. **Mockups every page** for approval before any code changes
7. **Implements** the approved designs, comparing live results to mockups
8. **Polishes** — responsive refinement, micro-interactions, dark mode, consistency sweep, before/after comparison

## Key Principle

You see mockups before any code changes. Iterate as many times as you want until you're excited about what you see. Only then does implementation begin.

## Works With

Any frontend framework — React, Vue, Svelte, Next, Nuxt, Angular, Astro, plain HTML/CSS. Uses whatever styling approach the project already has (Tailwind, CSS Modules, styled-components, etc.).

## Install

```powershell
git clone https://github.com/rj-gauntlet/cursor-skills.git $env:TEMP\cursor-skills
Copy-Item -Recurse "$env:TEMP\cursor-skills\stunner" "$HOME\.cursor\skills\"
Remove-Item -Recurse -Force "$env:TEMP\cursor-skills"
```

```bash
npx degit rj-gauntlet/cursor-skills/stunner ~/.cursor/skills/stunner
```

## Usage

Open a project with a working UI and say:
- "Make this UI stunning"
- "The app works but looks terrible, help me redesign it"
- "Run stunner on this project"
- "I want a bold, vibrant redesign"
