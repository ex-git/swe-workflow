# Persist Plan

## Overview

The plan file (`plans/<name>.md`) is the single source of truth for a task. Keep it accurate at all times. Every status change, implementation note, and file change must be recorded.

Additionally, when pausing to ask the user if work should continue, dump context to `plans/context.md` so future sessions can resume seamlessly.

**CRITICAL:** This includes verifying repo map synchronization before marking COMPLETED.

## When to Use

- After marking a step IN_PROGRESS (before execution)
- After completing implementation notes (after execution)
- After marking a step COMPLETED or BLOCKED
- Whenever the plan needs to reflect reality
- **Before asking the user "Should I continue?"** — dump context

## Instructions

### After Starting a Step (IN_PROGRESS):

1. Open the plan file with `read` tool
2. Find the current step
3. Use `edit` tool to change status from PENDING to IN_PROGRESS
4. Update "Last Updated" date in header
5. Add entry to Implementation Log if desired

### After Completing a Step (COMPLETED):

1. **Read the plan file** with `read` tool
2. **Read plans/repo-map.md** with `read` tool
3. **Find the current step** in the plan
4. **Update Status:** IN_PROGRESS → COMPLETED
5. **Fill in Implementation Notes:** what was actually done, decisions made
6. **Fill in Files Changed:** exact file paths created or modified
7. **Verify repo map sync** — see "Repo Map Sync Verification" below
8. **Update Context & Learnings:** add any new decisions, gotchas, or patterns discovered
9. **Check off Validation Checklist** items that passed
10. **Check off Test Checklist** items that passed
11. **Update "Last Updated" date**
12. **Add entry to Implementation Log** table

### Repo Map Sync Verification

Before marking a step COMPLETED, verify every file in the step's `Files Changed` is tracked in `plans/repo-map.md` (Core Files if modified, Related Files if only read). If anything is missing, add it first — do not mark the step COMPLETED with a stale repo map.

Full procedure and checklist: [maintain-repo-map](maintain-repo-map.md#verification-checklist).

### After Blocking a Step (BLOCKED):

1. Open the plan file with `read` tool
2. Find the current step
3. Update **Status:** IN_PROGRESS → BLOCKED
4. Add entry to Blocked Steps table with reason
5. Update "Last Updated" date

### When Scope Changes:

If you need to add/modify/remove steps mid-execution:
1. Read the entire plan
2. Use `edit` to insert/modify/remove steps
3. Preserve order (insert in correct dependency sequence)
4. Update any affected Implementation Notes
5. Add Deviations entry explaining the change

### Before Asking User "Should I Continue?":

1. Read `references/dump-context.md` for context dump guidelines
2. Write current state to `plans/context.md` using the template
3. Include: completed steps, current step progress, decisions, active files
4. Verify context file was written
5. Then ask your question to the user

## Status Values

See the Status Values table in [SKILL.md](../SKILL.md#status-values).

## Example Edit

Before:
```markdown
### Step 1: Add validation function

**Status:** PENDING
```

After (use edit tool):
```markdown
### Step 1: Add validation function

**Status:** IN_PROGRESS
```

After completion:
```markdown
### Step 1: Add validation function

**Status:** COMPLETED

**Implementation Notes:**
- Created `validateEmail()` function in `src/utils/validation.ts`
- Used regex pattern for email validation
- Returns `{ valid: boolean, error?: string }`

**Files Changed:**
- src/utils/validation.ts (created)

**Validation Checklist:**
- [x] Function accepts string input
- [x] Returns validation result object
- [x] Handles empty strings

**Test Checklist:**
- [x] Valid email passes
- [x] Invalid email fails
- [x] Empty string handled
```

## Mandatory Updates

**Always update these when marking COMPLETED:**

- [ ] Status changed to COMPLETED
- [ ] Implementation Notes filled (what + why + decisions)
- [ ] Files Changed listed (exact paths)
- [ ] **Repo map verified (all Files Changed are in repo map)**
- [ ] **Repo map updated (any newly discovered files added)**
- [ ] Context & Learnings updated (decisions, gotchas, patterns)
- [ ] Validation Checklist items checked
- [ ] Test Checklist items checked
- [ ] Last Updated date changed
- [ ] Implementation Log entry added

**Always dump context before asking to continue:**

- [ ] `plans/context.md` written
- [ ] Last Updated includes date and time
- [ ] Completed Steps listed
- [ ] Current Step status and progress clear
- [ ] Decisions Made captures key decisions
- [ ] Active Files lists current work
- [ ] Next Actions is ordered and actionable

## Constraints

- **Plan must never drift from reality** — if reality changed, update the plan immediately
- **Repo map must never drift from files** — all touched files must be tracked
- **Never leave IN_PROGRESS across sessions** — complete it or revert to PENDING before ending session
- **Always dump context before pausing** — preserves session for resume
- **Use edit tool for status changes** — don't rewrite the entire plan file
- **Keep repo map synchronized** — update `plans/repo-map.md` whenever files are touched
- If a step is blocked: mark BLOCKED with clear reason, then move to next unblocked step
- When adding steps mid-execution: insert them in dependency order
- Document any deviations from original plan

## Why Repo Map Sync Matters

| Problem | Consequence |
|---------|-------------|
| File not in repo map | Future sessions waste time searching |
| Stale entries | Wrong file locations cause confusion |
| Missing purpose | Context lost about why file matters |
| No task history | Hard to trace what changed when |

**The repo map is the navigation system. Without it, the agent is lost.**

## Next Step

After persisting:

- If more PENDING steps: → Proceed to **execute-step**
- If all steps COMPLETED: → Proceed to **global-reflection**
- If step BLOCKED: → Proceed to next PENDING step (if any)
- If pausing for user: → Context dumped, ready to ask

## Remind User of Progress

After persisting, briefly remind the user of remaining work:

**When resuming (IN_PROGRESS step found):**
> "Continuing with Step X of Y. We have N steps remaining after this one."

**After completing a step (COMPLETED):**
> "Step X complete. Repo map synchronized. Steps remaining: [list brief descriptions]. Context saved. Ready for Step Y?"

**When blocked:**
> "Step X is blocked: [reason]. Skipping to Step Y. [Or: No more steps can proceed.]"

**Before continuing:**
> "I've saved our progress to `plans/context.md`. Should I continue with [next action]?"

This keeps the user oriented on progress and prevents drift.