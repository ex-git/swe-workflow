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

This workflow fixes all of that with a structured skill and a persistent plan file.

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
│   ├── validate-step.md
│   ├── enforce-tests.md
│   ├── review-diff.md
│   ├── protect-code.md
│   ├── maintain-repo-map.md
│   ├── reflect-after-changes.md
│   └── global-reflection.md
├── assets/
│   └── plan-template.md         # Plan file template
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
  └──────────────┬──────────────────────┘
                 │
  ┌──────────────▼──────────────────────┐
  │  For each step:                     │
  │    3. execute-step                  │
  │    4. validate-step                 │
  │    5. enforce-tests                 │
  │    6. review-diff                   │
  │    7. mark COMPLETED                │
  │                                     │
  │  Every 2-3 steps:                   │
  │    reflect-after-changes            │
  └──────────────┬──────────────────────┘
                 │
  ┌──────────────▼──────────────────────┐
  │  global-reflection                  │
  └─────────────────────────────────────┘
```

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

The plan file (`plans/<name>.md`) enables multi-agent handoffs:

- Agent A creates the plan, completes steps 1-3
- Agent B reads the plan, picks up from step 4 via `resume-workflow`
- Both agents follow the same rules, same validation

## Verification

After installation, test with:

```
Add a function that validates email addresses
```

**Expected behavior:** The agent should ask clarifying questions FIRST, not jump to writing code.

## License

MIT