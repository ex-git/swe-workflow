# Execute Step

## Overview

Implement exactly one plan step at a time. No more, no less.

## Prerequisites

Before starting:
- [ ] Plan file exists in `plans/`
- [ ] You know which step to execute (from resume-workflow or completing previous step)
- [ ] No step is currently IN_PROGRESS (finish or revert previous steps first)

## Instructions

1. **Read the plan file:**
   - Use `read` tool on `plans/<name>.md`
   - Confirm you have the correct plan

2. **Select the first PENDING step:**
   - Skip any COMPLETED steps
   - Note any BLOCKED steps and why

3. **Mark it IN_PROGRESS:**
   - Use `edit` tool to update the step status
   - Update "Last Updated" date in plan header
   - Use persist-plan to save status

4. **Read the step's Plan section carefully:**
   - Understand every bullet point
   - Identify files that will be affected
   - Note any dependencies on previous steps

5. **Protect code before changes:**
   - See protect-code reference for details
   - Take snapshot or note current state of files to be modified

6. **Implement ONLY what this step requires:**
   - Write the code/config/docs specified
   - Stay within scope — do not touch unrelated files
   - Do not "while I'm here" refactor other code
   - Use `edit` tool for surgical changes, `write` for new files

7. **Update the Repo Map as you discover files:**
   - Check Repo Map before searching for files
   - Add newly discovered files to appropriate section
   - Update file purposes as you learn more
   - See `references/maintain-repo-map.md` for guidelines

8. **Document what you did:**
   - Fill in **Implementation Notes** — what was done, why, any decisions
   - List all **Files Changed** with exact paths
   - Note any **deviations** from the original plan
   - Use persist-plan to update the plan file

## Implementation Guidelines

### DO:
- Make surgical, minimal changes
- Follow existing code patterns and style
- Write self-documenting code with clear names
- Handle edge cases and errors
- Update related documentation

### DO NOT:
- Refactor code outside the step's scope
- Add features "while you're here"
- Delete code without understanding its purpose
- Make formatting-only changes to unrelated code
- Leave debugging statements (console.log, print)

## Constraints

- **ONE step at a time** — never batch multiple steps
- **Do not touch files outside the step's scope**
- **Do not refactor unrelated code** — even if you see issues
- **Do not implement more than specified** — stay within scope
- If you discover the step needs to be split: **update the plan first**, then execute the smaller steps
- If a step turns out to be unnecessary: mark it COMPLETED with a note explaining why

## When New Findings Require Additional Changes

If during execution you discover code changes needed outside the step's scope:

1. **STOP and assess:**
   - Is this a **direct dependency** (step can't complete without it)?
   - Is it an **optional improvement** (nice to have but not required)?
   - Is it a **separate concern** (should be its own step)?

2. **For direct dependencies:**
   - If the change is small and tightly coupled → include it in current step, document in Files Changed
   - If the change is significant → STOP, add as a prerequisite step, notify user
   - Update the step's Plan section to reflect the new scope

3. **For optional improvements:**
   - Note in Implementation Notes as "Future consideration"
   - Do NOT implement now
   - May propose as a new step after current step completes

4. **For separate concerns:**
   - Add as a new step after the current one in the plan
   - Complete current step first
   - Address in proper order

**Key principle:** Never silently expand scope. All changes must be documented in the plan.

## When New Findings Change What We Know

### Minor findings → Update step, continue

Changes that don't affect the overall approach:
- "Need to also update this helper function"
- "Parameter name is slightly different than expected"
- "Need to add an import"
- "File location is different than planned"

**Action:** Update the step's Plan section, document in Implementation Notes, continue.

### Major findings → Pause and reassess

Changes that affect Assumptions or invalidate earlier steps:
- "The library we assumed exists doesn't have this feature"
- "The API works fundamentally differently than documented"
- "This architectural approach won't scale"
- "Previous step's implementation was based on wrong assumption"

**Action:**
1. STOP implementation
2. Update Assumptions in the plan header
3. Review if earlier COMPLETED steps are still valid
4. If earlier steps may be affected → use reflect-after-changes to reassess
5. Get user confirmation if the change is significant
6. Document in "Deviations from Original Plan" section
7. Only then continue execution

## When Step Cannot Be Completed

If you encounter a blocker:
1. Mark step as BLOCKED in the plan
2. Document the reason and what's needed to unblock
3. Proceed to next available PENDING step (if any)
4. If no progress possible, report to user

## Mandatory Checklist

Before marking step as ready for validation:

- [ ] Code changes are complete
- [ ] Only files in scope were modified
- [ ] Implementation Notes are filled in
- [ ] Files Changed list is complete
- [ ] Any deviations are documented
- [ ] Repo Map updated if new files discovered

## Next Steps

After implementation:

1. → Proceed to: **validate-step** (verify correctness)
2. → Then: **enforce-tests** (write/run tests)
3. → Then: **review-diff** (review your changes)
4. → Then: **persist-plan** (mark COMPLETED)
5. → Then: **execute-step** (next PENDING step) or **global-reflection** (if all done)