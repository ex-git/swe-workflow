# Maintain Repo Map

## Overview

Keep the Repo Map in the plan file up-to-date as you discover files during planning and execution. This prevents redundant searches and helps future agents (or yourself in new sessions) navigate the codebase efficiently.

## Why Maintain a Repo Map?

- **Avoid repeated searches** — reference the map instead of searching again
- **Preserve context** — knowledge persists across sessions
- **Speed up handoffs** — next agent doesn't start from zero
- **Document discoveries** — file purposes are recorded, not just paths

## When to Update

### During Planning

After exploring the codebase to understand structure:
- Add directories that contain relevant code
- Add files that will likely be modified
- Add files that are related but won't be modified

### During Execution

When you discover new files:
- Files you needed to read to understand context
- Files you had to modify that weren't in original scope
- Important configuration or dependency files

When file purpose becomes clearer:
- Update the "Purpose" or "Why Relevant" column
- Add notes about key functions or patterns

## What to Include

### Core Files (Files directly modified)

Include:
- Files you're implementing or modifying
- Files you created
- Configuration files you changed

### Related Files (Files referenced but not modified)

Include:
- Files you read for context
- Files with related functionality
- Files that import/export from core files
- Test files related to your changes

### Key Directories

Include:
- Directories containing your core files
- Important project directories (src/, lib/, tests/, etc.)
- Directories with relevant configuration

## What to Skip

Do NOT include:
- `node_modules/`, `vendor/`, or other dependency directories
- Build output directories (`dist/`, `build/`, `.next/`)
- Temporary files or caches
- Files with obvious purpose from name alone (e.g., `README.md`)
- Files you never touched or referenced

## How to Update

### Adding a Core File

```markdown
| Path | Purpose | Last Updated |
|------|---------|--------------|
| `src/auth/login.ts` | Login flow with JWT handling | 2024-01-15 |
```

### Adding a Related File

```markdown
| Path | Why Relevant | Last Updated |
|------|--------------|--------------|
| `src/utils/validation.ts` | Email validation used by login | 2024-01-15 |
```

### Adding a Directory

```markdown
| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/api/` | REST endpoints for user features | 2024-01-15 |
```

### Updating Purpose

If you discover more about a file:
- Update the Purpose/Why Relevant column
- Update the Last Updated date
- Previous info is preserved; new info is additive

## Format Guidelines

- **Paths:** Use relative paths from project root, no leading `./`
- **Purpose/Why Relevant:** One line, be specific
- **Last Updated:** ISO date format (YYYY-MM-DD)
- **No duplicates:** One entry per file/directory

## Using the Repo Map During Execution

Before searching for files:
1. Check the Repo Map first
2. If the file is there, use the path directly
3. If not found, search, then add to map

## Example

Before:
```markdown
### Core Files

| Path | Purpose | Last Updated |
|------|---------|--------------|

### Related Files

| Path | Why Relevant | Last Updated |
|------|--------------|--------------|

### Key Directories

| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/` | Main source code | 2024-01-15 |
```

After exploring and implementing:
```markdown
### Core Files

| Path | Purpose | Last Updated |
|------|---------|--------------|
| `src/auth/login.ts` | Login flow with JWT handling | 2024-01-15 |
| `src/auth/middleware.ts` | Auth middleware for protected routes | 2024-01-15 |

### Related Files

| Path | Why Relevant | Last Updated |
|------|--------------|--------------|
| `src/utils/validation.ts` | Email validation used by login | 2024-01-15 |
| `src/config/constants.ts` | JWT secret and expiry constants | 2024-01-15 |
| `tests/auth/login.test.ts` | Existing tests for login flow | 2024-01-15 |

### Key Directories

| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/` | Main source code | 2024-01-15 |
| `src/auth/` | Authentication modules | 2024-01-15 |
| `tests/` | Test files mirroring src structure | 2024-01-15 |
```

## Global Repo Map

For ongoing projects, consider maintaining a global map:

```
plans/repo-map.md
```

This file accumulates knowledge across multiple tasks:
- Project-wide directory structure
- Common files frequently referenced
- Key architectural patterns discovered

Update it when you discover something that future tasks might benefit from.

## Mandatory Outputs

Before completing Repo Map updates, verify:

- [ ] Core Files table updated if any files were modified/created
- [ ] Related Files table updated if any files were read for context
- [ ] Key Directories updated if new directories discovered
- [ ] All entries have Purpose/Why Relevant filled in (not empty)
- [ ] All entries have Last Updated date
- [ ] No duplicate entries exist

---