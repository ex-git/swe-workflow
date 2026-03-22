# Validate Step

## Overview

Strict validation before any step can be marked complete. Trust the file, not your memory.

**CRITICAL:** This includes verifying the repo map is synchronized with files touched.

## When to Use

After completing implementation in execute-step, before proceeding to tests or marking complete.

## Instructions

1. **Re-read every modified file** — use the `read` tool, not your memory of what you wrote
2. **Check syntax:** Does the code parse? Any typos, missing brackets, incorrect imports?
3. **Check logic:** Does the implementation match the plan? Are edge cases handled?
4. **Check consistency:** Does it follow existing code style and patterns?
5. **Check side effects:** Could this change break anything else?
6. **Verify repo map is synchronized** — see "Repo Map Sync Check" below
7. **Run available checks:**
   - Linter/formatter if configured (`npm run lint`, `eslint`, etc.)
   - Type checker if applicable (`tsc`, `mypy`, etc.)
   - Unit tests: `npm test`, `pytest`, or equivalent
8. **If issues found:** fix immediately, update Implementation Notes
9. **Only pass if ALL checks are clean**

## Repo Map Sync Check

**This is a blocking requirement.** A step cannot pass validation if the repo map doesn't reflect all touched files.

### How to Verify

1. **Read the current step's Files Changed section:**
   - From the plan file, note every file in Files Changed
   - This is what you touched in this step

2. **Read plans/repo-map.md:**
   - Check each file from Files Changed exists in the Repo Map
   - Either in Core Files (if modified) or Related Files (if only read)

3. **Compare and verify:**

```
For each file in step's Files Changed:
  IF file was modified → must appear in Core Files table
  IF file was only read → must appear in Related Files table
  IF file is missing → FAIL validation, add to repo map
```

4. **If any file is missing:**
   - STOP validation
   - Add missing files to `plans/repo-map.md`
   - Re-read repo map to confirm additions
   - Then continue validation

### Why This Matters

- **Context is lost** between sessions if repo map is stale
- **Future agents** won't know what files exist
- **Search time** increases without a reliable file index
- **Handoffs fail** when file knowledge is incomplete

### Example

```
Step Files Changed:
- src/auth/login.ts (modified)
- src/utils/jwt.ts (created)
- src/config/constants.ts (read for context)

Check repo-map.md:
✓ src/auth/login.ts → Core Files ✓
✓ src/utils/jwt.ts → Core Files ✓
✗ src/config/constants.ts → NOT in repo map

Action: Add src/config/constants.ts to Related Files before passing validation.
```

## Validation Checklist

Go through each item:

### Code Quality
- [ ] Code parses without syntax errors
- [ ] No TypeScript/mypy type errors (if applicable)
- [ ] No linter warnings (if configured)
- [ ] Follows existing code style and patterns
- [ ] Variable/function names are clear and descriptive
- [ ] No hardcoded values that should be configurable
- [ ] No commented-out code
- [ ] No debugging statements (`console.log`, `print`, etc.)

### Logic
- [ ] Implementation matches the plan's specification
- [ ] Edge cases are handled (empty input, null, boundaries)
- [ ] Error cases are handled appropriately
- [ ] Return values/types match expectations
- [ ] No infinite loops or recursion issues

### Scope
- [ ] Only files in scope were modified
- [ ] No unintended changes to other files
- [ ] No "while I'm here" refactoring
- [ ] Changes are minimal and focused

### Repo Map
- [ ] All modified files are in Core Files table
- [ ] All read files are in Related Files table
- [ ] No files from Files Changed are missing from repo map
- [ ] All entries have Purpose/Why Relevant filled in
- [ ] All entries have Last Updated date

### Dependencies
- [ ] No new dependencies added unexpectedly
- [ ] Existing dependencies used correctly
- [ ] No circular dependencies introduced

### Side Effects
- [ ] Changes don't break existing functionality
- [ ] Database queries are efficient (if applicable)
- [ ] No memory leaks introduced
- [ ] No security vulnerabilities introduced

## Running Checks

```bash
# JavaScript/TypeScript
npm run lint
npm run typecheck  # or: npx tsc --noEmit
npm test

# Python
ruff check .
mypy .
pytest

# Rust
cargo clippy
cargo test

# Go
go vet ./...
go test ./...
```

## If Validation Fails

1. Document the issue in Implementation Notes
2. Fix the issue immediately
3. Re-run validation
4. Do NOT proceed until all checks pass
5. If fix requires significant changes, consider updating the plan

## Constraints

- **No step passes with known issues** — fix them or mark step BLOCKED
- **No step passes without repo map sync** — all touched files must be tracked
- **Do not rely on memory** — re-read the actual file content
- **Do not skip because "it's a small change"** — small changes cause big bugs
- **Run automated checks** — don't just visually inspect
- If tests don't exist yet: validation still applies (syntax + logic + consistency + repo map)

## Mandatory Checklist

Before proceeding to tests:

- [ ] All modified files re-read with `read` tool
- [ ] Syntax check passed
- [ ] Logic matches plan specification
- [ ] Scope check: only intended files modified
- [ ] **Repo map check: all touched files are tracked**
- [ ] Automated checks (lint/test) passed or documented why not run

## Next Steps

After validation passes:

1. → Proceed to: **enforce-tests** (write/run tests)
2. → Then: **review-diff** (review your changes)
3. → Then: **persist-plan** (mark step COMPLETED)