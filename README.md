# swe-workflow

A lightweight, structured development workflow for coding agents. Compatible with **Pi**, **Claude Code**, **Cursor**, **Codex**, **Gemini**, and **40+ agents** supporting the Agent Skills spec.

> No code without a plan. One step at a time. Every step validated and tested.

## What Is This?

A single skill that enforces a disciplined development process for AI coding agents:

1. **Clarify** — understand the request before acting
2. **Plan** — break work into small, ordered steps
3. **Execute** — one step at a time, with full documentation
4. **Validate** — re-read files, check correctness, run tests
5. **Reflect** — catch complexity before it compounds

## Why?

Without structure, coding agents:
- Jump straight to code without understanding the request
- Make sweeping changes that break existing code
- Skip testing and validation
- Lose context across sessions
- Can't hand off work to another agent

This workflow fixes all of that with a structured skill and persistent plan files.

## File Structure

```
plans/
├── repo-map.md      # Project-wide file inventory (shared across all plans)
├── context.md       # Current session state (overwritten each pause)
└── <task>.md        # Task-specific plan and progress (one per feature)
```

| File | Purpose |
|------|---------|
| `repo-map.md` | All discovered files, patterns, and conventions for the project |
| `context.md` | Session state — progress, decisions, active files |
| `<task>.md` | Task definition, steps, and progress for a specific feature |

## Skill Structure

Per [Agent Skills spec](https://github.com/agentskills/agentskills):

```
swe-workflow/
├── SKILL.md                     # Entry point with metadata
├── references/                  # Detailed reference documents
│   ├── require-clarification.md
│   ├── create-plan.md
│   ├── execute-step.md
│   ├── resume-workflow.md
│   ├── persist-plan.md
│   ├── dump-context.md
│   ├── validate-step.md
│   ├── enforce-tests.md
│   ├── review-diff.md
│   ├── protect-code.md
│   ├── maintain-repo-map.md
│   ├── reflect-after-changes.md
│   └── global-reflection.md
├── assets/
│   ├── plan-template.md         # Plan file template
│   ├── repo-map-template.md     # Repo map template
│   └── context-template.md      # Context dump template
└── package.json
```

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
| [maintain-repo-map](references/maintain-repo-map.md) | Track file locations | During planning & execution |
| [reflect-after-changes](references/reflect-after-changes.md) | Check complexity | Every 2-3 steps |
| [global-reflection](references/global-reflection.md) | Final review | All steps complete |

## Workflow

```
  ┌─────────────────────────────────────┐
  │  1. require-clarification           │
  │  2. create-plan                     │
  │     └─ init plans/repo-map.md       │
  └──────────────┬──────────────────────┘
                 │
  ┌──────────────▼──────────────────────┐
  │  For each step:                     │
  │    3. execute-step                  │
  │    4. validate-step                 │
  │    5. enforce-tests                 │
  │    6. review-diff                   │
  │    7. update plans/repo-map.md      │
  │    8. persist-plan                  │
  │       └─ dump context if pausing    │
  │                                     │
  │  Every 2-3 steps:                   │
  │    reflect-after-changes            │
  └──────────────┬──────────────────────┘
                 │
  ┌──────────────▼──────────────────────┐
  │  global-reflection                  │
  └─────────────────────────────────────┘
```

## Context Preservation

### Why Dump Context?

When the agent pauses to ask "Should I continue?", it preserves state in `plans/context.md`:

- Completed steps and files changed
- Current step progress and decisions
- Key learnings (gotchas, patterns)
- Open questions and next actions

This enables seamless resumption in new sessions.

### Why Read Context Before Resuming?

| Without Context | With Context |
|-----------------|--------------|
| Don't know what was done | Know exactly where work stopped |
| May redo completed work | Continue from right place |
| Forget decisions made | Respect earlier decisions |
| Miss learnings | Apply discovered patterns |

### Why Read Repo Map?

| Without Repo Map | With Repo Map |
|------------------|---------------|
| Search for files randomly | Go directly to known locations |
| Miss related files | Know what files exist |
| Forget patterns | Apply discovered conventions |
| Duplicate discoveries | Build on previous knowledge |

### When Context is Dumped

- Before asking "Should I continue?"
- After completing 2-3 steps
- When significant decisions are made
- Before ending a session

## Repo Map

The `plans/repo-map.md` file tracks all discovered files:

- **Core Files**: Files directly modified by any task
- **Related Files**: Files referenced for context
- **Key Directories**: Important project directories
- **Architecture Notes**: Patterns and conventions
- **Task History**: Which tasks touched which files

This prevents repeated searches and preserves file knowledge across tasks.

### Repo Map is Enforced

**The repo map must be synchronized before a step can complete.**

The validation gate in `validate-step` explicitly checks:
1. All files in step's "Files Changed" are in the repo map
2. New files are added to Core Files table
3. Referenced files are in Related Files table

If files are missing, validation fails and the agent must add them before proceeding.

## Installation

### Quick Install (Recommended)

Install via [Agent Skills CLI](https://skills.sh):

```bash
# Install globally (all agents)
npx skills add ex-git/swe-workflow -g

# Install for specific agent
npx skills add ex-git/swe-workflow -g --agent claude
npx skills add ex-git/swe-workflow -g --agent cursor
npx skills add ex-git/swe-workflow -g --agent pi
npx skills add ex-git/swe-workflow -g --agent codex
npx skills add ex-git/swe-workflow -g --agent gemini
```

### Manual Install

Copy the skill directory to your agent's skills location:

```bash
# Pi
cp -r skills/swe-workflow ~/.pi/agent/skills/

# Claude Code
cp -r skills/swe-workflow ~/.claude/skills/

# Cursor
cp -r skills/swe-workflow ~/.cursor/skills/

# Codex
cp -r skills/swe-workflow ~/.codex/skills/

# Gemini
cp -r skills/swe-workflow ~/.gemini/skills/
```

## Multi-Agent Support

The plan files enable multi-agent handoffs:

- Agent A creates the plan, completes steps 1-3, dumps context
- Agent B reads `plans/context.md`, picks up from step 4 via `resume-workflow`
- Both agents follow the same rules, same validation
- Repo map persists across all agents and tasks

### Resume Protocol

When resuming work, agents MUST read three files in order:

1. **`plans/context.md`** — Where work stopped, decisions made, active files
2. **`plans/<task>.md`** — Full task definition and step progress
3. **`plans/repo-map.md`** — File locations and architecture notes

This ensures no context is lost and agents don't forget earlier decisions.

## Verification

After installation, test with:

```
Add a function that validates email addresses
```

**Expected behavior:** The agent should ask clarifying questions FIRST, not jump to writing code.

## License

MIT