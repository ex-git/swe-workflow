# Verify Step

## Contract — Read This First

1. Run all three passes — Validate, Test, Review Diff — before marking any step COMPLETED.
2. Re-read every modified file with `read`. Do not trust memory.
3. Run automated checks: lint, typecheck, format, pre-commit hooks (when configured).
4. Tests must cover happy path, edge cases, and error paths. All must pass.
5. Review the actual `git diff` — no artifacts, no scope creep, no unverified imports.
6. Every changed file must be in the step's Files Changed; every fact in Verified Facts must have tool evidence.
7. If any pass fails: fix, re-run. Do not defer. Do not mark COMPLETED with known failures.

## When to Use

After finishing implementation in `execute-step`, before `persist-plan`.

## Pass 1 — Validate

1. **Re-read every modified file** with `read`. Not from memory.
2. **Run automated checks:**
   ```bash
   # JS/TS:     npm run lint && npm run typecheck && npm test
   # Python:    ruff check . && mypy . && pytest
   # Rust:      cargo fmt --check && cargo clippy && cargo test
   # Go:        gofmt -w <changed-files> && go vet ./... && go test ./...
   ```
3. **Run pre-commit hooks** when configured (lefthook, husky, pre-commit). If hooks can't run, document why and the closest equivalent checks run.
4. **Verify evidence:** every changed file in Files Changed, every fact in Working Set/Verified Facts backed by read/search/tool output.

**Checklist:** re-read ✓ | parses ✓ | lint ✓ | format ✓ | hooks ✓ | matches plan ✓ | edge cases ✓ | only in-scope files ✓ | evidence accurate ✓

## Pass 2 — Test

1. **Define expected behavior** — inputs, outputs, failure modes.
2. **Write tests** — happy path, edge cases, error cases.
3. **Run tests** — all must pass.
4. **No test framework?** Document manual verification:
   ```markdown
   **Test Checklist:**
   - [x] Manual: ran `node scripts/example.js` → expected output "success"
   ```
5. **Add test files to Working Set.**

**Checklist:** happy path ✓ | edge cases ✓ | error paths ✓ | all pass ✓ | test files in Working Set ✓

## Pass 3 — Review Diff

1. **View the actual diff:** `git diff` / `git diff --staged`
2. **Scan and fix now:**

   | Category | Look for |
   |---|---|
   | Artifacts | `console.log`, commented-out code, stray TODO/FIXME, unused imports |
   | Scope creep | Formatting changes to unrelated files, "while I'm here" refactors |
   | Evidence gaps | Unverified imports, packages, APIs, paths, callers |
   | Quality | Magic numbers, poor names, deep nesting (>3), long functions (>30 lines) |
   | Duplication | Copy-pasted blocks, similar logic in multiple places |
   | Missing handling | No error handling on fallible ops, missing null/empty checks |
   | Style | Doesn't match existing formatting, naming, imports |

3. **Scope audit:** every changed file explainable by the step's Plan. If not → dependency (document) or scope creep (revert).

**Checklist:** diff reviewed ✓ | no artifacts ✓ | no formatting-only changes ✓ | only planned files ✓ | no duplication ✓ | error handling present ✓ | no unverified imports ✓ | style matches ✓

## If Any Pass Fails

1. Document issue in Implementation Notes.
2. Fix immediately — do not defer.
3. Re-run the failing pass.
4. Pre-existing unrelated failure? Document evidence, run narrowest passing check, don't broaden scope.
5. Significant extra work needed? Update plan (see execute-step → Scope Changes).

## Final Gate

All three passes must be clean before `persist-plan`:

- [ ] **Validate:** re-read, checks pass, hooks pass, evidence accurate
- [ ] **Test:** written, running, passing (or manual verification documented)
- [ ] **Review:** diff clean, scope intact, no artifacts

## Next Step

→ **persist-plan** (mark COMPLETED, update plan and context)
