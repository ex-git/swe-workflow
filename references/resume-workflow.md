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

When resuming, read files in this EXACT order:

```
1. plans/context.md     → What was being worked on (most recent state)
2. plans/<task>.md      → Full task definition and step progress
3. plans/repo-map.md    → Where files are and what they do
```

**Context is first** because:
- It's the most recent state
- It summarizes what was happening
- It points to relevant files

**Plan is second** because:
- It has full step definitions
- It has all context & learnings
- It has implementation history

**Repo map is third** because:
- It has all file locations
- It has architecture insights
- It prevents redundant searches

## Example Resume

```markdown
# Agent reads context.md

Context shows:
- Completed: Steps 1-2 (user model, password hashing)
- Current: Step 3 (JWT implementation) — IN_PROGRESS
- Decision: Using httpOnly cookies for JWT
- Active files: src/utils/jwt.ts, src/routes/auth.ts
- Next: Complete JWT token generation

# Agent reads plan file

Plan shows:
- Step 3 details: Implement JWT generation/validation
- Prerequisites: bcrypt must be installed ✓
- Context & Learnings: User model needs lean() for JSON
- All 5 steps listed with status

# Agent reads repo-map.md

Repo map shows:
- src/models/User.ts — User schema (Core file, touched in Step 1)
- src/utils/password.ts — Password hashing (Core file, touched in Step 2)
- src/config/constants.ts — JWT secret, expiry (Related file)
- Architecture: Auth middleware pattern: checkToken → validateUser → next

# Agent responds to user

> Found existing work: plans/add-user-auth.md
>
> **Context:** Step 2 of 5 completed. Currently on Step 3: JWT Implementation.
>
> **Key Decisions:** Using httpOnly cookies for JWT storage (prevents XSS).
>
> **Key Learnings:** User model needs lean() call for proper JSON serialization.
>
> **Files:**
> - src/utils/jwt.ts (in progress) — JWT generation/validation
> - src/routes/auth.ts (to be modified) — Login endpoint
> - src/config/constants.ts (reference) — JWT config
>
> Should I continue with Step 3 (JWT Implementation), or adjust the plan?
```

## Why This Rigor?

| Without Reading Context | With Context |
|-------------------------|--------------|
| Don't know what was done | Know exactly where work stopped |
| May redo completed work | Continue from right place |
| Forget decisions made | Respect earlier decisions |
| Miss learnings | Apply discovered patterns |

| Without Reading Repo Map | With Repo Map |
|--------------------------|---------------|
| Search for files randomly | Go directly to known locations |
| Miss related files | Know what files exist |
| Forget patterns | Apply discovered conventions |
| Duplicate discoveries | Build on previous knowledge |

## Status Interpretation

| Status | Meaning | Action |
|--------|---------|--------|
| PENDING | Not started | Execute this step |
| IN_PROGRESS | Partially done | Read notes, continue from where stopped |
| COMPLETED | Done, validated | Move to next step |
| BLOCKED | Cannot proceed | Note reason, try next unblocked step |

## Constraints

- **NEVER proceed without reading context.md** — if it exists, it has critical state
- **NEVER proceed without reading the plan** — it has full task definition
- **NEVER proceed without reading repo-map.md** — it prevents redundant searches
- **NEVER redo completed steps** — trust the plan
- **NEVER start from scratch** if a plan exists
- **NEVER rely on memory** — use read tool on all files
- **Always orient the user** — tell them what you read and where you're resuming

## Edge Cases

### Context File Missing but Plan Exists
- Plan file has step progress but no session state
- Read plan completely, note that context is absent
- Start from identified resume point
- Consider creating context after first action

### Context File Out of Sync with Plan
- Trust plan file for step completion status
- Trust context file for decisions and active work
- If they contradict significantly, ask user which to trust

### Multiple Plans with No Clear Winner
Present options to user:
> "I found multiple plan files. Which should I resume?
> - plans/add-user-auth.md (modified 2024-01-15)
> - plans/fix-login-bug.md (modified 2024-01-10)
> - Or start a new plan?"

### Plan Exists But Seems Wrong
Ask user:
> "I found an existing plan but it seems [stale/wrong/incomplete]. Should I:
> 1. Continue from this plan
> 2. Start fresh with require-clarification
> 3. Something else?"

## Next Step

→ Proceed to: **execute-step** (implement the next PENDING/IN_PROGRESS step)

**Or if no plan exists:** → Proceed to: **require-clarification**