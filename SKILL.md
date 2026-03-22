---
name: swe-workflow
description: A structured development workflow for coding agents. Use when starting a new feature, working on multi-step tasks, or when you need planning, validation, and quality checks. Follow Understand → Plan → Execute → Reflect phases with built-in safeguards against scope creep and incomplete implementations.
license: MIT
metadata:
  version: "1.1.0"
  author: "Evan Xu"
---

# SWE Workflow

A structured development workflow for coding agents — compatible with Pi, Claude Code, and Cursor.

## Quick Start

| Starting Point | Reference |
|----------------|-----------|
| New task or feature | [require-clarification](references/require-clarification.md) |
| Create plan from clarified request | [create-plan](references/create-plan.md) |
| Resume existing work | [resume-workflow](references/resume-workflow.md) |
| Execute plan step | [execute-step](references/execute-step.md) |

## File Structure

The workflow uses three files to manage context:

```
plans/
├── repo-map.md      # Project-wide file inventory (shared across all plans)
├── context.md       # Current session state (overwritten each pause)
└── <task>.md        # Task-specific plan and progress (one per feature)
```

| File | Scope | When Updated | Purpose |
|------|-------|--------------|---------|
| `repo-map.md` | Project | Whenever files discovered | Navigation, context preservation |
| `context.md` | Session | Before each pause/work complete | Resume capability |
| `<task>.md` | Task | After each step | Task tracking and progress |

## Workflow Phases

### Phase 1: Understand

**Goal:** Ensure the request is fully understood before any work begins.

1. Read [require-clarification](references/require-clarification.md)
2. Analyze scope, inputs, outputs, success criteria
3. Ask targeted questions if ambiguous
4. Get user confirmation before proceeding

### Phase 2: Plan

**Goal:** Create structured plan files as single source of truth.

1. Read [create-plan](references/create-plan.md)
2. Use [assets/plan-template.md](assets/plan-template.md) for plan structure
3. Initialize [assets/repo-map-template.md](assets/repo-map-template.md) in `plans/repo-map.md`
4. Break work into small, ordered steps (5-15 min each)
5. Save plan to `plans/<feature-name>.md`
6. Populate repo-map.md with files discovered during exploration

### Phase 3: Execute

**Goal:** Implement one step at a time with validation gates.

For each step:

1. **Execute:** [execute-step](references/execute-step.md) — Implement the step
2. **Validate:** [validate-step](references/validate-step.md) — Check correctness
3. **Test:** [enforce-tests](references/enforce-tests.md) — Ensure coverage
4. **Review:** [review-diff](references/review-diff.md) — Critical self-review
5. **Update Repo Map:** [maintain-repo-map](references/maintain-repo-map.md) — Add newly discovered files
6. **Persist:** [persist-plan](references/persist-plan.md) — Update plan status and dump context

### Phase 4: Reflect

**Goal:** Catch issues before they compound.

- Every 2-3 steps: [reflect-after-changes](references/reflect-after-changes.md)
- After all steps: [global-reflection](references/global-reflection.md)

## Reference Guide

| Reference | Purpose | When to Use |
|-----------|---------|-------------|
| [require-clarification](references/require-clarification.md) | Clarify ambiguous requests | Before any planning |
| [create-plan](references/create-plan.md) | Create structured plan | After clarification |
| [resume-workflow](references/resume-workflow.md) | Resume existing work | New session, context switch |
| [execute-step](references/execute-step.md) | Implement one step | Ready to code |
| [persist-plan](references/persist-plan.md) | Update plan status | After any progress |
| [dump-context](references/dump-context.md) | Save session state | Before asking user, after steps |
| [validate-step](references/validate-step.md) | Verify correctness | After implementation |
| [enforce-tests](references/enforce-tests.md) | Ensure test coverage | After validation |
| [review-diff](references/review-diff.md) | Self-review changes | Before completing step |
| [protect-code](references/protect-code.md) | Prevent accidental deletion | Before modifying files |
| [maintain-repo-map](references/maintain-repo-map.md) | Track file locations | After create-plan, after each step |
| [reflect-after-changes](references/reflect-after-changes.md) | Catch complexity | Every 2-3 steps |
| [global-reflection](references/global-reflection.md) | Final review | All steps complete |

## Workflow Guards

**Do NOT proceed if:**

| Condition | Required Action |
|-----------|-----------------|
| No plan file exists | Run create-plan |
| Plan has IN_PROGRESS steps | Complete or revert them |
| Clarification has open questions | Run require-clarification |
| Step validation failed | Fix issues first |
| Tests not written/run | Run enforce-tests |
| **Repo map not synchronized** | **Update plans/repo-map.md first** |

**Do NOT skip:**

| Phase | Risk of Skipping |
|-------|------------------|
| Clarification | Wasted work on wrong scope |
| Planning | Lost context, scope creep |
| Validation | Bugs, incomplete implementations |
| Testing | No coverage, future breakage |
| Diff review | Artifacts, scope creep |
| **Repo map update** | **Lost file context, redundant searches** |
| Context dump | Lost session progress |
| Reflection | Accumulating complexity |

## Skill Chain

```
require-clarification → create-plan → maintain-repo-map (initial)
                          ↓
                    execute-step → validate-step → enforce-tests → review-diff → maintain-repo-map → persist-plan
                          ↑                                                                                           |
                          |                                                                                           ↓
                          └─────────────────────────────────── (next step) ←──────────────────────────────────────────┘
                                                          |
                                                          ↓
                                              reflect-after-changes (every 2-3 steps)
                                                          |
                                                          ↓
                                              dump-context (before asking user)
                                                          |
                                                          ↓
                                              global-reflection (when all done)
```

## Context Preservation

### Why Dump Context?

When an agent pauses to ask the user "Should I continue?", it should preserve session state:

- **Resume seamlessly** — Next session picks up exactly where stopped
- **No memory loss** — All decisions, discoveries, and progress preserved
- **Better handoffs** — Another agent can understand current state

### When to Dump Context

- Before asking "Should I continue?"
- After completing 2-3 steps
- When significant decisions have been made
- Before ending a session

### Context File Contents

| Section | Purpose |
|---------|---------|
| Current Task | What we're working on |
| Completed Steps | What's done and files changed |
| Current Step | Progress, decisions, active files |
| Key Learnings | Gotchas, patterns, warnings |
| Open Questions | Waiting for user input |
| Next Actions | Ordered next steps |

See [dump-context](references/dump-context.md) for full guidelines.

## Repo Map

### Why Separate Repo Map?

The repo map is a project-wide file (`plans/repo-map.md`) separate from individual plans:

- **Single source of truth** — All plans reference the same file inventory
- **Accumulates knowledge** — Patterns and conventions discovered in one task benefit future tasks
- **Reduces duplication** — Update once, all plans benefit
- **Prevents repeated searches** — Know where files are without re-exploring

### What's Tracked

| Section | Content |
|---------|---------|
| Core Files | Files directly modified |
| Related Files | Files referenced for context |
| Key Directories | Important project directories |
| Architecture Notes | Patterns, conventions, dependencies |
| Task History | Which tasks touched which files |

See [maintain-repo-map](references/maintain-repo-map.md) for full guidelines.

## Templates

- **Plan Template:** [assets/plan-template.md](assets/plan-template.md)
- **Repo Map Template:** [assets/repo-map-template.md](assets/repo-map-template.md)
- **Context Template:** [assets/context-template.md](assets/context-template.md)

## Status Values

| Status | Meaning |
|--------|---------|
| PENDING | Not started |
| IN_PROGRESS | Currently being worked on |
| COMPLETED | Done, validated, tested |
| BLOCKED | Cannot proceed (reason documented) |

## Constraints

- **One step at a time** — Never batch multiple steps
- **Stay in scope** — Do not touch files outside the step's scope
- **Trust the plan** — Do not redo completed steps
- **Persist reality** — Keep plan file accurate at all times
- **Maintain repo map** — All touched files MUST be in `plans/repo-map.md`
- **Dump context** — Save session state before asking user to continue
- **Validate before complete** — No step passes with known issues
- **No step passes without repo map sync** — Verify all files are tracked

## Session Management

### When to Start a New Session

Consider starting a fresh session per step when:
- Plan has many steps (8+) and context is getting muddy
- Previous step had significant bug fixes or pivots
- Agent keeps referencing old/incorrect context
- Steps are mostly independent

### How Context Persists

| File | What It Captures | Lifetime |
|------|------------------|----------|
| `context.md` | Current session state | Overwritten each pause |
| `<task>.md` | Task definition, all steps | Task lifetime |
| `repo-map.md` | All discovered files | Project lifetime |

### Starting Fresh

1. **Read `plans/context.md`** (MANDATORY if exists) — quick state overview
2. **Read `plans/<task>.md`** (MANDATORY) — full task details and progress
3. **Read `plans/repo-map.md`** (MANDATORY) — file locations and patterns
4. Resume from the identified step

### Mandatory Resume Checklist

| Step | Action | Why |
|------|--------|-----|
| 1 | Read context.md | Know where work stopped |
| 2 | Read plan file | Know full task scope |
| 3 | Read repo-map.md | Know where files are |
| 4 | Verify completed steps | Don't redo work |
| 5 | Identify current step | Know what to do next |
| 6 | Capture decisions/learnings | Don't re-litigate |
| 7 | Note file locations | Don't search for known files |

### Why This Works

- **Isolated steps**: Prerequisites and Deliverables make each step self-contained
- **Preserved learnings**: Context & Learnings capture decisions, not just actions
- **Fresh context**: New session = no accumulated confusion
- **Plan-driven**: Agent reads plan to understand state, not memory
- **Per-file persistence**: repo-map.md survives across tasks, context.md preserves sessions
- **Mandatory reads**: Three-file check ensures no forgotten context