# Step 1: Define error codes and result type

> Status: COMPLETED
> Created: 2024-11-04

## Goal
Define `EmailErrorCode` discriminated union and `EmailValidationResult` type, both exported via the utils barrel.

## Prerequisites
- `src/utils/` directory exists
- Existing `validatePhone` signature reviewed for consistency

## Deliverables
- `EmailErrorCode` union type covering: `EMPTY`, `NO_AT`, `MULTIPLE_AT`, `INVALID_LOCAL`, `INVALID_DOMAIN`, `TOO_LONG`
- `EmailValidationResult` type matching the project's `{ valid: boolean; error?: Code }` pattern
- After this step: `npx tsc --noEmit` clean and types exported from `src/utils/index.ts`

## Plan
- [x] `write` src/utils/email.ts — add `EmailErrorCode` union and `EmailValidationResult` type
- [x] `edit` src/utils/index.ts — add barrel re-export for the new types
- [x] `bash` npx tsc --noEmit — expect 0 errors

## Quality Checklist
- [x] Existing pattern identified — `validatePhone.ts` uses `satisfies readonly string[]` for the error code union
- [x] Contract understood — types must be named exports, no default
- [x] Reuse checked — barrel export pattern already in `src/utils/index.ts`
- [x] Risk reviewed — low, types only
- [x] Mitigation recorded — TypeScript compile gate

## Validation Checklist
- [x] `npx tsc --noEmit` exits 0
- [x] Types exported from barrel (verified by `grep export src/utils/index.ts`)

## Test Checklist
- [x] Compile-time assertions for exhaustive error codes (via `satisfies`)

## Implementation Notes
Used `satisfies readonly string[]` pattern to derive the union from the array; matches `validatePhone.ts` exactly. Exported types as named exports only; no default export.

## Files Changed
- `src/utils/email.ts` (created)
- `src/utils/index.ts` (modified — barrel export added)
