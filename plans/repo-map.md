# Repo Map

> Last Updated: 2026-04-26
> Project: swe-workflow

This file tracks all discovered files for this project. It persists across all plans and tasks, helping future sessions navigate the codebase efficiently.

## Core Files

> Files directly modified by any task. Updated when files are created, modified, or deleted.

| Path | Purpose | Task | Last Updated |
|------|---------|------|--------------|
| `SKILL.md` | Canonical agent-facing workflow and behavioral guards. | require-tests-and-hooks | 2026-04-26 |
| `references/verify-step.md` | Verification gate covering validation, tests, and diff review. | require-tests-and-hooks | 2026-04-26 |
| `CHANGELOG.md` | Release history and user-visible workflow changes. | require-tests-and-hooks | 2026-04-26 |
| `package.json` | Package metadata including published version. | require-tests-and-hooks | 2026-04-26 |

## Related Files

> Files referenced but not modified. Useful for context.

| Path | Why Relevant | Task | Last Updated |
|------|--------------|------|--------------|
| `README.md` | Public overview of workflow behavior and verification expectations. | require-tests-and-hooks | 2026-04-26 |
| `references/execute-step.md` | Describes step execution before verification. | require-tests-and-hooks | 2026-04-26 |
| `assets/plan-template.md` | Required plan structure for Full workflow tasks. | require-tests-and-hooks | 2026-04-26 |
| `assets/repo-map-template.md` | Template used to initialize this repo map. | require-tests-and-hooks | 2026-04-26 |

## Key Directories

> Important directories for understanding project structure.

| Path | Contents | Last Updated |
|------|----------|--------------|
| `references/` | Detailed workflow reference documents. | 2026-04-26 |
| `assets/` | Plan, repo-map, and context templates. | 2026-04-26 |
| `examples/` | Worked examples of plans and workflow artifacts. | 2026-04-26 |

## Architecture Notes

> High-level architectural patterns and conventions discovered across tasks.

### Patterns
- Skill entrypoint plus references: `SKILL.md` holds canonical top-level rules and links to detailed reference files under `references/`.
- Full workflow bookkeeping: broad/source-of-truth updates use `plans/` files before editing task target files.

### Conventions
- Source-of-truth workflow changes are documented in `CHANGELOG.md` under `[Unreleased]`.
- Keep skill guidance concise and imperative so agents can follow it reliably.

### Key Dependencies
- Markdown-only skill package: behavior is implemented through documentation consumed by agents.

## Task History

> Tracks which tasks touched which files. Helps understand file evolution.

| Task | Files Modified | Date |
|------|----------------|------|
| require-tests-and-hooks | `SKILL.md`, `references/verify-step.md`, `CHANGELOG.md`, `package.json` | 2026-04-26 |

---

## How to Update

When discovering new files or directories:
1. Check if entry already exists
2. If new: Add entry with purpose/description
3. If existing: Update Last Updated date and Purpose if clearer
4. Keep descriptions concise and useful for future reference

See `references/maintain-repo-map.md` for detailed guidelines.
