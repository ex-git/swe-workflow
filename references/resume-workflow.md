# Resume Workflow

## Overview

Safely resume work from where it was left off. Never restart, never guess — read the plan, context, and repo map.

## When to Use

Use this reference when:
- Starting a new session on existing work
- Resuming after a context switch
- Taking over from another agent
- Unsure what work was already done

## Critical Principle

**Memory is unreliable. Files are truth.**

When resuming, you MUST read three files in order:
1. `plans/context.md` — current session state
2. `plans/<task>.md` — task definition and progress
3. `plans/repo-map.md` — file locations and purposes

Without reading all three, you WILL forget important context.

## Instructions

### Step 1: Discover Existing Plans

```bash
# Check if plans directory exists
ls -la plans/ 2>/dev/null || echo "No plans directory"

# List all plan files with timestamps
ls -lt plans/*.md 2>/dev/null || echo "No plan files found"
```

If no plans exist, start from require-clarification.

### Step 2: Read Context File (MANDATORY)

**This step is mandatory, not optional.**

```
Use read tool on: plans/context.md
```

The context file contains:
- What was being worked on
- Which steps are complete
- Current step progress
- Decisions made
- Active files
- Key learnings (gotchas, patterns)
- Next actions planned

If context file exists, you MUST read it completely.

**If context file doesn't exist:**
- Note that this is a fresh resume
- Proceed to read the plan file

**Checkpoint:** 
- [ ] Context file read (or confirmed absent)
- [ ] Completed steps identified
- [ ] Current step identified
- [ ] Decisions made captured
- [ ] Key learnings captured

### Step 3: Read Plan File (MANDATORY)

```
Use read tool on: plans/<task-name>.md
```

Read ALL sections:
- Goal and Assumptions
- Context & Learnings (decisions, gotchas, patterns)
- Repo Map Quick Reference
- ALL steps (not just current one)
- Implementation Log
- Deviations from Original Plan

**Do NOT rely on memory or partial reads.**

If multiple plans exist, ask the user which one to resume.

**Checkpoint:**
- [ ] Goal understood
- [ ] Assumptions captured
- [ ] All COMPLETED steps noted
- [ ] Current step(s) identified
- [ ] Any BLOCKED steps noted
- [ ] Implementation log reviewed

### Step 4: Read Repo Map (MANDATORY)

**This step is mandatory, not optional.**

```
Use read tool on: plans/repo-map.md
```

The repo map contains:
- All discovered files and their purposes
- Which task touched which files
- Key directories
- Architecture patterns and conventions
- Dependencies

**Why This Matters:**
- Without repo map, you'll waste time searching for files
- You might modify wrong files or forget related files
- You'll miss patterns and conventions discovered earlier
- Future searches are redundant if repo map is current

**Checkpoint:**
- [ ] Core Files table reviewed
- [ ] Related Files table reviewed
- [ ] Key Directories captured
- [ ] Architecture Notes (patterns, conventions) noted
- [ ] File locations for current task identified

### Step 5: Find Resume Point

After reading all three files:

```
Scan for first non-COMPLETED step:
- If IN_PROGRESS: read its Implementation Notes, continue from where stopped
- If PENDING: this is the next step to execute
- If BLOCKED: note reason, try next unblocked step
- If all COMPLETED: proceed to global-reflection
```

Cross-check with context file's "Current Step" to ensure consistency.

### Step 6: Verify Prerequisites

Before executing the step:

1. **Read the step's Prerequisites section**
2. **Check if all prerequisites are met**
3. If prerequisites reference files:
   - Verify files exist in repo map
   - Read those files if needed

### Step 7: Orient User

Tell the user clearly:

```
**Resuming work on: [task name]**

**Files Read:**
- ✓ plans/context.md — session state
- ✓ plans/[task].md — task progress
- ✓ plans/repo-map.md — file inventory

**Progress:**
- Completed: Step X, Y, Z
- Current: Step N — [step title]
- Remaining: [N steps]

**Key Decisions Made:**
- [Decision 1 from context/plan]
- [Decision 2]

**Key Learnings:**
- [Gotcha 1 from context/plan]
- [Pattern discovered]

**Files We're Working With:**
- [Key file 1] — [purpose from repo map]
- [Key file 2] — [purpose]

Should I continue with Step N, or would you like to adjust the plan?
```

### Step 8: Resume Execution

Continue from the identified step:
- Follow normal workflow
- Reference repo map before searching for files
- Reference context for decisions and learnings
- Reference plan for step details

## Mandatory Checklist

Before proceeding with execution, verify ALL of:

| Check | Why It Matters |
|-------|----------------|
| Context file read (or confirmed absent) | Knows where work stopped |
| Plan file read completely | Knows full task scope |
| Repo map read completely | Knows where files are |
| Resume point identified | Knows what to do next |
| Prerequisites verified | Won't fail due to missing deps |
| Decisions and learnings captured | Won't re-litigate settled decisions |
| File locations identified | Won't search for known files |

**If any check fails, STOP and complete it before proceeding.**

## File Priority Order

Read in this exact order: **context → plan → repo-map**. Context is most recent (current state + pointers), plan has full step definitions and learnings, repo-map has file locations so you don't re-search.

## Example Resume

```markdown
# After reading context.md, plans/add-user-auth.md, plans/repo-map.md:

> Found existing work: plans/add-user-auth.md
>
> **Context:** Step 2 of 5 completed. Currently on Step 3: JWT Implementation.
> **Key Decisions:** Using httpOnly cookies for JWT storage (prevents XSS).
> **Key Learnings:** User model needs lean() call for proper JSON serialization.
> **Active Files:**
> - src/utils/jwt.ts (in progress) — JWT generation/validation
> - src/routes/auth.ts (to modify) — Login endpoint
> - src/config/constants.ts (reference) — JWT config
>
> Should I continue with Step 3, or adjust the plan?
```

## Status Interpretation

See [SKILL.md → Status Values](../SKILL.md#status-values). On resume:
- **IN_PROGRESS** — read Implementation Notes, continue from where stopped
- **PENDING** — this is the next step to execute
- **BLOCKED** — note reason, try next unblocked step
- All **COMPLETED** → proceed to `global-reflection`

## Constraints

- **Never proceed without reading all three files** (or confirming context is absent).
- **Never redo completed steps** — trust the plan.
- **Never start from scratch** if a plan exists — ask the user first.
- **Never rely on memory** — use the `read` tool on all files.
- **Always orient the user** with a brief summary before continuing.

## Edge Cases

| Situation | Action |
|---|---|
| Context file missing, plan exists | Read plan completely; treat as fresh resume; create context after first action |
| Context and plan disagree | Trust plan for step status, context for decisions/active work; if they contradict significantly, ask user |
| Multiple plan files | Ask user which to resume |
| Plan looks stale or wrong | Ask user: continue, start fresh, or something else |

## Next Step

→ Proceed to: **execute-step** (implement the next PENDING or IN_PROGRESS step)

**Or if no plan exists:** → Proceed to: **require-clarification**