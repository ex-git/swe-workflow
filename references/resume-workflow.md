# Resume Workflow

## Overview

Safely resume existing work. Never restart or guess from memory — read the saved files, identify the current step, verify evidence against the current workspace, and then continue.

## When to Use

Use this reference when:

- Starting a new session on existing work
- Resuming after a context switch
- Taking over from another agent
- Unsure what work was already done

## Critical Principle

**Memory is unreliable. Files plus current-workspace verification are truth.**

Read in this priority order:

1. `plans/context.md` — latest session state, if present
2. `plans/<task>.md` — task definition, Working Set, Verified Facts, progress
3. `plans/repo-map.md` — optional advisory project memory, if present/useful

The plan and context are authoritative for the current task. The repo map is advisory and may be stale across branches/worktrees; verify before relying on it.

## Instructions

### Step 1: Discover Existing Plans

```bash
ls -la plans/ 2>/dev/null || echo "No plans directory"
ls -lt plans/*.md 2>/dev/null || echo "No plan files found"
```

If no plans exist, start from require-clarification.

### Step 2: Read Context File First

If `plans/context.md` exists, read it completely.

Capture:

- current task and step
- completed work
- decisions made
- active files
- Working Set / Verified Facts summary
- open questions
- next actions

If context is absent, note that and proceed to the plan.

### Step 3: Read the Plan File Completely

Use `read` on `plans/<task-name>.md`.

Read:

- Goal, Assumptions, Open Questions
- Context & Learnings
- Working Set and Verified Facts
- all steps, not just the current one
- Implementation Log
- Files Changed and Implementation Notes

If multiple plan files could apply, ask the user which one to resume.

### Step 4: Optionally Read Repo Map

Read `plans/repo-map.md` only if it exists and would help orientation.

When using it:

- Treat entries as hints, not facts.
- Verify paths with `ls`, `rg`, `find`, `git ls-files`, or `read` before editing.
- If entries conflict with the current workspace, trust the current workspace and update the task plan's Verified Facts.

### Step 5: Find Resume Point

Scan for the first non-COMPLETED step:

- `IN_PROGRESS` — read Implementation Notes and continue only after verifying current state
- `PENDING` — this is the next step to execute
- `BLOCKED` — note reason; try next unblocked step only if safe
- all `COMPLETED` — proceed to global-reflection

Cross-check with `plans/context.md` if present. If context and plan disagree significantly, ask the user before editing.

### Step 6: Verify Prerequisites and Evidence

Before executing:

1. Read the step's Prerequisites.
2. Confirm required files/facts are in Working Set or Verified Facts.
3. Re-verify any stale or branch-sensitive facts in the current workspace.
4. Read target files or targeted sections before editing.

### Step 7: Orient User

Before continuing, summarize briefly:

```markdown
**Resuming work on:** [task name]

**Files Read:**
- ✓ plans/context.md — [read / absent]
- ✓ plans/[task].md — task progress and evidence
- [optional] plans/repo-map.md — advisory project memory, verified where used

**Progress:**
- Completed: Step X, Y, Z
- Current: Step N — [step title]
- Remaining: [N steps]

**Key Decisions / Facts:**
- [Decision or verified fact]

**Working Set:**
- [file] — [role/evidence]

Should I continue with Step N, or would you like to adjust the plan?
```

### Step 8: Resume Execution

Continue from the identified step using execute-step:

- Keep one step `IN_PROGRESS`
- Stay within the plan
- Verify files/facts before editing
- Update Working Set, Verified Facts, Implementation Notes, and Files Changed as reality changes

## Mandatory Checklist

Before proceeding with edits:

| Check | Why It Matters |
|-------|----------------|
| Context file read or confirmed absent | Knows where work stopped |
| Plan file read completely | Knows full task scope |
| Resume point identified | Knows what to do next |
| Working Set and Verified Facts reviewed | Avoids hallucinated paths/APIs/conventions |
| Prerequisites verified | Won't fail due to missing deps |
| Current workspace checked for target files | Avoids stale branch/worktree assumptions |
| User oriented if resuming interactive work | Prevents drift |

**If any check fails, STOP and complete it before proceeding.**

## Edge Cases

| Situation | Action |
|---|---|
| Context file missing, plan exists | Read plan completely; treat as fresh resume; create context after first action |
| Context and plan disagree | Trust plan for step status, context for recent work; ask user if contradiction affects edits |
| Repo map disagrees with current files | Trust current workspace; update Verified Facts and optionally repo map |
| Multiple plan files | Ask user which to resume |
| Plan looks stale or wrong | Ask user: continue, update plan, start fresh, or something else |

## Constraints

- **Never restart from scratch** if a plan exists without asking.
- **Never redo completed steps** unless user asks or verification proves they are invalid.
- **Never rely on memory** — read saved files and current targets.
- **Never rely on repo-map entries without current-workspace verification.**
- **Always orient the user** with a brief summary before continuing interactive work.

## Next Step

→ Proceed to: **execute-step** (implement the next PENDING or IN_PROGRESS step)

**Or if no plan exists:** → Proceed to: **require-clarification**
