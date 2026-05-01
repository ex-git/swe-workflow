# Verify Step

## Overview

Single verification gate that runs after implementation and before marking a step COMPLETED. Combines three passes: **Validate** (re-read, syntax, logic), **Test** (coverage), and **Review Diff** (scope, artifacts, quality).

Trust the file, not your memory. Assume you made mistakes — because you probably did.

## When to Use

After finishing the implementation portion of `execute-step`, before moving to `persist-plan`.

## Prerequisites

- [ ] Implementation for the current step is complete
- [ ] You know which files were changed (from `Files Changed` in the plan step)

---

## Pass 1 — Validate

**Goal:** the code is correct, matches the plan, and stays in scope.

1. **Re-read every modified file** with the `read` tool. Do not rely on memory of what you wrote.
2. **Run automated checks** that exist in the project, including formatter/format-checker when configured:
   ```bash
   # JS/TS:     npm run lint && npm run typecheck && npm test
   # Python:    ruff check . && mypy . && pytest
   # Rust:      cargo fmt --check && cargo clippy && cargo test
   # Go:        gofmt -w <changed-files> && go vet ./... && go test ./...
   ```
   Prefer the narrowest meaningful check first for quick feedback, then run the project-standard gate before completion when practical.
3. **Run configured pre-commit hooks / quality gates** when present. Detect common hook managers and run the matching command, for example:
   ```bash
   # Lefthook:  npx lefthook run pre-commit --all-files
   # Lefthook:  lefthook run pre-commit --all-files
   # Generic:   pre-commit run --all-files
   ```
   If hooks exist but cannot run in the environment, document the reason and the closest equivalent checks that were run.
4. **Verify task evidence.** Every changed file must be listed in the step's `Files Changed`, and any file/fact used to justify the change must appear in the plan's Working Set or Verified Facts with read/search/tool evidence. Update the advisory repo map only for durable project discoveries. See [maintain-repo-map](maintain-repo-map.md).

### Validate checklist
- [ ] All modified files re-read with `read` tool
- [ ] Code parses (no syntax / type errors)
- [ ] Linter clean (if configured)
- [ ] Formatter/format-checker clean (if configured)
- [ ] Pre-commit hooks / quality gates run clean (if configured, e.g. lefthook)
- [ ] Implementation matches the step's Plan section
- [ ] Edge cases / error cases handled
- [ ] Only files in scope were modified
- [ ] Working Set, Verified Facts, and Files Changed are accurate and evidence-backed

---

## Pass 2 — Test

**Goal:** behavior is covered by tests, and the tests pass.

1. **Define expected behavior** — inputs, outputs, failure modes.
2. **Write tests** covering happy path, edge cases, and error cases.
3. **Run the test command** for the project. All tests must pass.
4. **If no test framework exists**, document manual verification in the plan:
   ```markdown
   **Test Checklist:**
   - [x] Manual: ran `node scripts/example.js` → expected output "success"
   - [x] Manual: tested with invalid input → error message shown
   ```
5. **Add test files to the Working Set** — tests are part of the implementation evidence.

### What tests look like by change type

| Change type | Tests required |
|---|---|
| New utility / function / module | Unit tests (happy, edge, error) |
| Bug fix | Regression test that fails before fix, passes after |
| API change | Integration tests (request validation, response shape, errors) |
| Config change | Valid / invalid config behavior |
| Documentation | Manual verification acceptable |

### Test checklist
- [ ] Happy path covered
- [ ] Edge cases covered
- [ ] Error paths covered
- [ ] New utilities/functions/modules have focused tests when a test framework exists
- [ ] All tests pass (or manual verification documented)
- [ ] Test files added to the Working Set

**Write tests that verify behavior, not that the function exists.** If writing a test reveals a bug, fix the implementation and note it.

---

## Pass 3 — Review Diff

**Goal:** no artifacts, no scope creep, no copy-paste, nothing you wouldn't hand to a teammate.

1. **View the actual diff:**
   ```bash
   git diff
   git diff --staged
   ```
   If there's no git, compare against snapshots taken in `execute-step`.

2. **Scan for these issues and fix them now** (not later):

   | Category | Look for |
   |---|---|
   | Artifacts | `console.log`, `print`, commented-out code, stray `TODO`/`FIXME`, unused imports |
   | Scope creep | Formatting-only changes to unrelated files, "while I'm here" refactors, renames without reason |
   | Evidence gaps | Imports, packages, APIs, paths, callers, or conventions not verified by read/search/tool output |
   | Bad patterns | Magic numbers, hardcoded values, poor names (`temp`, `data`, `x`), deep nesting (>3), long functions (>30 lines) |
   | Degraded patterns | Copying unsafe or obsolete surrounding patterns instead of using project-approved tooling/conventions |
   | Unnecessary abstraction | New utilities, wrappers, factories, dependencies, or indirection not required by the task |
   | Duplication | Copy-pasted blocks, similar logic in multiple places |
   | Missing handling | No error handling on fallible ops, missing null/empty checks |
   | Style | Doesn't match existing codebase formatting, naming, imports, or comments |

3. **Scope audit:** every changed file should be explainable by the step's Plan. If not, either (a) it's a dependency — document in Implementation Notes, or (b) it's scope creep — revert.

### Review checklist
- [ ] Reviewed the actual diff (not from memory)
- [ ] No debugging artifacts or commented-out code remain
- [ ] No accidental formatting-only changes
- [ ] Only planned files modified
- [ ] No copy-paste duplication
- [ ] Error handling present where operations can fail
- [ ] No unverified imports, dependencies, paths, APIs, callers, or conventions
- [ ] No unnecessary abstraction, dependency, or utility was added
- [ ] Style matches surrounding code

---

## Mandatory Final Checklist

Before moving to `persist-plan`, all three passes must be clean:

- [ ] **Validate:** re-read, checks and configured hooks pass, task evidence accurate
- [ ] **Test:** written, running, passing (or manual steps documented)
- [ ] **Review:** diff clean, scope intact, no artifacts

## If Any Pass Fails

1. Document the issue in the step's Implementation Notes.
2. Fix issues introduced by this change immediately — do not defer.
3. Re-run the failing pass.
4. If a failure is pre-existing and unrelated, document evidence, run the narrowest relevant passing check, and do not broaden scope without user approval.
5. If the fix requires significant extra work, consider updating the plan (see `execute-step` → "When New Findings Require Additional Changes").

## Constraints

- **No step passes with known introduced issues** — fix them or mark the step BLOCKED.
- **No step passes with guessed facts** — Working Set and Verified Facts must be evidence-backed.
- **Do not rely on memory** — re-read actual files and view the actual diff.
- **Run automated checks, formatters, and configured pre-commit hooks** when they exist — don't just visually inspect.
- **Fix introduced issues now, not later.**

## Next Step

→ Proceed to: **persist-plan** (mark COMPLETED, update plan and context)
