# Validate Step

## Overview

Strict validation before any step can be marked complete. Trust the file, not your memory.

## When to Use

After completing implementation in execute-step, before proceeding to tests or marking complete.

## Instructions

1. **Re-read every modified file** — use the `read` tool, not your memory of what you wrote
2. **Check syntax:** Does the code parse? Any typos, missing brackets, incorrect imports?
3. **Check logic:** Does the implementation match the plan? Are edge cases handled?
4. **Check consistency:** Does it follow existing code style and patterns?
5. **Check side effects:** Could this change break anything else?
6. **Run available checks:**
   - Linter/formatter if configured (`npm run lint`, `eslint`, etc.)
   - Type checker if applicable (`tsc`, `mypy`, etc.)
   - Unit tests: `npm test`, `pytest`, or equivalent
7. **If issues found:** fix immediately, update Implementation Notes
8. **Only pass if ALL checks are clean**

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
- **Do not rely on memory** — re-read the actual file content
- **Do not skip because "it's a small change"** — small changes cause big bugs
- **Run automated checks** — don't just visually inspect
- If tests don't exist yet: validation still applies (syntax + logic + consistency)

## Mandatory Checklist

Before proceeding to tests:

- [ ] All modified files re-read with `read` tool
- [ ] Syntax check passed
- [ ] Logic matches plan specification
- [ ] Scope check: only intended files modified
- [ ] Automated checks (lint/test) passed or documented why not run

## Next Steps

After validation passes:

1. → Proceed to: **enforce-tests** (write/run tests)
2. → Then: **review-diff** (review your changes critically)
3. → Then: **persist-plan** (mark step COMPLETED)