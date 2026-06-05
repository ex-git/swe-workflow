# Execute Step

## Contract — Read This First

1. Read the task overview from `plans/<YYYY-MM-DD>-<slug>/plan.md`. Confirm `Open Questions` is `None.` — if not, STOP and ask in chat.
2. Select the first PENDING step file (`steps/step-N.md`). Mark it `IN_PROGRESS`. Update the plan's `Last Updated` and Steps Overview table.
3. Re-read target files before editing — do not rely on memory. Verify imports, callers, dependencies.
4. Evidence-before-edit is mandatory: target read, impact search when shared behavior can change, and validation command or skipped reason. If any item is `N/A`, say why.
5. Think in code for analysis tasks — when answering aggregate questions across many files/logs, prefer one short script/command that computes concise results.
6. If step is design-sensitive (UI, schema, API shape): confirm the approach matches existing conventions or the Design Decisions table. If ambiguous, ask the user before implementing.
7. Protect code: `git commit` or `git stash` before changes.
8. Use `edit` for existing files, `write` for new files. Never `write` over an existing file.
9. Tool-output heuristics — prefer targeted search/scoped reads/bounded command output; summarize key lines instead of dumping large raw output. For noisy commands, write raw output to `/tmp` in its native format and print only a bounded slice plus command evidence.
10. Implement ONLY what this step requires. Do not touch unrelated files.
11. Reuse before create: if about to create a new component/utility/pattern, search for existing equivalents first. Record evidence.
12. Update Working Set, Verified Facts, Implementation Notes, Files Changed.
13. If scope needs to expand: STOP, update the plan, notify user. Never silently expand.

## Instructions

1. **Read the task overview** — use `read` on `plans/<YYYY-MM-DD>-<slug>/plan.md`. Confirm correct task.

2. **Open Questions gate** — confirm `## Open Questions` is `None.` in the plan overview. If unresolved, STOP and ask the user.

3. **Select first PENDING step file** — find the step with **Status:** PENDING that has no unmet prerequisites. Read its `steps/step-N.md` file. Skip COMPLETED, note BLOCKED.

4. **Mark IN_PROGRESS** — edit the step file's status, update `Last Updated` and the Steps Overview table in the plan overview, persist.

5. **Read the step's Plan section** — understand every bullet, identify target files, note dependencies.

6. **Evidence-before-edit mini-gate:**
    - **Target read:** re-read target files or targeted sections; do not rely on memory
    - **Impact search:** search callers/usages before changing shared functions, types, routes, schemas, config, docs contracts, or reusable patterns; write `N/A — isolated/local change` when not applicable
    - **Validation identified:** discover validation commands, or record `skipped — <reason>` plus residual risk
    - **No guessed facts:** repo claims must be `Verified`, `Assumption`, `Unknown`, or `Recommendation`; only `Verified` claims get cited as facts

7. **Verify Working Set before editing:**
    - Check imports/exports and nearby code for local conventions
    - Verify dependencies/packages/APIs exist before importing
    - Prefer targeted reads/search (`rg`, scoped line ranges) over full raw dumps unless full context is required
    - For aggregate checks (counts, inventories, comparisons), run one short script/command and capture concise output
    - For noisy output: redirect raw output to `/tmp/swe-<task>-<cmd>.<log|json|txt|diff>`, then print only a summary, failure lines, or `head`/`tail` slice
    - Record command evidence as: `Command`, `Exit`, `Output`, `Summary`, `Freshness`
    - Record new evidence in Working Set and Verified Facts

8. **Quality Gate Before Editing:**
    - Fill the step's Quality Checklist as an evidence summary; do not duplicate the full quality docs in the step file
    - Identify and record the local pattern this change should follow
    - Identify the contract: inputs, outputs, errors, side effects
    - Identify the reuse target, or record evidence that none exists
    - Label key claims as Verified / Assumption / Unknown / Recommendation; record evidence for each Verified claim
    - Check applicable `references/code-quality.md` code-smell, test-first, and design-quality prompts for this change path
    - Classify risk with [`references/risk-classification.md`](risk-classification.md) and record mitigation for every non-none risk
    - Before implementation, discover validation commands with [`references/command-discovery.md`](command-discovery.md) when the task will require tests, build, lint, typecheck, or CI-equivalent validation
    - Identify the exact behavior this step will prove with tests or manual verification
    - If any item is unknown, inspect the repo before editing. Do not guess.

9. **Design confirmation gate** (when step touches UI, schema, or API surface):
    - Verify approach matches existing patterns (component library, naming, spacing, response shape)
    - Check the Design Decisions table in plan overview — is this choice already confirmed?
    - If no pattern exists or two reasonable approaches exist, ask the user before implementing
    - Before creating new: search for existing reusable code, record evidence

10. **Protect code before changes:**
    - **Git (preferred):** `git add -A && git commit -m "WIP: before step N"`
    - **Copy snapshot:** if no git, copy files to `snapshots/step-N/`
    - **Tool rules:** 1-20 lines → `edit`; 21+ lines → `edit` with multiple entries; new file → `write`; delete → verify in plan first
    - **Never `write` to "edit" an existing file** — you'll lose content

11. **Implement ONLY what this step requires:**
    - Stay within scope — do not touch unrelated files
    - Use `edit` for surgical changes, `write` for new files
    - Follow existing formatting, naming, import, and comment conventions
    - Handle edge cases and errors consistently with the project
    - If you need a file outside the Working Set, verify it first and add evidence

12. **During Implementation:**
    - Apply [`references/code-quality.md`](code-quality.md)
    - Do not complete the step with unverified imports, packages, or APIs
    - Do not duplicate business logic
    - Do not add a new dependency without approval
    - Do not make a silent contract change
    - Do not perform broad refactors unrelated to the step
    - Do not omit error handling for fallible operations
    - Do not rely on tests that cover only the happy path
    - Reclassify risk if implementation touches new files or contracts

13. **Update evidence:**
     - Add modified files to Working Set with role and evidence
     - Record facts in Verified Facts with tool/read/search proof
     - Keep unverified claims in Assumptions or Implementation Notes until verified
     - Do not record guesses as facts

## Implementation Guidelines

Apply [`references/code-quality.md`](code-quality.md) throughout implementation.

**DO:** surgical changes, follow existing conventions, clear names, avoid duplication when extraction is small and in scope, handle errors consistently, update docs when contracts change.

**DO NOT:** refactor outside scope, add unrequested features/abstractions/dependencies, delete without understanding, formatting-only changes to unrelated files, leave debug statements, silence failures without documenting, copy degraded patterns, guess paths/APIs/callers.

## Scope Changes During Execution

### Out-of-scope finding
| Type | Action |
|------|--------|
| **Direct dependency** (step can't complete without it) | Small + coupled → include, document in Files Changed. Significant → STOP, add prerequisite step, notify user. |
| **Optional improvement** | Note in Implementation Notes as "Future consideration." Do NOT implement. |
| **Separate concern** | Add as new step after current one. Complete current step first. |

### Unplanned work (bug fixes, user adjustments)
After any unplanned change: (1) re-read remaining PENDING steps, (2) check if they're still valid, (3) summarize to user with "Steps completed: X/Y. Remaining: [list]. Still valid?" — do NOT silently continue.

**Plan drift signals:** bug fix took 30+ min, user adjusted 3+ times, remaining steps seem wrong, step took 3× longer than expected → pause, re-assess, communicate.

### Minor vs major findings
- **Minor** (different param name, extra import, file location differs): update step Plan, document in Notes, continue.
- **Major** (library missing feature, API works differently, architecture won't scale): STOP, update Assumptions, review earlier steps, get user confirmation, document deviations.

### Blocker
Mark step `BLOCKED`, document reason + unblock path, proceed to next safe PENDING step or report to user.

## Mandatory Checklist

- [ ] Code changes complete
- [ ] Only files in scope modified
- [ ] Evidence-before-edit mini-gate recorded in plan evidence or notes
- [ ] Quality Gate Before Editing recorded in plan evidence or notes
- [ ] Working Set and Verified Facts updated with evidence
- [ ] Implementation Notes filled in
- [ ] Files Changed list complete
- [ ] Deviations documented
- [ ] Design choices confirmed (if design-sensitive step)

## Next Steps

1. → **verify-step** (validate + test + review-diff)
2. → **persist-plan** (mark COMPLETED, update plan and step status)
3. → **execute-step** (next PENDING) or **global-reflection** (all done)

After `verify-step` passes, compare diff against snapshot from step 10. If unrelated code was modified or deleted, restore and re-implement with smaller edits.
