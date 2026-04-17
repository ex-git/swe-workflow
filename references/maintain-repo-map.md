# Maintain Repo Map

## Overview

Keep the Repo Map (`plans/repo-map.md`) up-to-date as you discover files during planning and execution. This prevents redundant searches and helps future agents navigate the codebase efficiently.

## Why a Separate Repo Map?

- **Single source of truth** — One file for all discovered project files
- **Shared across plans** — All plans reference the same map, avoiding duplication
- **Accumulates knowledge** — Patterns and conventions discovered in one task benefit future tasks
- **Efficient updates** — Update once, all plans benefit
- **Reduces context bloat** — Plans stay smaller, repo map holds file details

## When to Update

### During Planning

After exploring the codebase to understand structure:
- Add directories that contain relevant code
- Add files that will likely be modified
- Add files that are related but won't be modified

### During Execution

- **Every time you touch a file** — Update if not in map
- **Every time you discover a file** — Add it
- **When file purpose becomes clearer** — Update the description

### After Each Step

Before marking a step complete:
- Review all files touched in that step
- Ensure each is in the repo map
- Update descriptions if understanding improved

## What to Include

### Core Files (Files directly modified)

Include:
- Files you're implementing or modifying
- Files you created
- Configuration files you changed

Format: Add to Core Files table with task name

### Related Files (Files referenced but not modified)

Include:
- Files you read for context
- Files with related functionality
- Files that import/export from core files
- Test files related to your changes
- Configuration files relevant to the task

Format: Add to Related Files table with task name

### Key Directories

Include:
- Directories containing your core files
- Important project directories (src/, lib/, tests/, etc.)
- Directories with relevant configuration

### Architecture Notes

Include:
- Patterns discovered (e.g., "Service layer pattern", "Repository pattern")
- Conventions followed (e.g., "All handlers return Result<T>", "Errors use custom AppError class")
- Key dependencies and why they're used

## What to Skip

Do NOT include:
- `node_modules/`, `vendor/`, or other dependency directories
- Build output directories (`dist/`, `build/`, `.next/`)
- Temporary files or caches
- Files with obvious purpose from name alone (e.g., `README.md`)
- Files you never touched or referenced

## How to Update

### Reading the Repo Map

Always read `plans/repo-map.md` before making changes:

```
Use the read tool on: plans/repo-map.md
```

### Adding a Core File

```markdown
| Path | Purpose | Task | Last Updated |
|------|---------|------|--------------|
| `src/auth/login.ts` | Login flow with JWT handling | add-user-auth | 2024-01-15 |
```

### Adding a Related File

```markdown
| Path | Why Relevant | Task | Last Updated |
|------|--------------|------|--------------|
| `src/utils/validation.ts` | Email validation used by login | add-user-auth | 2024-01-15 |
```

### Adding a Directory

```markdown
| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/api/` | REST endpoints for user features | 2024-01-15 |
```

### Updating an Existing Entry

If an entry already exists:
- Keep the existing Purpose/Why Relevant
- Add additional context if needed (append, don't replace)
- Update Last Updated date
- Keep the original Task reference (helps trace history)

### Recording Task History

Update the Task History section with files modified:

```markdown
| Task | Files Modified | Date |
|------|----------------|------|
| add-user-auth | src/auth/login.ts, src/auth/middleware.ts | 2024-01-15 |
```

### Recording Architecture Notes

When patterns or conventions are discovered:

```markdown
### Patterns
- **Service Layer**: Business logic in src/services/, controllers are thin wrappers

### Conventions
- **Error Handling**: All errors use AppError class with code and message

### Key Dependencies
- **jsonwebtoken**: JWT token generation and validation
```

## Using the Repo Map During Execution

Before searching for files:
1. Read `plans/repo-map.md` first
2. If the file is there, use the path directly
3. If not found, search, then add to map

This prevents:
- Repeated searches for the same file
- Confusion about file locations
- Lost context across sessions

## Example

### Before exploring:

```markdown
# Repo Map

> Last Updated: 2024-01-15
> Project: My App

## Core Files

| Path | Purpose | Task | Last Updated |
|------|---------|------|--------------|

## Related Files

| Path | Why Relevant | Task | Last Updated |
|------|--------------|------|--------------|

## Key Directories

| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/` | Main source code | 2024-01-15 |
```

### After implementing authentication:

```markdown
# Repo Map

> Last Updated: 2024-01-15
> Project: My App

## Core Files

| Path | Purpose | Task | Last Updated |
|------|---------|------|--------------|
| `src/auth/login.ts` | Login flow with JWT handling | add-user-auth | 2024-01-15 |
| `src/auth/middleware.ts` | Auth middleware for protected routes | add-user-auth | 2024-01-15 |

## Related Files

| Path | Why Relevant | Task | Last Updated |
|------|--------------|------|--------------|
| `src/utils/validation.ts` | Email validation used by login | add-user-auth | 2024-01-15 |
| `src/config/constants.ts` | JWT secret and expiry constants | add-user-auth | 2024-01-15 |
| `tests/auth/login.test.ts` | Existing tests for login flow | add-user-auth | 2024-01-15 |

## Key Directories

| Path | Contents | Last Updated |
|------|----------|--------------|
| `src/` | Main source code | 2024-01-15 |
| `src/auth/` | Authentication modules | 2024-01-15 |
| `tests/` | Test files mirroring src structure | 2024-01-15 |

## Architecture Notes

### Patterns
- **Middleware Chain**: Auth middleware validates JWT before route handlers

### Conventions
- **JWT in httpOnly Cookie**: Prevents XSS attacks on tokens

### Key Dependencies
- **bcrypt**: Password hashing with salt
- **jsonwebtoken**: JWT token management

## Task History

| Task | Files Modified | Date |
|------|----------------|------|
| add-user-auth | src/auth/login.ts, src/auth/middleware.ts | 2024-01-15 |
```

## Relationship to Plan

The plan file contains a **Quick Reference** table:
- Lists only the key files for that specific task
- Provides at-a-glance context within the plan
- References the full repo map for details

The repo map is the **complete** file inventory:
- Lives in `plans/repo-map.md`
- Project-wide, accumulates across all tasks
- Contains full details and history

## Mandatory Outputs

Before completing Repo Map updates, verify:

- [ ] Read `plans/repo-map.md` before editing
- [ ] Core Files table updated if any files were modified/created
- [ ] Related Files table updated if any files were read for context
- [ ] Key Directories updated if new directories discovered
- [ ] Task History has entry for current task
- [ ] All entries have Purpose/Why Relevant filled in (not empty)
- [ ] All entries have Last Updated date
- [ ] No duplicate entries exist
- [ ] Architecture Notes updated if patterns/conventions discovered

## Verification Checklist

When another step asks you to verify repo map sync, use this:

```
From the step's Files Changed section:
  1. List all files
  2. For each file:
     - Is it in Core Files? (if modified/created)
     - Is it in Related Files? (if only read)
     - If missing → FAIL, must add
  3. For each directory containing modified files:
     - Is directory in Key Directories?
     - If missing → FAIL, must add
```

This verification is enforced in verify-step and persist-plan.