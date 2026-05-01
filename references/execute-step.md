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

5. **Verify the Working Set before editing:**
   - Re-read target files or targeted sections; do not rely on memory
   - Check imports/exports and nearby code to understand local conventions
   - Search callers/usages before changing shared functions, types, routes, schemas, or config contracts
   - Check nearby tests or test conventions for the area being changed
   - Verify dependencies/packages/APIs exist before importing or using them
   - Record new evidence in the plan's Working Set and Verified Facts

6. **Protect code before changes** (safety net against accidental overwrites):

   Choose one strategy, in order of preference:

   - **Git (preferred):** commit or stash the current state so you can `git diff` and roll back:
     ```bash
     git add -A && git commit -m "WIP: before step N"   # checkpoint
     # or
     git add -A && git stash push -m "before step N"
     ```
   - **Copy snapshot:** if git isn't available, copy files you'll modify into `snapshots/step-N/`.
   - **Read and note:** as a last resort, `read` the file and keep content in context.

   **Rules of thumb:**

   | Change size | Tool to use |
   |---|---|
   | 1–20 lines | `edit` tool (surgical) |
   | 21+ lines in an existing file | `edit` with multiple entries; only `write` after verifying against snapshot |
   | New file | `write` tool |
   | Delete file | Verify it's in the plan, then delete |

   **Never use `write` to "edit" an existing file** — you'll lose anything not in your new version. Prefer surgical edits.

7. **Implement ONLY what this step requires:**
   - Write the code/config/docs specified
   - Stay within scope — do not touch unrelated files
   - Do not "while I'm here" refactor other code
   - Use `edit` tool for surgical changes, `write` for new files
   - If you need to access a file outside the Working Set, verify it first and add the evidence to the plan

8. **Update task-local evidence as you discover facts:**
   - Add modified files to the Working Set with their role and evidence
   - Record implementation-relevant facts in Verified Facts with the tool/read/search used to prove them
   - Update advisory repo map only for durable project discoveries (see [maintain-repo-map](maintain-repo-map.md))
   - Do not record guesses as facts

9. **Document what you did:**
   - Fill in **Implementation Notes** — what was done, why, any decisions
   - List all **Files Changed** with exact paths
   - Note any **deviations** from the original plan
   - Use persist-plan to update the plan file

## Implementation Guidelines

### DO:
- Make surgical, minimal changes
- Follow existing formatting, naming, import, and comment conventions
- Write self-documenting, cohesive code with clear names
- Avoid duplication in touched code when extraction is small and in scope
- Handle edge cases and errors consistently with the project
- Update related documentation when the behavior or contract changes

### DO NOT:
- Refactor code outside the step's scope
- Add features, abstractions, utilities, dependencies, or cleanup "while you're here"
- Delete code without understanding its purpose
- Make formatting-only changes to unrelated code
- Leave debugging statements (console.log, print)
- Silence lint/type/test failures without understanding and documenting why
- Copy degraded correctness patterns just because surrounding code has them
- Guess paths, imports, packages, APIs, or callers — verify them first

## Constraints

- **ONE step at a time** — never batch multiple steps
- **Do not touch files outside the step's scope**
- **Do not edit based on guesses** — verify files, imports, dependencies, and callers first
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
   - **Remind user of remaining progress:**
     - "We've completed X of Y steps."
     - "Steps remaining: [brief list]"
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
- [ ] Working Set and Verified Facts reflect any new files or facts used
- [ ] Implementation Notes are filled in
- [ ] Files Changed list is complete
- [ ] Any deviations are documented
- [ ] Advisory repo map updated only if durable project discoveries were made

## Next Steps

After implementation:

1. → Proceed to: **verify-step** (validate + test + review-diff in one gate)
2. → Then: **persist-plan** (mark COMPLETED)
3. → Then: **execute-step** (next PENDING step) or **global-reflection** (if all done)

After `verify-step` passes, compare final diff against the snapshot from step 6. If unrelated code was modified or deleted, restore from the snapshot and re-implement with smaller edits.