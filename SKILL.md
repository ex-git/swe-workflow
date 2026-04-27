---
name: swe-workflow
description: >-
  REQUIRED for every code-related task. Step 1 read this skill's SKILL.md via
  the `read` tool before acting — do not proceed from this description alone.
  Step 2 emit the triage block as the first lines of your reply; it has four
  labels exactly `Workflow mode: Lightweight|Full`, `Reason:`, `Success
  criteria:`, `Plan needed: yes|no`. Step 3 for Full-mode work, write
  `plans/<task>.md` using the template at `references/plan-template.md`
  (required per-step fields `Status`, `Prerequisites`, `Deliverables`, `Plan`,
  `Validation Checklist`, `Test Checklist`, `Files Changed`, plus a top-level
  `Implementation Log`). Violations include freeform plans, inline
  `### Step N — title [STATUS]` headers, skipping the triage block, and
  deciding a task is "simple" without emitting triage. Applies to reads,
  edits, writes, and bash — triage comes first.
license: MIT
metadata:
  version: "1.6.0"
  author: "Evan Xu"
---

# SWE Workflow

> Every code task starts with workflow triage. Simple tasks stay lightweight; broad or ambiguous tasks use a persisted plan.

## Contract

This block is the minimum every agent must follow. If you skim only this section, you are still compliant.

1. **Triage first.** The first lines of your reply to any code-related task must be:
   ```text
   Workflow mode: Lightweight | Full
   Reason: <one sentence>
   Success criteria:
   - <what done means>
   Plan needed: yes | no
   ```
2. **Full workflow is mandatory for:** repo-wide scans/migrations/cleanup, lint/type/build/test cleanup, broad refactors touching multiple files, deleting/moving/renaming files, backend + frontend changes in one task, API/schema/route/config contract changes, source-of-truth doc updates, tasks touching >3 files, or ambiguous scope. Lightweight work that hits any trigger MUST escalate: stop, declare Full, create a plan, then resume.
3. **Pre-edit gate (Full mode).** Before any `write`/`edit`/`bash` on task target files: a `plans/<task>.md` file must exist, exactly one step must have `**Status:** IN_PROGRESS`, and the edit must map to that step. Only `plans/*.md` files may be written before this gate is satisfied.
4. **Plan file format.** Every `plans/<task>.md` copies [`references/plan-template.md`](references/plan-template.md) verbatim. Every step has all seven fields: `Status`, `Prerequisites`, `Deliverables`, `Plan`, `Validation Checklist`, `Test Checklist`, `Files Changed`. The file has a top-level `Implementation Log` table appended as steps complete.
5. **One step at a time.** Never batch. Never mark `COMPLETED` with known failures — fix or mark `BLOCKED`.

## Plan Template

Use [`references/plan-template.md`](references/plan-template.md) as the source of truth for `plans/<task>.md`. Copy it verbatim for every new plan. Do not reconstruct the structure from memory; do not invent alternative formats.

## Anti-Patterns — Wrong vs Right

**Plan format — missing required fields:**
```
WRONG:  ### Step 1 — Add dependency [COMPLETED]     ← no Status field, no Prerequisites,
        ### Step 2 — Create module [COMPLETED]         no Deliverables, no Validation, no Files Changed

RIGHT:  ### Step 1: Add dependency
        **Status:** COMPLETED
        **Prerequisites:** ...
        **Deliverables:** ...
        **Validation Checklist:** ...
        **Files Changed:** ...
```

**Step decomposition — horizontal vs vertical slices:**
```
WRONG (horizontal):              RIGHT (vertical):
  Step 1: Define all types         Step 1: Implement + test feature A
  Step 2: Write all functions       Step 2: Implement + test feature B
  Step 3: Write all tests           Step 3: Implement + test feature C
```
Each step is a thin vertical slice through all layers (types, logic, tests) — independently verifiable.

**Triage skipped because "the task seemed simple":** the triage block decides what is simple. Emit it even for one-line changes; Lightweight is a valid outcome, but the declaration is not optional.

Status values: `PENDING` | `IN_PROGRESS` | `COMPLETED` | `BLOCKED`

## Behavioral Guards — All Code Tasks

These guards are active for the **entire session**, not just the first response. Do not drift.

1. **Think first** — state assumptions; ask when ambiguous; surface tradeoffs; push back on overcomplicated approaches.
2. **Simplicity** — minimum code for the problem; no unrequested features, abstractions, or defensive handling.
3. **Surgical changes** — touch only needed files/lines; match existing style; mention unrelated issues but don't fix them.
4. **Goal-driven** — define success before coding; verify via tests/lint/build/typecheck; add focused tests for new code and bug fixes when a test framework exists; run quality gates including pre-commit hooks; loop until verified.

## Full Workflow

### File Structure

```
plans/
├── repo-map.md      # Project-wide file inventory (shared across plans)
├── context.md       # Session state (overwritten each pause)
└── <task>.md        # Task plan and progress (one per feature)
```

### Phases

1. **Clarify** — analyze scope, ask targeted questions, get explicit confirmation. See [require-clarification](references/require-clarification.md).
2. **Plan** — break into small ordered steps, copy [`references/plan-template.md`](references/plan-template.md) to `plans/<task>.md` and fill it in. See [create-plan](references/create-plan.md).
3. **Execute** — one step at a time: [execute-step](references/execute-step.md) → [verify-step](references/verify-step.md) → [maintain-repo-map](references/maintain-repo-map.md) → [persist-plan](references/persist-plan.md). Every 2–3 steps: [reflect](references/reflect-after-changes.md). Before pausing: [dump-context](references/dump-context.md).
4. **Reflect** — after all steps done: [global-reflection](references/global-reflection.md).

### Resume Protocol

New session on existing work → read in order: `plans/context.md` → `plans/<task>.md` → `plans/repo-map.md`. See [resume-workflow](references/resume-workflow.md).

## Core Constraints

- **One step at a time** — never batch multiple steps.
- **Stay in scope** — only touch files in the step's plan.
- **Persist reality** — keep the plan file accurate at all times.
- **Maintain the repo map** — every touched file must be tracked.
- **Never mark COMPLETED with known issues** — fix or mark BLOCKED.

## Reference Guide

| Reference | Purpose |
|---|---|
| [plan-template](references/plan-template.md) | Canonical `plans/<task>.md` skeleton — copy verbatim |
| [require-clarification](references/require-clarification.md) | Clarify ambiguous requests before planning |
| [create-plan](references/create-plan.md) | Exploration, step sizing, plan creation procedure |
| [execute-step](references/execute-step.md) | Code protection, implementation, scope management |
| [verify-step](references/verify-step.md) | Validate + test + review diff (single gate) |
| [maintain-repo-map](references/maintain-repo-map.md) | Track file locations and architecture |
| [persist-plan](references/persist-plan.md) | Update plan status and context |
| [dump-context](references/dump-context.md) | Save session state before pausing |
| [reflect-after-changes](references/reflect-after-changes.md) | Catch complexity every 2–3 steps |
| [global-reflection](references/global-reflection.md) | Final review when all steps done |
| [resume-workflow](references/resume-workflow.md) | Resume existing work in a new session |
