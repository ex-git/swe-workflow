# Step 2: Implement validateEmail with regex + length checks

> Status: COMPLETED
> Created: 2024-11-04

## Goal
Implement the `validateEmail(input: string): EmailValidationResult` function covering all six error codes via regex + length checks.

## Prerequisites
- Step 1 completed (types are available in `src/utils/email.ts`)
- Files to modify: `src/utils/email.ts`

## Deliverables
- `validateEmail(input: string): EmailValidationResult` function exported from `src/utils/email.ts`
- All six error branches reachable (`EMPTY`, `NO_AT`, `MULTIPLE_AT`, `TOO_LONG`, `INVALID_LOCAL`, `INVALID_DOMAIN`)
- After this step: `npx tsc --noEmit` and `npx eslint src/` both clean

## Plan
- [x] `edit` src/utils/email.ts — empty-string check → `EMPTY`
- [x] `edit` src/utils/email.ts — `@` count check → `NO_AT` / `MULTIPLE_AT`
- [x] `edit` src/utils/email.ts — total length ≤254 → `TOO_LONG`
- [x] `edit` src/utils/email.ts — local part length ≤64 + `LOCAL_RE` → `INVALID_LOCAL`
- [x] `edit` src/utils/email.ts — domain `DOMAIN_RE` (labels, TLD ≥2 chars) → `INVALID_DOMAIN`
- [x] `bash` npx tsc --noEmit && npx eslint src/ — expect 0 errors

## Quality Checklist
- [x] Existing pattern identified — `validatePhone.ts` early-returns on each error case
- [x] Contract understood — function returns discriminated union, never throws
- [x] Reuse checked — no shared regex utility exists; new constants are appropriate
- [x] Risk reviewed — medium (regex correctness); mitigated via comprehensive Step 3 tests
- [x] Mitigation recorded — Step 3 covers all six branches with parameterized tests

## Validation Checklist
- [x] `npx tsc --noEmit` exits 0
- [x] `npx eslint src/utils/email.ts` exits 0
- [x] No `console.log` or debug statements left

## Test Checklist
- [x] All six error branches manually verified during implementation (formal tests in Step 3)
- [x] Valid address returns `{ valid: true }`

## Implementation Notes
**Deviation from original plan:** the original Step 2 sketch had a single mega-regex. Split into `LOCAL_RE` and `DOMAIN_RE` constants at the top of the file for readability and to make the `INVALID_LOCAL` vs `INVALID_DOMAIN` branching cleaner. No behavioral change. Length check happens first after empty check — cheapest to reject pathological inputs.

## Files Changed
- `src/utils/email.ts` (modified — added `validateEmail` function and `LOCAL_RE`/`DOMAIN_RE` constants)
