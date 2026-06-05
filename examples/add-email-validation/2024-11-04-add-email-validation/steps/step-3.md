# Step 3: Write comprehensive Vitest test suite

> Status: IN_PROGRESS
> Created: 2024-11-04

## Goal
Provide ≥20 Vitest cases covering happy paths, all six error codes, and key boundary conditions.

## Prerequisites
- Step 2 completed (`validateEmail` implementation exists)
- Vitest available (verified in `package.json`)
- Files to modify: `src/utils/email.test.ts` (new)

## Deliverables
- `src/utils/email.test.ts` with ≥20 cases
- Each error code triggered by ≥1 test
- Boundary cases (254, 64, empty label) covered
- After this step: `npx vitest run --filter=email` passes with all cases

## Plan
- [x] `write` src/utils/email.test.ts — happy-path cases: standard address, plus-addressing, subdomain, numeric TLD variants
- [x] `edit` src/utils/email.test.ts — `EMPTY`: `""`, whitespace-only
- [x] `edit` src/utils/email.test.ts — `NO_AT`: `"foo"`, `"foo.bar"`
- [ ] `edit` src/utils/email.test.ts — `MULTIPLE_AT`: `"a@b@c"`
- [ ] `edit` src/utils/email.test.ts — `TOO_LONG`: 255-char string, 254-char valid string (boundary)
- [ ] `edit` src/utils/email.test.ts — `INVALID_LOCAL`: leading dot, trailing dot, consecutive dots, 65-char local part
- [ ] `edit` src/utils/email.test.ts — `INVALID_DOMAIN`: no TLD, single-char TLD, trailing hyphen on label
- [ ] `bash` npx vitest run --filter=email — expect 0 failures

## Quality Checklist
- [ ] Existing pattern identified — `phone.test.ts` uses `describe.each` for parameterized cases
- [x] Contract understood — every error code path must have at least one test
- [ ] Reuse checked — no shared test fixtures needed
- [ ] Risk reviewed — low (test code only)
- [ ] Mitigation recorded — explicit count check (≥20 cases)

## Validation Checklist
- [ ] `npx tsc --noEmit` exits 0
- [ ] `npx eslint src/utils/email.test.ts` exits 0
- [ ] All tests pass

## Test Checklist
- [ ] ≥20 cases total
- [ ] Each error code triggered by ≥1 test
- [ ] Boundary cases (254, 64, empty label) covered

## Implementation Notes
Using `describe.each` for parameterized cases — mirrors `phone.test.ts` style. Currently 8 cases written; remaining boundary and `INVALID_*` cases pending.

## Files Changed
- `src/utils/email.test.ts` (created, in progress)
