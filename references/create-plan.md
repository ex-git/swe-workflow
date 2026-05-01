# Create Plan

## Overview

Transform a clarified request into a structured, step-by-step plan. The plan is saved as a file and becomes the single source of truth for execution.

## Prerequisites

Before starting:
- [ ] Clarification is complete (require-clarification finished)
- [ ] No open questions remain
- [ ] You understand the scope, inputs, outputs, and success criteria

## Instructions

1. **Verify clarification is complete** — if any open questions remain, go back to require-clarification

2. **Define the plan header:**
   - **Goal:** one sentence describing what we're building
   - **Assumptions:** what we're taking for granted
   - **Open Questions:** must be empty (otherwise stop here)

3. **Explore the codebase narrowly:**
   - Search for relevant files and directories with targeted `rg`/`find`/`git` commands
   - Read only files needed to plan the task: likely targets, direct callers/callees, nearby tests, and relevant config
   - Record task-local evidence in the plan's Working Set and Verified Facts
   - If `plans/repo-map.md` exists, treat it as advisory project memory; verify entries against the current workspace before relying on them

4. **Break work into ordered steps:**
   - Each step is ONE discrete action (5-15 minutes of work)
   - Steps must be ordered by dependency
   - Each step includes: title, prerequisites, deliverables, plan bullets, validation checklist, test checklist
   - All steps start as **PENDING**

5. **Define prerequisites and deliverables for each step:**
   - **Prerequisites:** What must be true before starting (files exist, previous steps done, dependencies installed)
   - **Deliverables:** What this step produces (files created/modified, functions added, tests passing)
   - This makes steps self-contained and enables new sessions to pick up any step

6. **Create the plans directory if needed:**
   - Run: `mkdir -p plans/`

7. **Use advisory repo-map.md only when helpful:**
   - Check if `plans/repo-map.md` exists when the task would benefit from project memory
   - Do not create or update it as a complete inventory
   - Add to it only for durable project conventions, stable directories, architecture notes, or handoff facts that will help future tasks

8. **Derive the plan filename:**
   - Use kebab-case derived from the feature description
   - Examples:
     - "Add user authentication" → `plans/add-user-authentication.md`
     - "Fix memory leak in cache" → `plans/fix-memory-leak-in-cache.md`
     - "Refactor API handlers" → `plans/refactor-api-handlers.md`

9. **Write the plan file — ALL steps:**
   - Use the `write` tool to create `plans/<feature-name>.md`
   - Copy [`../references/plan-template.md`](plan-template.md) verbatim — do NOT invent your own format
   - The plan file MUST contain every step for the entire task, not just the first phase or batch
   - If the user approved a multi-phase plan discussed in chat, write ALL phases and ALL steps to the file before any execution begins
   - A plan file missing later phases/steps is incomplete and blocks execution

10. **Populate Working Set and Verified Facts:**
   - Add each likely target file to the plan's Working Set with the evidence used to verify it
   - Record facts that affect implementation choices, such as existing test style, dependency availability, import/export patterns, callers, and relevant config
   - Keep this task-local; do not turn the plan into an exhaustive repo inventory

11. **Verify the plan was created:**
   - Use `read` tool to confirm the file exists and has correct structure
   - Ensure all sections are present: Goal, Assumptions, Context & Learnings, Working Set, Verified Facts, Steps
   - Verify every non-trivial assumption is either answered, recorded as an assumption, or backed by evidence

## Step Sizing Guide

Good step: "Add user validation function with email format check"
Too big: "Implement the entire authentication system"
Too small: "Import the email regex library"

**Vertical slices, not horizontal.** Each step should be a thin slice through all layers (types, logic, tests) that is independently verifiable — NOT a horizontal slice of one layer.

```
WRONG (horizontal):              RIGHT (vertical):
  Step 1: Define all types         Step 1: Implement + test feature A
  Step 2: Write all functions       Step 2: Implement + test feature B
  Step 3: Write all tests           Step 3: Implement + test feature C
```

A completed step should be demoable or verifiable on its own.

## Mandatory Outputs

Before marking complete, verify ALL of the following:

- [ ] Plan file exists at `plans/<feature-name>.md`
- [ ] File contains: Goal (one sentence), Assumptions (list), Open Questions (empty)
- [ ] Context & Learnings section initialized, including Working Set and Verified Facts
- [ ] Working Set lists key files for the task with evidence, not guesses
- [ ] Verified Facts records implementation-relevant facts with read/search/tool evidence
- [ ] Each step has: Title, Status (PENDING), Prerequisites, Deliverables, Plan bullets, Validation checklist, Test checklist
- [ ] At least one step exists (most features need 3-10 steps)
- [ ] ALL steps for ALL phases of the task are present — no phase or step discussed with the user is omitted
- [ ] Steps are ordered by dependency (earlier steps don't depend on later ones)
- [ ] No code has been written yet

## File Structure

```
plans/
├── repo-map.md           # Optional advisory project memory; verify before use
├── context.md            # Current session context (created on pause/resume)
└── <feature-name>.md     # Individual plan with Working Set and Verified Facts
```

- **repo-map.md**: Optional advisory memory for durable project conventions, stable directories, and gotchas; not authoritative across branches/worktrees
- **context.md**: One file per project, overwritten each session, preserves current work state
- **plan files**: One file per task/feature, contains steps, Working Set, Verified Facts, and task-specific context

## Constraints

- **Do NOT write any code** in this phase — plans only
- **Do NOT skip directly to implementation** — the plan must exist first
- **Do NOT write a partial plan** — the plan file must contain ALL steps for ALL phases of the task; never write only the current or next phase
- **Do NOT invent your own plan format** — copy `references/plan-template.md` verbatim
- **Do NOT create an exhaustive repo inventory** — keep Working Set limited to files/facts needed for this task
- Steps must be small enough to validate individually
- If you can't break it into steps, the scope is too vague — go back to clarification

### Common Scope Traps

Before writing the plan, grep the repo for every occurrence of any value that is meant to stay consistent across files. Missing even one causes silent drift.

- **Version bumps:** grep for the current version string (e.g. `1.5.1`) across `SKILL.md`, `package.json`, `CHANGELOG.md`, `README.md`, and any other config/docs. Every hit is a file the plan must touch.
- **Renames (symbols, routes, env vars, file paths):** grep for the old name; include every caller in the plan's Files Changed.
- **Schema/column changes:** grep for the column name in migrations, models, raw SQL, and type definitions.
- **Config keys:** grep for the key in `.env.example`, `config.*`, docs, and tests.

If the grep surfaces unexpected hits, either fold them into the plan or explicitly call them out in Assumptions as intentionally out of scope.

## Next Step

→ Proceed to: **execute-step** (implement the first PENDING step)