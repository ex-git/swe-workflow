# Resume Workflow

## Overview

Safely resume work from where it was left off. Never restart, never guess — read the plan.

## When to Use

Use this reference when:
- Starting a new session on existing work
- Resuming after a context switch
- Taking over from another agent
- Unsure what work was already done

## Instructions

1. **Discover existing plans:**
   - Check if `plans/` directory exists
   - List all files in `plans/` directory
   - If no directory or no files: start from require-clarification

2. **Select the correct plan:**
   - If ONE plan exists: use it
   - If MULTIPLE plans exist:
     - List all plan files to the user
     - Use the most recently modified file (check file timestamps)
     - Or ask user which plan to resume
   - If NO plans exist: start workflow from require-clarification

3. **Read the plan file completely:**
   - Use `read` tool on the selected plan
   - Read ALL sections: Goal, Assumptions, Steps, Implementation Log
   - Do NOT rely on memory or partial reads

4. **Find the resume point:**
   - Scan for first non-COMPLETED step
   - If a step is IN_PROGRESS: read its Implementation Notes
   - If all steps are COMPLETED: proceed to global-reflection
   - Note any BLOCKED steps and their reasons

5. **Review context:**
   - Re-read Goal and Assumptions sections
   - **Read Context & Learnings** — capture decisions, gotchas, patterns from previous steps
   - Check Deviations from Original Plan
   - Review recent Implementation Notes
   - Review Repo Map for file locations discovered so far
   - Understand Files Changed in previous steps

6. **Remind user of remaining work:**
   - State how many steps remain (e.g., "Step 4 of 10 - 6 steps remaining")
   - Briefly mention what the remaining steps cover
   - Ask: "Should I continue with Step X, or would you like to adjust the plan?"

7. **Resume execution:**
   - Continue from the identified step
   - **Read the step's Prerequisites** — verify all prerequisites are met
   - **Read the step's Deliverables** — understand what this step should produce
   - Check Repo Map for file locations before starting
   - Follow normal workflow: execute-step → validate-step → enforce-tests → review-diff → maintain-repo-map → persist-plan

## Plan Discovery Commands

```bash
# Check if plans directory exists
ls -la plans/ 2>/dev/null || echo "No plans directory"

# List all plan files with timestamps
ls -lt plans/*.md 2>/dev/null || echo "No plan files found"

# Find most recently modified plan
ls -t plans/*.md 2>/dev/null | head -1
```

## Mandatory Checklist

Before proceeding with execution:

- [ ] Plan file has been read completely (not from memory)
- [ ] Resume point (next PENDING or IN_PROGRESS step) is identified
- [ ] Context (Goal, Assumptions, recent changes) is understood
- [ ] Any BLOCKED steps are noted

## Status Interpretation

| Status | Meaning | Action |
|--------|---------|--------|
| PENDING | Not started | Execute this step |
| IN_PROGRESS | Partially done | Read notes, continue from where stopped |
| COMPLETED | Done, validated | Move to next step |
| BLOCKED | Cannot proceed | Note reason, try next unblocked step |

## Constraints

- **NEVER redo completed steps** — trust the plan
- **NEVER start from scratch** if a plan exists
- **Always read the full plan** before taking any action
- **Always use read tool** — don't rely on memory
- If the plan seems stale, wrong, or outdated: **flag it to the user** — don't silently diverge
- If IN_PROGRESS step has partial work: continue from where it stopped, don't restart

## Edge Cases

### Plan Exists But Seems Wrong
Ask user: "I found an existing plan at `plans/X.md`, but it seems [stale/wrong/incomplete]. Should I:
1. Continue from this plan
2. Start fresh with require-clarification
3. Something else?"

### Multiple Plans with No Clear Winner
Present options to user: "I found multiple plan files. Which should I resume?
- plans/add-user-auth.md (modified 2024-01-15)
- plans/fix-login-bug.md (modified 2024-01-10)
- Or start a new plan?"

## Next Step

→ Proceed to: **execute-step** (implement the next PENDING/IN_PROGRESS step)

**Or if no plan exists:** → Proceed to: **require-clarification**