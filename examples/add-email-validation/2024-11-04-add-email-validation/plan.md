# Plan: Add Email Validation

> Status: ACTIVE
> Created: 2024-11-04
> Last Updated: 2024-11-05

<!--
  Plan-level status (lifecycle):
    DRAFT     — awaiting approval after clarification
    ACTIVE    — execution in progress
    COMPLETED — all steps done, verified
    ARCHIVED  — optional long-term archival state
-->

## Goal
Provide a standalone `validateEmail(input)` utility that returns a structured result with a specific error code when validation fails.

## Assumptions
- TypeScript project with existing `src/utils/` directory and Vitest configured.
- No email deliverability check (DNS/MX lookup) — format validation only.
- RFC 5322 "practical" subset is acceptable (not the full grammar).
- Error shape: `{ valid: boolean; error?: EmailErrorCode }`.

## Open Questions
None.

## Design Decisions
| Decision | Options Considered | Chosen | Confirmed |
|----------|--------------------|--------|-----------|
| Error representation | Free-form strings vs discriminated union with codes | Discriminated union with codes — callers can localize messages | yes |
| Validation library | `validator` package vs regex + length checks | Regex + length checks — `validator` rejected by `docs/deps-policy.md` | yes |
| Whitespace handling | Trim-and-accept vs reject as invalid | Reject — matches existing `validatePhone` behavior | yes |

## Steps Overview
| Step | File | Status | Goal |
|------|------|--------|------|
| Step 1 | `steps/step-1.md` | COMPLETED | Define `EmailErrorCode` union and `EmailValidationResult` type |
| Step 2 | `steps/step-2.md` | COMPLETED | Implement `validateEmail` with regex + length checks |
| Step 3 | `steps/step-3.md` | IN_PROGRESS | Write comprehensive Vitest test suite (≥20 cases) |
| Step 4 | `steps/step-4.md` | PENDING | Update CHANGELOG and `docs/utils.md` |

## Validation Commands

| Purpose | Command | Source | Required? |
|---|---|---|---|
| Typecheck | `npx tsc --noEmit` | tsconfig.json | yes |
| Lint | `npx eslint src/` | .eslintrc.js | yes |
| Test | `npx vitest run` | package.json scripts | yes |

## Context & Learnings
### Key Decisions
- **Return discriminated union with error codes, not free-form strings.** Callers need to localize messages; strings would lock us to English. Codes: `EMPTY`, `NO_AT`, `MULTIPLE_AT`, `INVALID_LOCAL`, `INVALID_DOMAIN`, `TOO_LONG`.
- **Use a regex + length checks, not a parsing library.** The project already rejects `validator` as a dependency in `docs/deps-policy.md`.
- **Treat trailing/leading whitespace as invalid, not trim-and-accept.** Matches the existing `validatePhone` behavior in `src/utils/phone.ts`.

### Gotchas & Warnings
- **Max total length is 254 chars, not 320.** RFC 5321 limit for SMTP envelope. Several sources cite 320 incorrectly.
- **Local part max is 64 chars.** Check independently — some emails have short domains but very long local parts.
- Unicode domains (IDN / Punycode) are explicitly out of scope for v1.

> Append only. Never delete or rewrite existing entries below — only add new rows/facts as steps complete.
### Working Set
| Path | Role in this task | Evidence |
|------|-------------------|----------|
| `src/utils/email.ts` | New validator and main implementation target | Planned from user request; created in Step 1 |
| `src/utils/email.test.ts` | Vitest coverage for validator behavior | Test framework confirmed in `package.json`; created in Step 3 |
| `src/utils/index.ts` | Barrel export for utils | Existing barrel pattern verified by reading file in Step 1 |
| `src/utils/phone.ts` | Reference validator shape/style | Read before implementation; matches `{ valid, error }` pattern |
| `src/utils/phone.test.ts` | Reference test structure | Read before Step 3; uses `describe.each` |
| `docs/deps-policy.md` | Dependency policy | Read before planning; rejects `validator` package |

### Verified Facts
- Vitest is already configured — verified by reading `package.json`.
- Validators use named exports and barrel export through `src/utils/index.ts` — verified by reading existing utils.
- Tests are colocated as `*.test.ts` next to source files — verified by reading `src/utils/phone.test.ts`.
- The `validator` package should not be added — verified by reading `docs/deps-policy.md`.
- Every util in `src/utils/` has a matching `*.test.ts` next to it (not in a separate `tests/` tree) — verified by `ls src/utils/`.
- All validators return `{ valid: boolean; error?: Code }` — see `validatePhone`, `validateZip`.

## Implementation Log
| Date | Step | Summary |
|------|------|---------|
| 2024-11-04 | Step 1 | Added error codes + result types; barrel export updated |
| 2024-11-05 | Step 2 | Implemented `validateEmail`; all branches reachable. Split single mega-regex into `LOCAL_RE` + `DOMAIN_RE` for readability — no behavioral change. |
| 2024-11-05 | Step 3 | Started test suite; happy path + first 2 error codes done |
