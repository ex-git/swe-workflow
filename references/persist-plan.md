# Persist Plan

## Overview

The plan file (`plans/<name>.md`) is the single source of truth. Keep it accurate at all times. Every status change, implementation note, and file change must be recorded.

## When to Use

- After marking a step IN_PROGRESS (before execution)
- After completing implementation notes (after execution)
- After marking a step COMPLETED or BLOCKED
- Whenever the plan needs to reflect reality

## Instructions

### After Starting a Step (IN_PROGRESS):

1. Open the plan file with `read` tool
2. Find the current step
3. Use `edit` tool to change status from PENDING to IN_PROGRESS
4. Update "Last Updated" date in header
5. Add entry to Implementation Log if desired

### After Completing a Step (COMPLETED):

1. Open the plan file with `read` tool
2. Find the current step
3. Update **Status**: IN_PROGRESS → COMPLETED
4. Fill in **Implementation Notes**: what was actually done, decisions made
5. Fill in **Files Changed**: exact file paths created or modified
6. **Update Repo Map**: add any newly discovered files to Core Files or Related Files
7. **Update Context & Learnings**: add any new decisions, gotchas, or patterns discovered
8. Check off **Validation Checklist** items that passed
9. Check off **Test Checklist** items that passed
10. Update "Last Updated" date
11. Add entry to Implementation Log table

### After Blocking a Step (BLOCKED):

1. Open the plan file with `read` tool
2. Find the current step
3. Update **Status**: IN_PROGRESS → BLOCKED
4. Add entry to Blocked Steps table with reason
5. Update "Last Updated" date

### When Scope Changes:

If you need to add/modify/remove steps mid-execution:
1. Read the entire plan
2. Use `edit` to insert/modify/remove steps
3. Preserve order (insert in correct dependency sequence)
4. Update any affected Implementation Notes
5. Add Deviations entry explaining the change

## Status Values

| Status | Meaning | Next Action |
|--------|---------|-------------|
| PENDING | Not started | Wait until earlier steps complete |
| IN_PROGRESS | Currently being worked on | Complete or block the step |
| COMPLETED | Done, validated, tested | Move to next PENDING step |
| BLOCKED | Cannot proceed | Document reason, try next step |

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

**Always update these fields when marking COMPLETED:**

- [ ] Status changed to COMPLETED
- [ ] Implementation Notes filled (what + why + decisions)
- [ ] Files Changed listed (exact paths)
- [ ] Repo Map updated (any newly discovered files)
- [ ] Context & Learnings updated (decisions, gotchas, patterns)
- [ ] Validation Checklist items checked
- [ ] Test Checklist items checked
- [ ] Last Updated date changed
- [ ] Implementation Log entry added

## Constraints

- **Plan must never drift from reality** — if reality changed, update the plan immediately
- **Never leave IN_PROGRESS across sessions** — complete it or revert to PENDING before ending session
- **Use edit tool for status changes** — don't rewrite the entire plan file
- If a step is blocked: mark BLOCKED with clear reason, then move to next unblocked step
- When adding steps mid-execution: insert them in dependency order
- Document any deviations from original plan

## Next Step

After persisting:

- If more PENDING steps: → Proceed to **execute-step**
- If all steps COMPLETED: → Proceed to **global-reflection**
- If step BLOCKED: → Proceed to next PENDING step (if any)

## Remind User of Progress

After persisting, briefly remind the user of remaining work:

**When resuming (IN_PROGRESS step found):**
> "Continuing with Step X of Y. We have N steps remaining after this one."

**After completing a step (COMPLETED):**
> "Step X complete. Steps remaining: [list brief descriptions]. Ready for Step Y?"

**When blocked:**
> "Step X is blocked: [reason]. Skipping to Step Y. [Or: No more steps can proceed.]"

This keeps the user oriented on progress and prevents drift.