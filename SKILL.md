---
name: swe-workflow
description: Structured workflow for complex coding tasks requiring planning and coordination. Use ONLY when task involves multiple interdependent files, has ambiguous scope needing clarification, or requires explicit upfront planning. Do NOT use for single-file edits, quick fixes, simple refactors, debugging, or when requirements are already clear.
license: MIT
metadata:
  version: "1.3.0"
  author: "Evan Xu"
---

# SWE Workflow

A structured development workflow for coding agents — compatible with Pi, Claude Code, Cursor, Codex, Gemini, and other agents supporting the Agent Skills spec.

> No code without a plan. One step at a time. Every step validated and tested.

## ⚠️ When NOT to Use This Skill

Skip this workflow entirely when any of these are true:

| Skip if... | Why |
|---|---|
| Task is a single straightforward edit | No coordination needed |
| Requirements are clear and unambiguous | No clarification phase needed |
| Task takes <5 minutes without planning | Overhead exceeds benefit |
| You're answering questions or explaining code | No implementation needed |
| User says "quick fix" or "just change X" | Explicit simple request |
| Fixing a bug with known cause | Direct implementation suffices |

**Decision test:** if the answer to all three is "no", work directly and don't use this skill.

1. Does this require tracking state across multiple files?
2. Would I benefit from writing down a plan before starting?
3. Is the scope unclear enough to need clarification?

## Quick Start

| Starting point | Load reference |
|---|---|
| New task or feature | [require-clarification](references/require-clarification.md) |
| Create plan from clarified request | [create-plan](references/create-plan.md) |
| Resume existing work (new session, handoff) | [resume-workflow](references/resume-workflow.md) |
| Execute the next PENDING step | [execute-step](references/execute-step.md) |

## File Structure

```
plans/
├── repo-map.md      # Project-wide file inventory (shared across all plans)
├── context.md       # Current session state (overwritten each pause)
└── <task>.md        # Task-specific plan and progress (one per feature)
```

| File | Scope | Updated | Purpose |
|---|---|---|---|
| `repo-map.md` | Project | When files are discovered | Navigation, shared file knowledge |
| `context.md` | Session | Before each pause | Resume capability across sessions |
| `<task>.md` | Task | After each step | Task tracking and progress |

## Workflow Phases

### 1. Understand — [require-clarification](references/require-clarification.md)
Analyze scope, inputs, outputs, success criteria. Ask targeted questions if ambiguous. Get explicit user confirmation before planning.

### 2. Plan — [create-plan](references/create-plan.md)
Break work into small ordered steps (5–15 min each). Save to `plans/<feature>.md`. Initialize `plans/repo-map.md` if absent. Use [plan-template](assets/plan-template.md) and [repo-map-template](assets/repo-map-template.md).

### 3. Execute — one step at a time

For each PENDING step:

1. [execute-step](references/execute-step.md) — protect code, implement only what's in scope
2. [verify-step](references/verify-step.md) — validate + test + review diff (single gate)
3. [maintain-repo-map](references/maintain-repo-map.md) — ensure every touched file is tracked
4. [persist-plan](references/persist-plan.md) — mark COMPLETED, update context

Every 2–3 steps (or after risky changes): [reflect-after-changes](references/reflect-after-changes.md).

Before pausing to ask the user anything: [dump-context](references/dump-context.md).

### 4. Reflect — [global-reflection](references/global-reflection.md)
After all steps COMPLETED. Re-read everything, verify the goal was achieved.

## Skill Chain

```
require-clarification → create-plan → execute-step → verify-step → maintain-repo-map → persist-plan
                                            ↑_______________________________________________|
                                                    (next PENDING step)

   every 2-3 steps: reflect-after-changes     before pausing: dump-context
   all steps done:  global-reflection
```

## Reference Guide

| Reference | Purpose | When |
|---|---|---|
| [require-clarification](references/require-clarification.md) | Clarify ambiguous requests | Before any planning |
| [create-plan](references/create-plan.md) | Create structured plan | After clarification |
| [resume-workflow](references/resume-workflow.md) | Resume existing work | New session / handoff |
| [execute-step](references/execute-step.md) | Implement one step (includes code protection) | Ready to code |
| [verify-step](references/verify-step.md) | Validate, test, and review diff | After implementation |
| [maintain-repo-map](references/maintain-repo-map.md) | Track file locations | Planning + after each step |
| [persist-plan](references/persist-plan.md) | Update plan status | After any progress |
| [dump-context](references/dump-context.md) | Save session state | Before pausing for user |
| [reflect-after-changes](references/reflect-after-changes.md) | Catch complexity early | Every 2–3 steps |
| [global-reflection](references/global-reflection.md) | Final feature review | All steps complete |

## Workflow Guards

**Do NOT proceed if:**

| Condition | Required action |
|---|---|
| No plan file exists | Run create-plan |
| A step is IN_PROGRESS | Complete or revert before starting another |
| Clarification has open questions | Run require-clarification |
| verify-step failed | Fix issues first |
| Repo map not synchronized with Files Changed | Update `plans/repo-map.md` first |
| Pausing for user without dumping context | Run dump-context first |

## Status Values

| Status | Meaning |
|---|---|
| PENDING | Not started |
| IN_PROGRESS | Currently being worked on |
| COMPLETED | Done, verified, repo map synced |
| BLOCKED | Cannot proceed (reason documented) |

## Core Constraints

- **One step at a time** — never batch multiple steps.
- **Stay in scope** — do not touch files outside the step's Plan.
- **Trust the plan** — do not redo COMPLETED steps.
- **Persist reality** — keep `<task>.md` accurate at all times.
- **Maintain the repo map** — every touched file must be in `plans/repo-map.md`.
- **Dump context before pausing** — preserves session for resume.
- **Never mark COMPLETED with known issues** — fix or mark BLOCKED.

## Resume Protocol

When starting a fresh session on existing work, read three files **in this order**:

1. `plans/context.md` — where work stopped, recent decisions
2. `plans/<task>.md` — full task definition and step progress
3. `plans/repo-map.md` — file locations and architecture notes

See [resume-workflow](references/resume-workflow.md) for the full procedure and edge cases (missing context file, multiple plans, stale plan, etc.).

## Templates

- [assets/plan-template.md](assets/plan-template.md)
- [assets/repo-map-template.md](assets/repo-map-template.md)
- [assets/context-template.md](assets/context-template.md)

## Why This Works

- **Isolated steps** — Prerequisites and Deliverables make each step self-contained; a new session can pick up any step.
- **Preserved learnings** — Context & Learnings captures decisions, not just actions, so they aren't re-litigated.
- **Fresh context per session** — New session + three-file resume protocol = no accumulated confusion.
- **Plan-driven, not memory-driven** — The agent reads the plan to know state, not its own context window.
- **Cross-task knowledge** — `repo-map.md` survives across tasks; patterns and file locations compound.
