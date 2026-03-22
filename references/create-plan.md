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

3. **Explore the codebase:**
   - Search for relevant files and directories
   - Note file locations for the Repo Map
   - See `references/maintain-repo-map.md` for guidelines

4. **Break work into ordered steps:**
   - Each step is ONE discrete action (5-15 minutes of work)
   - Steps must be ordered by dependency
   - Each step includes: title, prerequisites, deliverables, plan bullets, validation checklist, test checklist
   - All steps start as **PENDING**

5. **Define prerequisites and deliverables for each step:**
   - **Prerequisites:** What must be true before starting (files exist, previous steps done, dependencies installed)
   - **Deliverables:** What this step produces (files created/modified, functions added, tests passing)
   - This makes steps self-contained and enables new sessions to pick up any step

6. **Read the plan template:**
   - Use `read` tool on: `assets/plan-template.md` (relative to this skill's directory)
   - Understand the required structure before writing

7. **Create the plans directory if needed:**
   - Run: `mkdir -p plans/`

8. **Initialize repo-map.md if it doesn't exist:**
   - Check if `plans/repo-map.md` exists
   - If not, read the template: `assets/repo-map-template.md` (relative to this skill's directory)
   - Create `plans/repo-map.md` using the template structure
   - This file persists across all plans and is shared for the entire project

9. **Derive the plan filename:**
   - Use kebab-case derived from the feature description
   - Examples:
     - "Add user authentication" → `plans/add-user-authentication.md`
     - "Fix memory leak in cache" → `plans/fix-memory-leak-in-cache.md`
     - "Refactor API handlers" → `plans/refactor-api-handlers.md`

10. **Write the plan file:**
   - Use the `write` tool to create `plans/<feature-name>.md`
   - Follow the template structure exactly
   - For Repo Map: include quick reference table only, full map is in `plans/repo-map.md`

11. **Update repo-map.md with discovered files:**
   - Add files discovered during exploration
   - See `references/maintain-repo-map.md` for guidelines
   - This is a project-wide file that accumulates knowledge across tasks

12. **Verify the plan was created:**
   - Use `read` tool to confirm the file exists and has correct structure
   - Ensure all sections are present: Goal, Assumptions, Steps
   - Verify `plans/repo-map.md` exists

## Step Sizing Guide

Good step: "Add user validation function with email format check"
Too big: "Implement the entire authentication system"
Too small: "Import the email regex library"

## Mandatory Outputs

Before marking complete, verify ALL of the following:

- [ ] Plan file exists at `plans/<feature-name>.md`
- [ ] File contains: Goal (one sentence), Assumptions (list), Open Questions (empty)
- [ ] Context & Learnings section initialized (even if empty)
- [ ] Quick Reference table in plan includes key files
- [ ] `plans/repo-map.md` exists with at least one entry
- [ ] Each step has: Title, Status (PENDING), Prerequisites, Deliverables, Plan bullets, Validation checklist, Test checklist
- [ ] At least one step exists (most features need 3-10 steps)
- [ ] Steps are ordered by dependency (earlier steps don't depend on later ones)
- [ ] No code has been written yet

## File Structure

```
plans/
├── repo-map.md           # Project-wide file map (shared across all plans)
├── context.md            # Current session context (created on pause/resume)
└── <feature-name>.md     # Individual plan for this task
```

- **repo-map.md**: One file per project, accumulates file knowledge across all tasks
- **context.md**: One file per project, overwritten each session, preserves current work state
- **plan files**: One file per task/feature, contains steps and task-specific context

## Constraints

- **Do NOT write any code** in this phase — plans only
- **Do NOT skip directly to implementation** — the plan must exist first
- **Do NOT proceed without reading the template** — use the template structure
- **Do NOT embed repo map in plan** — use repo-map.md for all files
- Steps must be small enough to validate individually
- If you can't break it into steps, the scope is too vague — go back to clarification

## Next Step

→ Proceed to: **execute-step** (implement the first PENDING step)