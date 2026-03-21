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

5. **Check Repo Map for target files:**
   - Before implementing, check which files the step will affect
   - Verify each file is in the Repo Map (Core Files or Related Files)
   - If a file is NOT in Repo Map → add it first (see [maintain-repo-map](references/maintain-repo-map.md))
   - This prevents random file scanning and ensures you're working with known files

6. **Protect code before changes:**
   - See protect-code reference for details
   - Take snapshot or note current state of files to be modified

7. **Implement ONLY what this step requires:**
   - Write the code/config/docs specified
   - Stay within scope — do not touch unrelated files
   - Do not "while I'm here" refactor other code
   - Use `edit` tool for surgical changes, `write` for new files
   - If you need to access a file not in Repo Map → stop and add it first

8. **Update the Repo Map as you discover files:**
   - See [maintain-repo-map](references/maintain-repo-map.md) for detailed guidelines
   - Check Repo Map before searching for files
   - Add newly discovered files to appropriate section (Core Files or Related Files)
   - Update file purposes as you learn more

9. **Document what you did:**
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
- Touch files not in Repo Map without adding them first
- Scan files randomly — check Repo Map first

## Constraints

- **ONE step at a time** — never batch multiple steps
- **Do not touch files outside the step's scope**
- **Do not touch files not in Repo Map** — add them first
- **Check Repo Map before scanning for files** — prefer known locations over random search
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

## When Unplanned Work Happens

If you fix a bug, make user-requested adjustments, or address issues found during review:

1. **Pause and reassess:**
   - Did this work change the scope significantly?
   - Are remaining steps still valid?
   - Has the implementation drifted from the original plan?

2. **Re-check the plan:**
   - Read through remaining PENDING steps
   - Verify each step still makes sense in context
   - Check if any steps are now redundant or need modification

3. **Communicate with user:**
   - Summarize what was done (bug fix, adjustment)
   - State whether remaining steps are still valid
   - Ask: "Should I continue with the remaining steps, or would you like to adjust the plan?"
   - Do NOT assume you should just continue

4. **If plan needs adjustment:**
   - Update the affected steps
   - Add new steps if needed
   - Document the change in Deviations from Original Plan
   - Get user confirmation before continuing

### Signs of Plan Drift

Watch for these warning signs:

| Sign | What It Means | Action |
|------|---------------|--------|
| Bug fix took 30+ min | Scope expansion | Re-assess remaining steps |
| User adjusted 3+ times | Unclear requirements | Consider re-clarifying |
| Remaining steps seem wrong | Plan outdated | Update plan before continuing |
| "While fixing X, I also did Y" | Scope creep | Document, re-assess plan |
| Step took 3x longer than expected | Underestimated complexity | Check if remaining estimates are valid |

### Good Practice Example

```markdown
**Bad:** "I fixed the bug. Continuing with Step 9..."

**Good:** "I've fixed the login validation bug. This took longer than expected 
because we discovered the auth module needed updates.

Before continuing with Steps 9 and 10, I want to check: 
- Step 9 (add password reset) should still work as planned
- Step 10 (update auth docs) will need to include the bug fix details

Would you like me to continue, or should we adjust the remaining steps?"
```

**Key principle:** After unplanned work, always re-assess before continuing. Never assume the original plan is still valid.

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
- [ ] All modified files were in Repo Map before touching (or added first)
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