---
name: swe-workflow
description: Behavioral guards for all code-related tasks, mandatory workflow triage, and a structured planning workflow for complex or broad coding tasks. Always declare Lightweight or Full mode before edits; use Full workflow for mandatory triggers, ambiguity, or work needing resumable state.
license: MIT
metadata:
  version: "1.5.1"
  author: "Evan Xu"
---

# SWE Workflow

> Every code task starts with workflow triage. Simple tasks stay lightweight; broad or ambiguous tasks use a persisted plan.

## Workflow Triage — Required Before Any Edit

The first assistant response for code work MUST include:

```text
Workflow mode: Lightweight | Full
Reason: <one sentence>
Success criteria:
- <what done means>
Plan needed: yes | no
```

**Full workflow is mandatory for:** repo-wide scans/migrations/cleanup, lint/type/build/test cleanup, broad refactors touching multiple files, deleting/moving/renaming files, backend + frontend changes in one task, API/schema/route/config contract changes, source-of-truth doc updates, tasks touching >3 files, or ambiguous scope. If Lightweight work hits any of these triggers, STOP, escalate to Full, and create a plan before continuing.

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
2. **Plan** — break into small ordered steps, write `plans/<task>.md` using the format below. See [create-plan](references/create-plan.md).
3. **Execute** — one step at a time: [execute-step](references/execute-step.md) → [verify-step](references/verify-step.md) → [maintain-repo-map](references/maintain-repo-map.md) → [persist-plan](references/persist-plan.md). Every 2–3 steps: [reflect](references/reflect-after-changes.md). Before pausing: [dump-context](references/dump-context.md).
4. **Reflect** — after all steps done: [global-reflection](references/global-reflection.md).

### Pre-Edit Gate

Before any write/edit/delete on task target files: a plan file must exist, exactly one step must be `IN_PROGRESS`, and the edit must map to that step. Only `plans/*.md` files may be written before this gate is satisfied.

### Resume Protocol

New session on existing work → read in order: `plans/context.md` → `plans/<task>.md` → `plans/repo-map.md`. See [resume-workflow](references/resume-workflow.md).

## Plan Format — Required Structure

Every `plans/<task>.md` MUST use this structure. Do NOT invent your own format.

```markdown
# Plan: [Task Name]

> Status: DRAFT | IN_PROGRESS | COMPLETED
> Created: YYYY-MM-DD
> Last Updated: YYYY-MM-DD

## Goal
[One sentence]

## Assumptions
- [What we're taking for granted]

## Open Questions
> Must be empty before implementation begins.

## Context & Learnings
### Key Decisions
- [Decision]: [Rationale]
### Gotchas & Warnings
- [Warning]: [What to watch out for]

## Steps

### Step 1: [Title]
**Status:** PENDING | IN_PROGRESS | COMPLETED | BLOCKED
**Prerequisites:**
- [What must be true before starting]
**Deliverables:**
- [What this step produces]
**Plan:**
- [ ] [Specific action]
**Validation Checklist:**
- [ ] [How to verify]
**Test Checklist:**
- [ ] [What tests to write/run]
**Implementation Notes:**
[Fill after implementation]
**Files Changed:**
[List exact paths after implementation]

<!-- Repeat for each step -->

## Implementation Log
| Date | Step | Summary |
|------|------|---------|
```

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
Each step should be a thin vertical slice through all layers (types, logic, tests) — independently verifiable.

Status values: `PENDING` | `IN_PROGRESS` | `COMPLETED` | `BLOCKED`

## Core Constraints

- **One step at a time** — never batch multiple steps.
- **Stay in scope** — only touch files in the step's plan.
- **Persist reality** — keep the plan file accurate at all times.
- **Maintain the repo map** — every touched file must be tracked.
- **Never mark COMPLETED with known issues** — fix or mark BLOCKED.

## Reference Guide

| Reference | Purpose |
|---|---|
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
