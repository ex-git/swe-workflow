# Agent Instructions

This repository contains the `swe-workflow` skill. `SKILL.md` is the canonical source of truth; follow it before modifying code, docs, config, or workflow files.

## Mandatory Workflow Triage

For every code-related task, the first assistant response MUST include:

```text
Workflow mode: Lightweight | Full
Reason: <one sentence>
Success criteria:
- <what done means>
Plan needed: yes | no
```

## When Full Workflow Is Required

Use Full workflow mode before editing for:

- repo-wide scans, migrations, or cleanup
- lint/type/build/test cleanup
- broad refactors or behavior-preserving rewrites touching multiple files
- deleting, moving, or renaming files
- backend + frontend/UI changes in one task
- API/schema/route/tooling/configuration contract changes
- docs/source-of-truth updates, including `SKILL.md`, `README.md`, `CHANGELOG.md`, or this file
- any task expected to touch more than 3 files
- ambiguous scope or unclear success criteria

If a Lightweight task grows into any trigger above, stop before further edits, declare escalation to Full workflow mode, create/update the plan, and continue from the current verified state.

## Full Workflow Pre-Edit Gate

Before any write/edit/delete that changes task target files in Full workflow mode:

1. a plan file must exist under `plans/`,
2. exactly one current step must be marked `IN_PROGRESS`, and
3. the intended edit must map to that step.

Only workflow bookkeeping files (`plans/*.md`) may be created or updated before this gate is satisfied.

## Change Discipline

- Keep changes surgical and tied to the user request or current plan step.
- Verify each step with the relevant lint/type/build/test/manual check.
- Update `CHANGELOG.md` for user-visible behavior, packaging, or source-of-truth documentation changes.
