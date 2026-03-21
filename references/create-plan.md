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

5. **Read the plan template:**
   - Use `read` tool on: `assets/plan-template.md` (relative to this skill's directory)
   - Understand the required structure before writing

6. **Create the plans directory if needed:**
   - Run: `mkdir -p plans/`

7. **Derive the plan filename:**
   - Use kebab-case derived from the feature description
   - Examples:
     - "Add user authentication" → `plans/add-user-authentication.md`
     - "Fix memory leak in cache" → `plans/fix-memory-leak-in-cache.md`
     - "Refactor API handlers" → `plans/refactor-api-handlers.md`

8. **Write the plan file:**
   - Use the `write` tool to create `plans/<feature-name>.md`
   - Follow the template structure exactly
   - Populate the Repo Map with files discovered during exploration

9. **Verify the plan was created:**
   - Use `read` tool to confirm the file exists and has correct structure
   - Ensure all sections are present: Goal, Assumptions, Repo Map, Steps

## Step Sizing Guide

Good step: "Add user validation function with email format check"
Too big: "Implement the entire authentication system"
Too small: "Import the email regex library"

## Mandatory Outputs

Before marking complete, verify ALL of the following:

- [ ] Plan file exists at `plans/<feature-name>.md`
- [ ] File contains: Goal (one sentence), Assumptions (list), Open Questions (empty)
- [ ] Context & Learnings section initialized (even if empty)
- [ ] Repo Map populated with discovered files and directories
- [ ] **Repo Map has at least one entry** in Core Files OR Related Files (empty tables are incomplete)
- [ ] Each step has: Title, Status (PENDING), Prerequisites, Deliverables, Plan bullets, Validation checklist, Test checklist
- [ ] At least one step exists (most features need 3-10 steps)
- [ ] Steps are ordered by dependency (earlier steps don't depend on later ones)
- [ ] No code has been written yet

## Constraints

- **Do NOT write any code** in this phase — plans only
- **Do NOT skip directly to implementation** — the plan must exist first
- **Do NOT proceed without reading the template** — use the template structure
- Steps must be small enough to validate individually
- If you can't break it into steps, the scope is too vague — go back to clarification

## Next Step

→ Proceed to: **execute-step** (implement the first PENDING step)