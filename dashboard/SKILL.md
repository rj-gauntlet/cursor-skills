---
name: dashboard
description: Generate a live interactive HTML dashboard from pipeline artifacts (PROJECT_PLAN.md, IMPLEMENTATION_LOG.md, BUILD_MANIFEST.md, etc.) showing project progress, phases, tasks, requirements, and app status. Use when the user wants a project dashboard, progress tracker, status page, or when another pipeline skill needs to regenerate the dashboard.
---

# Dashboard — Project Progress Tracker

Parse pipeline artifacts and generate a self-contained, interactive `DASHBOARD.html` showing the full project state.

## Trigger

- The user asks for a project dashboard or progress tracker
- Another pipeline skill calls this after a checkpoint (implement, review, test-qa, ship)
- The user says "update the dashboard" or "regenerate the dashboard"

## Data Sources

Read and parse these artifacts from the project root. All are optional — the dashboard renders whatever is available and shows "Not created" for missing artifacts.

| Artifact | Data extracted |
|----------|---------------|
| `PROJECT_PLAN.md` | Project name, phases, tasks, deliverables, requirements (MoSCoW), tech stack, timeline, success criteria |
| `IMPLEMENTATION_LOG.md` | Phase completion dates, deviations, actual vs. estimated time |
| `BUILD_MANIFEST.md` | Start/test commands, dependencies, env vars, local URL, production URL |
| `REVIEW_REPORT.md` | Requirements pass/fail status, code quality issues, overall verdict |
| `TEST_REPORT.md` | Test results per category, overall verdict |
| `RELEASE.md` | Deployment status, production URL |

## Parsing Rules

### Project name and description
Extract from `PROJECT_PLAN.md` heading and Product Overview section.

### Phases and tasks
Parse the Implementation Plan section of `PROJECT_PLAN.md`:
- Each `### Phase N:` heading becomes a phase
- Deliverable checkboxes (`- [x]` / `- [ ]`) become tasks with done/pending status
- Key Tasks numbered lists provide task names if deliverables are sparse

Cross-reference with `IMPLEMENTATION_LOG.md` for:
- Phase completion dates
- Deviations (mark affected tasks with a "deviation" chip)
- Actual duration vs. estimated

### Task status inference
- `- [x]` in the plan → **done**
- Mentioned as "in progress" in the implementation log → **in progress**
- Mentioned as "blocked" in the implementation log → **blocked**
- `- [ ]` in the plan with no log mention → **not started**

### Requirements
Parse the Requirements Summary section of `PROJECT_PLAN.md`:
- Group by Priority column (Must-have, Should-have, Could-have, Won't-have)
- Cross-reference with `REVIEW_REPORT.md` requirements status table for completion
- If no review report, infer completion from which phases are done (if a requirement maps to a completed phase's deliverables, mark it done)

### Pipeline stage
Determine the current pipeline position:
- `PROJECT_PLAN.md` exists → Presearch complete
- `IMPLEMENTATION_LOG.md` exists → Implementation in progress or complete
- All phases checked off → Implementation complete
- `REVIEW_REPORT.md` exists → Review complete
- `TEST_REPORT.md` exists → Test/QA complete
- `RELEASE.md` exists → Shipped

### App URLs and start command
Extract from `BUILD_MANIFEST.md`:
- `localUrl` from the "How to Run > Start" section (default: `http://localhost:3000`)
- `prodUrl` from the deployment info or `RELEASE.md` production link
- `startCommand` from the "How to Run > Start" section

### Activity feed
Build from `IMPLEMENTATION_LOG.md` entries:
- Phase completions
- Deviations
- Blocked tasks
- Combine with artifact creation dates for presearch/review/test-qa/ship events

### Overall completion percentage
Calculate as: (completed tasks across all phases) / (total tasks across all phases) × 100

## Dashboard Structure

Generate `DASHBOARD.html` using the **tabbed layout** with these sections:

### Header
- Project name and description
- Overall completion percentage (large, prominent)
- App status indicators (local + production) — **live, via JavaScript ping**
- Start command button — copies the start command to clipboard when clicked

### Pipeline + Stats Bar
- Pipeline visualization: Presearch → Implement → Review → Test/QA → Ship
- Completed stages: green with checkmark
- Current stage: amber with dot
- Future stages: gray
- Stats: completion %, tasks remaining, time elapsed, time remaining

### Tab: Phases
- Card per phase with progress bar
- Expandable task list with status chips (done, in progress, blocked, deviation)
- Phase timing: estimated vs. actual

### Tab: Requirements
- 2x2 grid of requirement groups (Must-have, Should-have, Could-have, Won't-have)
- Each group has a progress bar and item list with completion dots
- Color-coded by priority (red, amber, blue, gray)

### Tab: Artifacts
- Card grid showing each pipeline artifact
- Green dot + "Updated [time]" for existing artifacts
- Gray dot + "Not created" for missing ones
- Clicking an artifact could open it (via relative file link)

### Tab: Activity
- Reverse-chronological feed of project events
- Color-coded dots by event type (green=completion, amber=in progress, red=blocked, purple=deviation, blue=pipeline event)
- Badge on tab showing count of recent events

## Live Status Script

Include this JavaScript in the generated HTML for real-time app status:

```javascript
async function checkStatus(url, elementId) {
  const dot = document.getElementById(elementId + '-dot');
  const label = document.getElementById(elementId + '-label');
  try {
    await fetch(url, { mode: 'no-cors', cache: 'no-cache' });
    dot.className = 'status-dot up';
    label.textContent = 'Online';
  } catch {
    dot.className = 'status-dot down';
    label.textContent = 'Offline';
  }
}

function updateStatus() {
  checkStatus('LOCAL_URL_PLACEHOLDER', 'local');
  const prodUrl = 'PROD_URL_PLACEHOLDER';
  if (prodUrl !== 'none') checkStatus(prodUrl, 'prod');
}

updateStatus();
setInterval(updateStatus, 5000);
```

Replace `LOCAL_URL_PLACEHOLDER` and `PROD_URL_PLACEHOLDER` with actual values from `BUILD_MANIFEST.md`.

## Start Command Button

Include a clipboard copy button:

```javascript
function copyStartCommand() {
  navigator.clipboard.writeText('START_COMMAND_PLACEHOLDER');
  const btn = document.getElementById('launch-btn');
  btn.textContent = '✓ Copied';
  setTimeout(() => { btn.textContent = 'Copy Start Command'; }, 2000);
}
```

When the local app is detected as running, change the button to show "● Running" (disabled state) instead.

## Styling

Use the dark theme from the approved tabbed layout mockup:
- Background: `#0f1117`
- Cards: `#1a1d27`
- Borders: `#2a2d3a`
- Text: `#e8eaed` primary, `#9ca3af` secondary, `#6b7280` muted
- Status colors: green `#34d399`, amber `#fbbf24`, red `#f87171`, blue `#60a5fa`, purple `#a78bfa`
- Font: Inter (via Google Fonts CDN)
- Border radius: 12px cards, 8px inner elements
- Pulsing animation on status dots

The HTML must be fully self-contained — inline CSS, no external dependencies except the Google Fonts CDN link.

## Output

Save as `DASHBOARD.html` in the project root. If a previous version exists, overwrite it.

After generating, inform the user:
- Where the file was saved
- How to open it (`open DASHBOARD.html` or `start DASHBOARD.html`)
- That status indicators are live as long as the tab is open

## When Called by Other Skills

When another pipeline skill triggers dashboard regeneration, run silently:
1. Parse all available artifacts
2. Regenerate `DASHBOARD.html`
3. No user interaction needed — just update the file

The calling skill should mention "Dashboard updated" in its checkpoint summary.
