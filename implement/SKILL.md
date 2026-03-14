---
name: implement
description: Take a PROJECT_PLAN.md and systematically turn it into working code, phase by phase, with tests, quality checks, and progress tracking. Use when the user wants to implement a project plan, build from a plan, start coding from a PROJECT_PLAN.md, or execute a presearch output.
---

# Implement — Plan to Code

Systematically build a project from a `PROJECT_PLAN.md`, phase by phase, with checkpoints for review.

## Modes

### Standard Mode (default)
Phase-gated — build one phase at a time, pause for user approval before moving to the next. The user can say "just keep going" at any checkpoint to let the remaining phases run without pausing.

### Quick Mode
Triggered when the user asks for speed (e.g., "just build it", "quick mode", "implement everything"). Runs all phases autonomously without pausing at checkpoints. Presents a full summary at the end with all decisions, deviations, and results.

## Trigger

The user has a `PROJECT_PLAN.md` in their project (typically produced by the presearch skill) and asks to implement it.

## Workflow

### Phase 0: Prepare

1. **Read `PROJECT_PLAN.md`** in full. Extract the phase list, tech stack, project structure, shared interfaces, dependencies, environment variables, and success criteria.
2. **Detect existing code.** If the project already has source files, note what exists to avoid overwriting.
3. **Verify environment.** Check for a `.env` file. If the plan lists required env vars:
   - Create `.env.example` with all required var names (no values) if it doesn't exist.
   - Compare `.env` against the plan's requirements. Warn the user about any missing vars. Don't proceed until critical vars are accounted for (the user can say "I'll add them later" to continue).
4. **Confirm mode.** If the user hasn't specified, ask: standard (phase-gated) or quick?
5. **Initialize git** if no `.git` directory exists. Create an initial commit with the plan and any existing files.

### Per-Phase Cycle

For each phase in the plan, run this cycle:

#### Step 1: Read

Pull the current phase from `PROJECT_PLAN.md`:
- Phase goal
- Deliverables (the checklist items)
- Key tasks
- Success criteria
- Risks

Present a brief summary of what's about to be built. In standard mode, confirm before proceeding.

#### Step 2: Scaffold (Phase 1 only)

If this is the first phase:
- Create the project directory structure from the plan
- Install all dependencies listed in the plan
- Set up configuration files (linter, formatter, tsconfig, etc.)
- Initialize the test framework specified in the tech stack

For existing projects, skip scaffolding that conflicts with what's already there.

#### Step 3: Implement

Work through the phase's tasks sequentially:
- **Respect dependency order.** If a task depends on a shared interface, check if it exists. If not, create it in the location specified by the plan's Shared Interfaces table, then proceed.
- **Write tests alongside each feature.** After implementing a feature, write tests for it immediately using the test framework from the plan.
- **Handle deviations (severity-based):**
  - *Minor* (different function signature, slightly different file organization, renamed variable) — make a judgment call, log the deviation, keep going.
  - *Major* (library doesn't work as expected, API shape fundamentally different, need to pull work from a later phase, security concern) — stop and ask the user in standard mode. In quick mode, make the best judgment call, log it prominently, and flag it in the final summary.

#### Step 4: Check

After all tasks in the phase are complete:
1. Run the linter / type checker
2. Run all tests (new tests from this phase + existing tests as regression)
3. Fix any failures. If a fix requires a significant change, treat it as a major deviation.

#### Step 5: Summarize

Present a phase completion summary:
- Deliverables completed (checked off)
- Files created and modified
- Dependencies added
- Tests written and their pass/fail status
- Deviations from the plan (with reasoning)
- Any risks that materialized

#### Step 6: Record

1. **Update `PROJECT_PLAN.md`** — check off completed deliverables:
   ```
   - [x] User authentication API    ← was [ ]
   - [x] JWT token generation        ← was [ ]
   - [ ] Password reset flow         ← next phase
   ```
2. **Append to `IMPLEMENTATION_LOG.md`** — log the phase results (see Output Artifacts below).
3. **Commit to git** with message: `feat: complete Phase N — [phase name]`
4. **Regenerate `DASHBOARD.html`** — launch the dashboard skill as a non-blocking subagent. Don't wait for it to complete — proceed to the checkpoint immediately.

#### Step 7: Checkpoint (standard mode only)

Pause and wait for user approval:
- "Looks good, continue" → proceed to next phase
- "Fix [issue]" → address it, re-run checks, update the log
- "Skip to phase N" → jump ahead
- "Stop here" → end the session

In quick mode, skip this step and proceed directly.

### Completion

After all phases are done:

1. **Update `BUILD_MANIFEST.md`** with the final project state (see Output Artifacts).
2. **Final git commit** if any remaining changes.
3. **Present a completion summary:**
   - All phases completed
   - Total files created/modified
   - Total tests and pass rate
   - All deviations from the plan
   - Suggested next step: "Run the review skill to verify against requirements"

## Output Artifacts

### `IMPLEMENTATION_LOG.md`

Appended after each phase. Captures what actually happened vs. what was planned.

```markdown
# Implementation Log

## Phase 1: [Phase Name] — [date]
- **Status:** Complete
- **Deliverables:** 4/4 complete
- **Deviations:**
  - Switched from `library-x` to `library-y` because [reason] (minor)
  - Pulled user validation from Phase 3 into Phase 1 because auth depended on it (major — user approved)
- **Notes:** [anything relevant for later phases]

## Phase 2: [Phase Name] — [date]
...
```

### `BUILD_MANIFEST.md`

Generated at completion. Structured handoff for the review skill.

```markdown
# Build Manifest

## Project Info
- **Plan:** PROJECT_PLAN.md
- **Phases completed:** [N/N]
- **Date:** [date]

## How to Run
- **Install:** `[install command]`
- **Start:** `[start command]`
- **Test:** `[test command]`

## Files Created
[List of all files created, grouped by feature/phase]

## Dependencies Installed
| Package | Version | Purpose |
|---------|---------|---------|
| [package] | [version] | [why] |

## Environment Variables Required
| Variable | Service | Required |
|----------|---------|----------|
| [var] | [what needs it] | yes/no |

## Success Criteria Status
| ID | Criteria | Status |
|----|----------|--------|
| SC-01 | [criteria from plan] | Met / Unverified / Not met |

## Non-Functional Requirements
| ID | Requirement | Target | Status |
|----|-------------|--------|--------|
| NFR-01 | [requirement] | [metric] | To be verified by test-qa |

## Deviations from Plan
[Summary of all deviations from IMPLEMENTATION_LOG.md]

## Known Gaps
[Anything explicitly not built, deferred, or incomplete]
```

## Interaction Guidelines

- **Follow the plan.** The `PROJECT_PLAN.md` is the source of truth. Deviate only when necessary, and always log why.
- **Don't gold-plate.** Build what the plan says. If you see opportunities for improvement, note them in the log but don't implement them unless they're needed for the current phase to work.
- **Tests are not optional.** Every feature gets tests. If a test framework isn't specified, choose the most common one for the stack and note it as a deviation.
- **Shared interfaces go where the plan says.** When creating a type or utility that multiple features depend on, place it in the location specified by the Shared Interfaces table.
- **Keep commits clean.** One commit per phase with a descriptive message. Don't commit broken code.
- **Env vars are a gate.** Don't build features that call external services if the required env vars are missing. Warn and wait.
