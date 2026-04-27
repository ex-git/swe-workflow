---
name: swe-workflow
description: Behavioral guards for all code-related tasks, mandatory workflow triage, and a structured planning workflow for complex or broad coding tasks. Always declare Lightweight or Full mode before edits; use Full workflow for mandatory triggers, ambiguity, or work needing resumable state.
license: MIT
metadata:
  version: "1.4.2"
  author: "Evan Xu"
---

# SWE Workflow

Behavioral guards for all code-related tasks, plus a structured development workflow for complex coding tasks — compatible with Pi, Claude Code, Cursor, Codex, Gemini, and other agents supporting the Agent Skills spec.

> Apply the Behavioral Guards to every coding task. Every code task starts with workflow triage; use plan files only when the task is large, ambiguous, or hits a mandatory Full workflow trigger.

## Mandatory Workflow Triage — All Code Tasks

Before using any write/edit/delete tool for a code-related task, declare the workflow mode. The first assistant response for code work MUST include:

```text
Workflow mode: Lightweight | Full
Reason: <one sentence>
Success criteria:
- <what done means>
Plan needed: yes | no
```

If the task is already in progress and plan files exist, declare Full workflow mode and use the [resume protocol](#full-workflow-resume-protocol) before making implementation edits.

You MUST choose **Full workflow mode** for:

- repo-wide scans, migrations, or cleanup
- lint/type/build/test cleanup
- broad refactors or behavior-preserving rewrites touching multiple files
- deleting files or moving/renaming files
- backend + frontend/UI changes in one task
- API/schema/route/tooling/configuration contract changes
- docs/source-of-truth updates, including changes to this skill's workflow rules
- any task expected to touch more than 3 files
- any task where scope or success criteria are ambiguous

If a task begins in Lightweight mode but later hits any Full workflow trigger, STOP before further edits, declare escalation to Full workflow mode, create/update the plan, and continue from the current verified state.

## Operating Modes

| Mode | Use when | Required behavior |
|---|---|---|
| **Lightweight mode** | Simple edits, quick fixes, clear debugging, small refactors, code review/explanation, and none of the mandatory Full triggers apply | Apply Behavioral Guards; work directly without plan files |
| **Full workflow mode** | Any mandatory Full trigger, multiple interdependent files, ambiguous scope, explicit upfront planning, or work that benefits from resumable state | Apply Behavioral Guards + require clarification, create plan files, execute one verified step at a time |

**Full workflow decision test:** use full workflow mode if any answer is "yes":

1. Does a mandatory Full workflow trigger apply?
2. Does this require tracking state across multiple files?
3. Would I benefit from writing down a plan before starting?
4. Is the scope unclear enough to need clarification?

If all four are "no", stay in Lightweight mode. Do **not** create `plans/` files just to satisfy the workflow.

## Behavioral Guards — Apply to All Code Tasks

These guards are always active when writing, reviewing, debugging, or refactoring code, regardless of whether full workflow mode is used.

### 1. Think Before Coding

- State assumptions explicitly when they matter.
- If multiple interpretations exist, present them instead of silently choosing one.
- If something important is unclear, stop and ask a targeted question.
- Surface tradeoffs when there is more than one reasonable approach.
- Push back when the requested approach is likely overcomplicated, risky, or inconsistent with the codebase.

### 2. Simplicity First

- Write the minimum code that solves the requested problem.
- Do not add features, abstractions, configurability, or flexibility that were not requested.
- Avoid abstractions for single-use code.
- Do not add defensive handling for impossible or irrelevant scenarios.
- If the implementation feels much larger than the problem, simplify before proceeding.

### 3. Surgical Changes

- Touch only files and lines needed for the request.
- Do not perform unrelated cleanups, formatting changes, comment rewrites, renames, or refactors.
- Match existing code style and patterns, even if you would choose differently in new code.
- If you notice unrelated dead code or quality issues, mention them; do not fix them unless asked.
- Remove imports, variables, functions, or files only when your own changes made them unused or obsolete.
- Every changed line should trace directly to the user request or the current plan step.

### 4. Goal-Driven Execution

- Define what success means before implementing.
- Prefer verifiable checks: tests, type checks, lint, build, or documented manual verification.
- For new utilities, functions, modules, or components, add focused tests covering happy path, edge cases, and error paths when a test framework exists.
- For bug fixes, prefer a regression test or reproduction before/alongside the fix.
- For refactors, preserve behavior and verify before and after when practical.
- Run configured project quality gates, including pre-commit hooks such as lefthook, before calling work done.
- Loop until the requested behavior is verified or clearly report what could not be verified.

## Full Workflow Quick Start

Use this table only after choosing **full workflow mode**.

| Starting point | Load reference |
|---|---|
| New task or feature | [require-clarification](references/require-clarification.md) |
| Create plan from clarified request | [create-plan](references/create-plan.md) |
| Resume existing work (new session, handoff) | [resume-workflow](references/resume-workflow.md) |
| Execute the next PENDING step | [execute-step](references/execute-step.md) |

## Compact Full Workflow — Cleanup, Refactor, Lint

Use this compact variant when a broad cleanup/refactor/lint task triggers Full workflow mode but does not need a feature-spec plan:

1. Create `plans/<task>.md` with a checklist of categories or file groups.
2. Scan and record findings before fixing them.
3. Fix one category or file group at a time as the current `IN_PROGRESS` step.
4. Verify after each category with the relevant lint/type/build/test/manual check.
5. Update repo map, plan status, and any affected source-of-truth docs before moving on.

This is still Full workflow mode: plan file required, one verified step at a time, no batching unrelated fixes.

## Full Workflow File Structure

Created only in full workflow mode.

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

## Full Workflow Phases

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

## Full Workflow Guards

In Full workflow mode, **do NOT proceed if:**

| Condition | Required action |
|---|---|
| No plan file exists and you are not currently creating the required workflow plan files | Run create-plan |
| Plan file exists but is missing steps for later phases discussed with the user | Complete the plan file with ALL steps before executing any |
| About to edit task target files but no step is `IN_PROGRESS` | Mark the current PENDING step `IN_PROGRESS` via execute-step first |
| Intended edit does not map to the current `IN_PROGRESS` step | STOP; update the plan or ask for clarification before editing |
| A different step is already `IN_PROGRESS` | Complete or revert it before starting another |
| Clarification has open questions | Run require-clarification |
| verify-step failed | Fix issues first |
| Repo map not synchronized with Files Changed | Update `plans/repo-map.md` first |
| Pausing for user without dumping context | Run dump-context first |

**Pre-edit hard stop:** before any write/edit/delete tool call that changes task target files in Full workflow mode, a plan file must exist, exactly one current step must be `IN_PROGRESS`, and the intended edit must belong to that step. Only workflow bookkeeping files (`plans/*.md`) may be created or updated before this gate is satisfied.

## Status Values

| Status | Meaning |
|---|---|
| PENDING | Not started |
| IN_PROGRESS | Currently being worked on |
| COMPLETED | Done, verified, repo map synced |
| BLOCKED | Cannot proceed (reason documented) |

## Full Workflow Core Constraints

- **One step at a time** — never batch multiple steps.
- **Stay in scope** — do not touch files outside the step's Plan.
- **Trust the plan** — do not redo COMPLETED steps.
- **Persist reality** — keep `<task>.md` accurate at all times.
- **Maintain the repo map** — every touched file must be in `plans/repo-map.md`.
- **Dump context before pausing** — preserves session for resume.
- **Never mark COMPLETED with known issues** — fix or mark BLOCKED.

## Full Workflow Resume Protocol

When starting a fresh session on existing full workflow work, read three files **in this order**:

1. `plans/context.md` — where work stopped, recent decisions
2. `plans/<task>.md` — full task definition and step progress
3. `plans/repo-map.md` — file locations and architecture notes

See [resume-workflow](references/resume-workflow.md) for the full procedure and edge cases (missing context file, multiple plans, stale plan, etc.).

## Templates

- [assets/plan-template.md](assets/plan-template.md)
- [assets/repo-map-template.md](assets/repo-map-template.md)
- [assets/context-template.md](assets/context-template.md)

## Why Full Workflow Mode Works

- **Isolated steps** — Prerequisites and Deliverables make each step self-contained; a new session can pick up any step.
- **Preserved learnings** — Context & Learnings captures decisions, not just actions, so they aren't re-litigated.
- **Fresh context per session** — New session + three-file resume protocol = no accumulated confusion.
- **Plan-driven, not memory-driven** — The agent reads the plan to know state, not its own context window.
- **Cross-task knowledge** — `repo-map.md` survives across tasks; patterns and file locations compound.
