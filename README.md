# swe-workflow

A lightweight, structured development workflow for AI coding agents. Compatible with **Pi**, **Claude Code**, **Cursor**, **Codex**, **Gemini**, and other agents supporting the Agent Skills spec.

> Every code task starts with workflow triage. Simple tasks stay lightweight; broad, ambiguous, or multi-file tasks use a persisted plan.

## What It Does

Enforces visible workflow selection plus a disciplined process for any coding task complex enough to warrant it:

1. **Triage** — declare `Lightweight` or `Full` mode before edits
2. **Clarify** — understand the request before acting
3. **Plan** — break Full workflow work into small, ordered steps
4. **Execute** — one step at a time, documented as you go
5. **Verify** — re-read, test, review diff (single gate)
6. **Reflect** — catch complexity before it compounds

In Full workflow mode, state lives in three files under `plans/`, so work survives across sessions and can be handed off between agents.

## Why

Without structure, coding agents tend to:
- Jump straight to code without understanding the request
- Make sweeping changes that break unrelated code
- Skip testing and self-review
- Lose context across sessions

This workflow addresses all of that with a single skill, mandatory mode declaration, and persistent plan files when needed. See [`SKILL.md`](SKILL.md) for the full agent-facing specification.

## Expected Agent Behavior

For every code-related task, the agent must first declare:

```text
Workflow mode: Lightweight | Full
Reason: ...
Success criteria:
- ...
Plan needed: yes | no
```

Full workflow mode is mandatory for broad cleanup/refactor/lint tasks, deletes/moves, backend + UI changes, API/schema/route/tooling/config changes, source-of-truth docs, ambiguous work, or anything expected to touch more than 3 files.

In Full workflow mode, implementation edits must wait until a plan exists, the current step is marked `IN_PROGRESS`, and the edit maps to that step.

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
├── SKILL.md                     # Agent-facing entry point (plan format inlined)
├── references/                  # Supplementary detail for each workflow phase
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
├── examples/                    # Worked examples (optional reading)
├── AGENTS.md                    # Repo-level agent instructions / template
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

**Expected behavior:** the agent declares Full workflow mode, explains why the request is ambiguous, and asks clarifying questions (scope, input format, error shape) rather than jumping to code.

## Multi-Agent Handoffs

The three `plans/` files are portable across agents. Agent A can complete steps 1–3 and dump context; Agent B reads `plans/context.md` + `plans/<task>.md` + `plans/repo-map.md` and picks up at step 4. Rules and validation gates are the same for both.

## Documentation

- [`SKILL.md`](SKILL.md) — the full agent-facing workflow
- [`CHANGELOG.md`](CHANGELOG.md) — release history
- [`examples/`](examples) — worked example plans

## License

MIT — see [LICENSE](LICENSE).
