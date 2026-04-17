# swe-workflow

A lightweight, structured development workflow for AI coding agents. Compatible with **Pi**, **Claude Code**, **Cursor**, **Codex**, **Gemini**, and other agents supporting the Agent Skills spec.

> No code without a plan. One step at a time. Every step validated and tested.

## What It Does

Enforces a disciplined process for any coding task complex enough to warrant it:

1. **Clarify** — understand the request before acting
2. **Plan** — break work into small, ordered steps
3. **Execute** — one step at a time, documented as you go
4. **Verify** — re-read, test, review diff (single gate)
5. **Reflect** — catch complexity before it compounds

State lives in three files under `plans/`, so work survives across sessions and can be handed off between agents.

## Why

Without structure, coding agents tend to:
- Jump straight to code without understanding the request
- Make sweeping changes that break unrelated code
- Skip testing and self-review
- Lose context across sessions

This workflow addresses all of that with a single skill and persistent plan files. See [`SKILL.md`](SKILL.md) for the full agent-facing specification.

## Installation

Via the [Agent Skills CLI](https://skills.sh):

```bash
# Install globally (all supported agents)
npx skills add ex-git/swe-workflow -g

# Install for a specific agent
npx skills add ex-git/swe-workflow -g --agent claude
npx skills add ex-git/swe-workflow -g --agent cursor
npx skills add ex-git/swe-workflow -g --agent pi
npx skills add ex-git/swe-workflow -g --agent codex
npx skills add ex-git/swe-workflow -g --agent gemini
```

### Manual install

Copy the skill directory to your agent's skills location:

```bash
# Pi
cp -r swe-workflow ~/.pi/agent/skills/

# Claude Code
cp -r swe-workflow ~/.claude/skills/

# Cursor
cp -r swe-workflow ~/.cursor/skills/

# Codex
cp -r swe-workflow ~/.codex/skills/

# Gemini
cp -r swe-workflow ~/.gemini/skills/
```

## Structure

```
swe-workflow/
├── SKILL.md                     # Agent-facing entry point (start here)
├── references/                  # Detailed reference documents
│   ├── require-clarification.md
│   ├── create-plan.md
│   ├── resume-workflow.md
│   ├── execute-step.md
│   ├── verify-step.md
│   ├── maintain-repo-map.md
│   ├── persist-plan.md
│   ├── dump-context.md
│   ├── reflect-after-changes.md
│   └── global-reflection.md
├── assets/
│   ├── plan-template.md
│   ├── repo-map-template.md
│   └── context-template.md
├── examples/                    # Worked examples (optional reading)
├── CHANGELOG.md
├── LICENSE
├── package.json
└── README.md
```

## Verification

After installing, test with a vague feature request:

```
Add a function that validates email addresses
```

**Expected behavior:** the agent asks clarifying questions first (scope, input format, error shape) rather than jumping to code.

## Multi-Agent Handoffs

The three `plans/` files are portable across agents. Agent A can complete steps 1–3 and dump context; Agent B reads `plans/context.md` + `plans/<task>.md` + `plans/repo-map.md` and picks up at step 4. Rules and validation gates are the same for both.

## Documentation

- [`SKILL.md`](SKILL.md) — the full agent-facing workflow
- [`CHANGELOG.md`](CHANGELOG.md) — release history
- [`examples/`](examples) — worked example plans

## License

MIT — see [LICENSE](LICENSE).
