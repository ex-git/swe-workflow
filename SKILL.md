---
name: swe-workflow
description: >-
  REQUIRED for every code-related task. Step 1 read this skill's SKILL.md via
  the `read` tool before acting — do not proceed from this description alone.
  Step 2 emit the triage block as the first lines of your reply; it has four
  labels exactly `Workflow mode: Lightweight|Full`, `Reason:`, `Success
  criteria:`, `Plan needed: yes|no`. Step 3 for Full-mode work, write
  `plans/<YYYY-MM-DD>-<slug>/plan.md` and
  `plans/<YYYY-MM-DD>-<slug>/steps/step-N.md` using the templates at
  `references/task-overview-template.md` and `references/step-template.md`
  (required per-step fields: `Status`, `Goal`, `Prerequisites`,
  `Deliverables`, `Plan`, `Quality Checklist`, `Validation Checklist`,
  `Test Checklist`, `Implementation Notes`, `Files Changed`). Violations
  include freeform plans, inline `### Step N — title [STATUS]` headers,
  skipping the triage block, and deciding a task is "simple" without
  emitting triage. Applies to reads, edits, writes, and bash — triage comes
  first.
license: MIT
metadata:
  version: "1.10.0"
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
3. **Pre-edit gate (Full mode).** Before any `write`/`edit`/`bash` on task target files: a plan directory must exist under `plans/<YYYY-MM-DD>-<slug>/`, exactly one step file must have `> Status: IN_PROGRESS`, and the edit must map to that step. Only workflow bookkeeping files (`plans/context.md`, `plans/*/plan.md`, `plans/*/steps/*.md`) may be written before this gate is satisfied.
4. **Plan file format.** The task overview at `plans/<YYYY-MM-DD>-<slug>/plan.md` copies [`references/task-overview-template.md`](references/task-overview-template.md) verbatim (contains `Design Decisions` table, `Working Set`, `Verified Facts`). Each step file `plans/<YYYY-MM-DD>-<slug>/steps/step-N.md` copies [`references/step-template.md`](references/step-template.md) verbatim. Every step has ten fields: `Status`, `Goal`, `Prerequisites`, `Deliverables`, `Plan`, `Quality Checklist`, `Validation Checklist`, `Test Checklist`, `Implementation Notes`, `Files Changed`.
5. **Clarification gate (Full mode).** If any requirement question is known, ask it in chat before writing or finalizing plan files. A valid plan has no unresolved questions; `DRAFT` means awaiting plan approval, not awaiting requirement answers, and clarification must not be turned into an implementation step.
6. **One step at a time.** Never batch. Never mark `COMPLETED` with known failures — fix or mark `BLOCKED`.

## Plan Directory Structure

All plans live in date-prefixed subdirectories for chronological sorting and smaller-model efficiency:

```
plans/
├── context.md                           # Optional session state (overwritten each pause)
└── <YYYY-MM-DD>-<slug>/                 # One directory per task
     ├── plan.md                         # Task overview, design decisions, working set
     └── steps/                          # Individual step files
          ├── step-1.md                  # Step 1: focused, self-contained
          ├── step-2.md                  # Step 2: ...
          └── step-N.md                  # Step N: ...
```

Date-prefixed slugs (`YYYY-MM-DD-<name>`) ensure chronological file sorting. To find active work, scan `plans/<YYYY-MM-DD>-<slug>/plan.md` files and step statuses. One file per step eliminates context noise from unrelated steps — critical for smaller LLM models with limited context windows.

## Status Vocabularies

The workflow uses **two distinct status vocabularies** — do not mix them up:

| Layer | File | Values | Meaning |
|-------|------|--------|---------|
| **Plan-level** (lifecycle) | `plan.md` | `DRAFT │ ACTIVE │ COMPLETED │ ARCHIVED` | Where the task is in its overall lifecycle |
| **Step-level** (execution) | `steps/step-N.md` | `PENDING │ IN_PROGRESS │ COMPLETED │ BLOCKED` | What the agent is doing right now |

**Plan transitions:** `DRAFT` (awaiting approval) → `ACTIVE` (executing) → `COMPLETED` (all steps done) → `ARCHIVED` (optional, for long-term cleanup).

**Step transitions:** `PENDING` → `IN_PROGRESS` → `COMPLETED` (or `BLOCKED` if it cannot proceed). Only one step per plan may be `IN_PROGRESS` at a time.

The pre-edit gate checks **step-level** `IN_PROGRESS`, never plan-level.

## Plan Templates

Use [`references/task-overview-template.md`](references/task-overview-template.md) as the source of truth for `plans/<YYYY-MM-DD>-<slug>/plan.md`. Use [`references/step-template.md`](references/step-template.md) for each step file. Copy them verbatim for every new plan. Do not reconstruct the structure from memory; do not invent alternative formats.

## Reference Loading Rules

Load reference files only when the current phase needs them.

- Planning: read [`references/create-plan.md`](references/create-plan.md), [`references/task-overview-template.md`](references/task-overview-template.md), and [`references/step-template.md`](references/step-template.md).
- Implementation: read [`references/execute-step.md`](references/execute-step.md) and [`references/code-quality.md`](references/code-quality.md).
- Verification: read [`references/verify-step.md`](references/verify-step.md) and [`references/definition-of-done.md`](references/definition-of-done.md).
- Command discovery: read [`references/command-discovery.md`](references/command-discovery.md) before deciding validation commands.
- Risk review: read [`references/risk-classification.md`](references/risk-classification.md) for API, data, security, performance, or observability risk.
- Project setup: use [`references/project-agents-template.md`](references/project-agents-template.md) only when creating or improving a target repo's `AGENTS.md`.

## Anti-Patterns — Wrong vs Right

**Plan format — missing required fields:**
```
WRONG:   ### Step 1 — Add dependency [COMPLETED]      ← no Status field, no Prerequisites,
         ### Step 2 — Create module [COMPLETED]         no Deliverables, no Quality, no Files Changed

RIGHT:   ### Step 1: Add dependency
         **Status:** COMPLETED
         **Prerequisites:** ...
         **Deliverables:** ...
         **Quality Checklist:** ...
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

**Open questions parked in the plan:**
```
WRONG:   ## Open Questions
         - Which UI should change?
         ### Step 1: Resolve open questions

RIGHT:  Ask the question in chat before creating/finalizing the plan,
        then write ## Open Questions as `None.`
```

**Silent design assumptions:**
```
WRONG:  Agent picks modal vs page, dropdown vs radio, join table vs JSON column
        without asking — implements whatever seems reasonable

RIGHT:  Surface the choice in Design Decisions table during planning,
        confirm with user, then implement the confirmed approach
```

Status values: `PENDING` | `IN_PROGRESS` | `COMPLETED` | `BLOCKED`

## Behavioral Guards — All Code Tasks

These guards are active for the **entire session**, not just the first response. Do not drift.

1. **Evidence first** — read relevant files before editing; verify paths/imports/dependencies; search callers/usages before changing shared behavior.
2. **Anti-shortcut gate** — before editing, have a target read, impact search when shared behavior can change, and validation command or skipped reason. If any item is `N/A`, say why.
3. **Evidence discipline** — label repo claims as `Verified`, `Assumption`, `Unknown`, or `Recommendation`; cite files/commands for key claims and do not present assumptions as facts.
4. **Think in code** — for aggregate analysis, prefer short scripts/commands that compute results and print only what is needed instead of many raw file/tool dumps.
5. **Tool-use heuristics** — default to targeted `rg`/scoped reads/bounded command output; avoid pasting large raw logs or file contents when a focused summary or key lines are sufficient. For noisy commands, store raw native output in `/tmp`, print only a bounded slice, and record `Command`, `Exit`, `Output`, `Summary`, `Freshness`.
6. **Simplicity** — minimum code for the problem; no unrequested features, abstractions, dependencies, or defensive handling.
7. **Surgical changes** — touch only needed files/lines; match formatting, naming, and import conventions; do not copy degraded correctness patterns.
8. **Reuse before create** — before writing a new component, utility, hook, type, or schema pattern, search for existing equivalents; evidence of the search must appear in Verified Facts. Extract duplication when extraction is small and in scope; mention unrelated issues but don't fix them.
9. **Design discipline** — do not make silent design choices about UI layout, schema shape, component structure, or API contracts. Surface design decisions for user confirmation during clarification. During execution, verify approach matches existing conventions or the Design Decisions table.
10. **Goal-driven** — define success before coding; verify via tests/lint/format/build/typecheck; add focused tests for new code and bug fixes when a test framework exists; run quality gates including pre-commit hooks; fix introduced issues or report blockers.

## Full Workflow

### Phases

1. **Clarify** — analyze scope, ask targeted questions, get explicit confirmation. See [require-clarification](references/require-clarification.md).
2. **Plan** — break into small ordered steps, write task overview to `plans/<YYYY-MM-DD>-<slug>/plan.md` and step files to `steps/step-N.md`. See [create-plan](references/create-plan.md).
3. **Execute** — one step at a time: [execute-step](references/execute-step.md) → [verify-step](references/verify-step.md) → [persist-plan](references/persist-plan.md). Every 2–3 steps: [reflect](references/reflect-after-changes.md). Before pausing: [dump-context](references/dump-context.md).
4. **Reflect** — after all steps done: [global-reflection](references/global-reflection.md).

### Resume Protocol

New session on existing work → read in order: `plans/context.md` if present → relevant `plans/<YYYY-MM-DD>-<slug>/plan.md` → current step file. If the task is unknown, scan plan directories for ACTIVE plans and step statuses. See [resume-workflow](references/resume-workflow.md).

## Core Constraints

- **One step at a time** — never batch multiple steps.
- **Stay in scope** — only touch files in the step's plan.
- **Persist reality** — keep `plan.md` and step files accurate at all times.
- **Record evidence** — keep Working Set, Verified Facts, Implementation Notes, and Files Changed accurate across plan overview and step files.
- **Never mark COMPLETED with known issues** — fix or mark `BLOCKED`.

## Reference Guide

| Reference | Purpose |
|---|---|
| [task-overview-template](references/task-overview-template.md) | Task overview skeleton — copy to `plan.md` verbatim |
| [step-template](references/step-template.md) | Per-step file skeleton — copy to `steps/step-N.md` verbatim |
| [require-clarification](references/require-clarification.md) | Clarify ambiguous requests before planning |
| [create-plan](references/create-plan.md) | Exploration, step sizing, plan creation procedure |
| [execute-step](references/execute-step.md) | Code protection, implementation, scope management |
| [verify-step](references/verify-step.md) | Validate + test + review diff (single gate) |
| [persist-plan](references/persist-plan.md) | Update plan and step status after each step |
| [dump-context](references/dump-context.md) | Save session state before pausing |
| [reflect-after-changes](references/reflect-after-changes.md) | Catch complexity every 2–3 steps |
| [global-reflection](references/global-reflection.md) | Final review when all steps done |
| [resume-workflow](references/resume-workflow.md) | Resume existing work in a new session |
