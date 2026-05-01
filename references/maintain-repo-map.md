# Maintain Repo Map

## Overview

`plans/repo-map.md` is **advisory project memory**, not an authoritative inventory. Use it to preserve durable context that helps future tasks, but verify every entry against the current workspace before relying on it.

The task plan's **Working Set**, **Verified Facts**, **Implementation Notes**, and **Files Changed** are the source of truth for the current request.

## When to Use

Use the repo map in Full workflow when it will help future agents understand stable project context:

- important directories or entry points that are likely to stay valid
- durable architecture patterns or conventions
- recurring gotchas that are not obvious from file names
- important cross-cutting files repeatedly relevant across tasks
- handoff notes that apply beyond the current plan

Do **not** use the repo map for Lightweight mode or one-off file listings.

## Branch and Staleness Rules

Repo maps can go stale across branches, worktrees, commits, generated code, or local edits.

Before using any repo-map entry:

1. Verify the path exists with `ls`, `find`, `rg`, `git ls-files`, or `read`.
2. Re-read the file or relevant section before editing.
3. If the entry is stale, update or remove it.
4. Never cite a repo-map entry as a Verified Fact unless you also verified it in the current workspace.

## What to Include

Keep entries sparse and durable:

### Key Files

Files that are repeatedly useful across tasks, such as main entry points, core configuration, routing tables, schema definitions, or shared utilities.

### Key Directories

Directories that explain project layout, such as `src/`, `tests/`, `packages/`, `apps/`, or generated-code locations.

### Architecture Notes

Stable patterns and conventions:

- error handling style
- dependency direction
- module boundaries
- test conventions
- logging/config patterns
- generated-code warnings

### Gotchas

Warnings that prevent future mistakes:

- files that look authoritative but are generated
- branch-specific behavior
- commands that must be run after changes
- conventions not enforced by tooling

## What to Skip

Do NOT include:

- every file read during a task
- every modified file just because it changed
- symbol-level maps, line ranges, or function inventories
- dependency/build output directories (`node_modules/`, `vendor/`, `dist/`, `build/`, `.next/`)
- facts that only apply to the current task and belong in the plan's Working Set or Verified Facts
- unverified paths or guesses

Use structural/navigation tools (`read` structural maps, `rg`, `find`, LSP, ctags, project search) for discovery instead of turning `repo-map.md` into a manual index.

## Suggested Structure

```markdown
# Repo Map

> Last Updated: YYYY-MM-DD
> Project: [name]
> Advisory: verify entries against the current branch/worktree before use.

## Key Files
| Path | Why It Matters | Last Verified |
|------|----------------|---------------|
| `src/index.ts` | Application entry point | 2026-04-30 |

## Key Directories
| Path | Contents | Last Verified |
|------|----------|---------------|
| `tests/` | Test files mirroring source structure | 2026-04-30 |

## Architecture Notes
### Patterns
- [Pattern]: [where it applies]

### Conventions
- [Convention]: [evidence or source]

### Gotchas
- [Warning]: [how to avoid the issue]
```

Existing repo maps with Core Files / Related Files / Task History tables are still valid, but agents should treat them as advisory and avoid expanding them into exhaustive inventories.

## How to Update

When a durable discovery is worth preserving:

1. Read the current `plans/repo-map.md` if it exists.
2. Verify the path/fact in the current workspace.
3. Add or update the smallest useful entry.
4. Record when it was last verified.
5. Keep task-specific evidence in `plans/<task>.md`, not here.

## Verification Checklist

Before relying on or updating the repo map:

- [ ] Entry was verified in the current branch/worktree.
- [ ] Entry is durable beyond the current task.
- [ ] Task-specific details remain in the plan's Working Set or Verified Facts.
- [ ] No exhaustive file inventory or symbol map was added.
- [ ] Stale entries discovered during verification were corrected or noted.

## Relationship to the Plan

| Need | Where it belongs |
|---|---|
| Files changed in this task | Step `Files Changed` |
| Evidence for current implementation choices | Plan `Working Set` and `Verified Facts` |
| Current session handoff | `plans/context.md` |
| Durable project conventions/gotchas | `plans/repo-map.md` |

**Key principle:** repo map reduces repeated orientation work, but it never replaces reading the current files before editing.
