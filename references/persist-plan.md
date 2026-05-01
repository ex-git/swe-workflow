# Persist Plan

## Overview

The plan file (`plans/<name>.md`) is the source of truth for the current task. Keep it accurate after every status change, implementation step, verification result, and scope change.

When pausing to ask the user if work should continue, dump context to `plans/context.md` so future sessions can resume safely.

## When to Use

- After marking a step `IN_PROGRESS`
- After implementation changes are made
- After verification passes or fails
- After marking a step `COMPLETED` or `BLOCKED`
- Whenever the plan no longer matches reality
- Before asking the user whether to continue

## After Starting a Step

1. Read the plan file.
2. Find the current step.
3. Change `**Status:** PENDING` to `**Status:** IN_PROGRESS`.
4. Update `Last Updated`.
5. Add an Implementation Log entry if useful.

## After Completing Implementation

Before verification, update the current step with:

- **Implementation Notes:** what changed, why, decisions, deviations
- **Files Changed:** exact paths created/modified/deleted
- **Working Set:** files/facts used for this step and the evidence used to verify them
- **Verified Facts:** implementation-relevant facts proven by `read`, `rg`, config, tests, or tooling

Do not record guesses as facts. If something was inferred but not verified, label it as an assumption.

## After Verification Passes

Before marking a step `COMPLETED`:

1. Re-read the step and confirm all required fields are populated.
2. Check off passed Validation Checklist items.
3. Check off passed Test Checklist items, or document manual verification / why tests could not run.
4. Confirm introduced issues were fixed, or mark the step `BLOCKED`.
5. Update Context & Learnings with durable decisions, gotchas, and patterns.
6. Update advisory `plans/repo-map.md` only for durable project discoveries worth preserving beyond this task.
7. Change `**Status:** IN_PROGRESS` to `**Status:** COMPLETED`.
8. Update `Last Updated`.
9. Add an Implementation Log row.

## After Blocking a Step

1. Read the plan file.
2. Find the current step.
3. Change `**Status:** IN_PROGRESS` to `**Status:** BLOCKED`.
4. Document the blocker, evidence, and what is needed to unblock.
5. Update `Last Updated`.
6. If another PENDING step can safely proceed, explain why; otherwise report the blocker to the user.

## When Scope Changes

If you need to add, modify, split, or remove steps mid-execution:

1. Stop implementation.
2. Read the entire plan.
3. Update steps in dependency order.
4. Document the change in Implementation Notes or Deviations from Original Plan.
5. Ensure exactly one step is `IN_PROGRESS` before resuming edits.

## Before Asking User "Should I Continue?"

1. Read `references/dump-context.md`.
2. Write current state to `plans/context.md`.
3. Include completed steps, current step progress, decisions, Working Set, Verified Facts, active files, and next actions.
4. Verify the context file was written.
5. Then ask the user.

## Mandatory Updates When Marking COMPLETED

- [ ] Status changed to `COMPLETED`
- [ ] Implementation Notes filled with what/why/decisions
- [ ] Files Changed lists exact paths
- [ ] Working Set and Verified Facts are accurate and evidence-backed
- [ ] Context & Learnings updated for decisions, gotchas, and patterns
- [ ] Validation Checklist items checked or documented
- [ ] Test Checklist items checked or documented
- [ ] Introduced check/lint/test/format failures fixed, or blocker documented
- [ ] Last Updated changed
- [ ] Implementation Log entry added
- [ ] Advisory repo map updated only if durable project discoveries were made

## Constraints

- **Plan must never drift from reality** — update it as soon as reality changes.
- **Never mark COMPLETED with known introduced issues** — fix them or mark `BLOCKED`.
- **Never leave stale evidence** — Working Set and Verified Facts must reflect what was actually verified.
- **Never trust repo-map entries blindly** — verify against the current workspace before relying on them.
- **Never leave IN_PROGRESS across sessions** — complete it, block it, or dump context with exact next action.
- **Use edit tool for status changes** — don't rewrite the entire plan file unless restructuring the plan itself.

## Remind User of Progress

After persisting, briefly orient the user:

**After completing a step:**
> "Step X complete. Verification passed: [checks]. Steps remaining: [brief list]."

**When blocked:**
> "Step X is blocked: [reason]. Evidence: [what was checked]."

**Before continuing:**
> "I've saved progress to `plans/context.md`. Should I continue with [next action]?"

## Next Step

After persisting:

- If more PENDING steps: proceed to **execute-step**
- If all steps COMPLETED: proceed to **global-reflection**
- If step BLOCKED: proceed to the next safe PENDING step or report the blocker
- If pausing for user: context dumped, ready to ask
