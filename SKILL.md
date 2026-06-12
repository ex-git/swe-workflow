[$T1=references/project-agents-template.md, $T2=references/task-overview-template.md, $T3=references/step-template.md, $T4=references/code-quality.md, $T5=references/execute-step.md, $T6=references/create-plan.md, $T7=references/verify-step.md, $T8=steps/step-N.md]
---
name: swe-workflow
description: >-
  REQUIRED for every code-related task. Step 1 read this skill's SKILL.md via
  the `read` tool before acting — do not proceed from this description alone.
  Step 2 emit the triage block as the first lines of your reply; it has four
  labels exactly `Workflow mode: Lightweight|Full`, `Reason:`, `Success
  criteria:`, `Plan needed: yes|no`. Step 3 for Full-mode work, write
  `plans/<YYYY-MM-DD>-<slug>/plan.md` and
  `plans/<YYYY-MM-DD>-<slug>/$T8` using the templates at
  `$T2` and `$T3`
  (required per-step fields: `Status`, `Goal`, `Prerequisites`,
  `Deliverables`, `Plan`, `Quality Checklist`, `Validation Checklist`,
  `Test Checklist`, `Implementation Notes`, `Files Changed`). Violations
  include freeform plans, inline `### Step N — title [STATUS]` headers,
  skipping the triage block, and deciding a task is "simple" without
  emitting triage. Applies to reads, edits, writes, and bash — triage comes
  first.
license: MIT
metadata:
  version: "1.12.0"
  author: "Evan Xu"
---

# SWE Workflow

> Every code task starts with workflow triage. Simple tasks stay lightweight; broad or ambiguous tasks use a persisted plan.

## Contract

1. **Triage first.** The first lines of your reply to any code-related task must be:
   ```text
   Workflow mode: Lightweight | Full
   Reason: <one sentence>
   Success criteria:
   - <what done means>
   Plan needed: yes | no
   ```
2. **Full workflow is mandatory for:** repo-wide scans/migrations/cleanup, lint/type/build/test cleanup, broad refactors touching multiple files, deleting/moving/renaming files, backend + frontend changes in one task, API/schema/route/config contract changes, source-of-truth doc updates, tasks touching >3 files, or ambiguous scope. Lightweight work that hits any trigger MUST escalate: stop, declare Full, create a plan, then resume.
3. **Pre-edit gate (Full mode).** Before any `write`/`edit`/`bash` on task target files: a plan directory must exist under `plans/<YYYY-MM-DD>-<slug>/`, exactly one step file must have `> Status: IN_PROGRESS`, and the edit must map to that step. Only workflow bookkeeping files may be written before this gate is satisfied.
4. **Plan file format.** Copy [`$T2`]($T2) to `plan.md` and [`$T3`]($T3) to each step file verbatim. Do not reconstruct from memory. Every step has ten fields: `Status`, `Goal`, `Prerequisites`, `Deliverables`, `Plan`, `Quality Checklist`, `Validation Checklist`, `Test Checklist`, `Implementation Notes`, `Files Changed`.
5. **Clarification gate (Full mode).** Ask requirement questions in chat before writing plan files. A valid plan has `Open Questions: None.` — clarification is never an implementation step.
6. **One step at a time.** Never batch. Never mark `COMPLETED` with known failures — fix or mark `BLOCKED`.

## Delegated Mode

When **all** of the following are true, skip triage/plan/pre-edit gate:

1. Task assigned by another agent or orchestrator (not an open-ended user request)
2. Explicit scope, success criteria, or acceptance contract received
3. Operating in a focused role (implementer, reviewer, analyst, etc.)

In Delegated Mode: follow all Behavioral Guards and [`$T4`]($T4); report changed files, commands, validation evidence, and surprises; escalate unapproved design/architecture decisions; stay in scope.

## Statuses

**Plan-level** (`plan.md`): `DRAFT` → `ACTIVE` → `COMPLETED` → `ARCHIVED`
**Step-level** (`$T8`): `PENDING` → `IN_PROGRESS` → `COMPLETED` | `BLOCKED`

Only one step may be `IN_PROGRESS` at a time. The pre-edit gate checks step-level status.

## Reference Loading

Load only what the current phase needs:

- **Planning:** [`$T6`]($T6), [`$T2`]($T2), [`$T3`]($T3)
- **Execution:** [`$T5`]($T5), [`$T4`]($T4)
- **Verification:** [`$T7`]($T7)
- **Command discovery:** [`references/command-discovery.md`](references/command-discovery.md)
- **Risk review:** [`references/risk-classification.md`](references/risk-classification.md)
- **Project setup:** [`$T1`]($T1)

## Behavioral Guards

Active for the entire session. Do not drift.

1. **Evidence first** — read target files before editing; search callers/usages before changing shared behavior; label claims as `Verified`/`Assumption`/`Unknown`.
2. **Anti-shortcut gate** — before editing: target read ✓, impact search (or `N/A — isolated`) ✓, validation command (or skipped reason) ✓.
3. **Think in code** — for aggregate analysis, prefer short scripts/commands that compute results over many raw file dumps. For noisy output, store in `/tmp`, print bounded slice.
4. **Simplicity** — minimum code for the problem; no unrequested features, abstractions, or dependencies.
5. **Surgical changes** — touch only needed files/lines; match existing formatting, naming, and conventions; do not copy degraded patterns.
6. **Reuse before create** — search for existing equivalents before writing new components/utilities/patterns; evidence of search in Verified Facts.
7. **Design discipline** — do not make silent design choices. Surface decisions for user confirmation. Verify approach matches conventions or Design Decisions table.
8. **Goal-driven** — define success before coding; verify via tests/lint/build/typecheck; add focused tests for new code and bug fixes; fix introduced issues or report blockers.

## Full Workflow Phases

1. **Clarify** — ask targeted questions, get explicit confirmation. See [`references/require-clarification.md`](references/require-clarification.md).
2. **Plan** — break into vertical-slice steps. See [`$T6`]($T6).
3. **Execute** — one step at a time: [`$T5`]($T5) → [`$T7`]($T7) → [`references/persist-plan.md`](references/persist-plan.md). Every 2–3 steps: [`references/checkpoint.md`](references/checkpoint.md). Before pausing: dump context.
4. **Reflect** — after all steps done: [`references/global-reflection.md`](references/global-reflection.md).

### Resume Protocol

New session → read `plans/context.md` → relevant `plan.md` → current step file. See [`references/resume-workflow.md`](references/resume-workflow.md).
