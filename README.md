# Cursor Skills

Personal collection of [Cursor](https://cursor.sh) agent skills.

## Available Skills

| Skill | Description |
|-------|-------------|
| [presearch](presearch/) | Ingest a PRD and collaboratively build architecture, strategy, implementation plan, cost analysis, and phased schedule. |

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

Replace `rj-gauntlet` with the GitHub username and `SKILL_NAME` with the skill folder name.

## Adding New Skills

Create a new folder with a `SKILL.md` file:

```
skill-name/
├── SKILL.md          # Required
├── README.md         # Install instructions for sharing
└── [other files]     # Templates, scripts, references
```
