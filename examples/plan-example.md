# Plan Example (Directory Structure)

> A filled-in example showing how a completed plan looks in the directory-based structure. Every file follows its template exactly. Use this as a reference when filling in [`task-overview-template.md`](task-overview-template.md) and [`step-template.md`](step-template.md).

---

## Directory Layout

```
plans/2026-04-28-add-email-validation/
├── plan.md                          # Task overview (see below)
└── steps/
    ├── step-1-client-validation.md   # Step 1: Add client-side validation (see below)
    ├── step-2-server-validation.md   # Step 2: Add server-side validation
    └── step-3-shared-messages.md     # Step 3: Extract shared messages
```

## `plan.md` — Task Overview

```markdown
# Plan: Add email validation to user registration

> Status: IN_PROGRESS
> Created: 2026-04-28
> Last Updated: 2026-04-28

## Goal
Registration rejects invalid email formats and shows a user-friendly error message before hitting the server.

## Assumptions
- The project uses React + TypeScript frontend with a Node/Express backend.
- There is an existing test framework (vitest) and lint setup (eslint).
- No email validation library is currently installed.

## Open Questions
None.

## Design Decisions
| Decision | Options Considered | Chosen | Confirmed |
|----------|--------------------|--------|-----------|
| Where to validate | Client-only vs client+server | Client + server — defense in depth | yes |
| Error display style | Inline under field vs toast | Inline — matches existing form patterns in `src/components/LoginForm.tsx` | yes |
| Validation library | regex vs zod vs validator.js | zod — already a project dependency for API schemas | yes |

## Steps Overview
| Step | File | Status | Goal |
|------|------|--------|------|
| Step 1 | `steps/step-1-client-validation.md` | COMPLETED | Add client-side email validation to RegistrationForm |
| Step 2 | `steps/step-2-server-validation.md` | IN_PROGRESS | Add server-side email validation to POST /register |
| Step 3 | `steps/step-3-shared-messages.md` | PENDING | Extract shared validation message constants |

## Validation Commands

| Purpose | Command | Source | Required? |
|---|---|---|---|
| Typecheck | `npx tsc --noEmit` | tsconfig.json | yes |
| Lint | `npx eslint src/` | .eslintrc.js | yes |
| Test | `npx vitest run` | package.json scripts | yes |

## Context & Learnings
### Key Decisions
- Use zod for email validation since it is already a dependency for API schema validation.
### Gotchas & Warnings
- `RegistrationForm` uses uncontrolled inputs with `useRef`; validation must read `.current.value`.

> Append only. Never delete or rewrite existing entries below — only add new rows/facts as steps complete.
### Working Set
| Path | Role in this task | Evidence |
|------|-------------------|----------|
| `src/components/RegistrationForm.tsx` | Form to add validation to | `read` — 89 lines, uncontrolled inputs with refs |
| `src/api/routes/auth.ts` | Server endpoint to add validation to | `read` — POST /register handler at line 42 |
| `src/api/schemas/auth.ts` | Existing zod schemas for auth | `read` — exports `loginSchema`, no `registerSchema` yet |
| `src/components/__tests__/RegistrationForm.test.tsx` | Existing test file to extend | `read` — 3 tests, all happy-path |
| `src/api/__tests__/auth.test.ts` | Existing API test file | `read` — covers POST /login, no /register tests |
### Verified Facts
- zod is installed (v3.22.4) — verified by `read package.json`, 2026-04-28.
- `RegistrationForm` uses `useRef` not `useState` for inputs — verified by `read src/components/RegistrationForm.tsx`, line 12-14.
- POST /register has no server-side email check — verified by `read src/api/routes/auth.ts`, line 42-58.
- `LoginForm.tsx` shows inline error pattern: `<span className="field-error">` — verified by `read src/components/LoginForm.tsx`, line 31.

## Implementation Log
| Date | Step | Summary |
|------|------|---------|
| 2026-04-28 | Step 1 | Client-side email validation with zod + inline errors + 3 tests. |
```

## `step-1-client-validation.md` — Completed Step Example

```markdown
# Step 1: Add client-side email validation to RegistrationForm

> Status: COMPLETED
> Created: 2026-04-28

## Goal
RegistrationForm validates email on blur and submit using zod, showing inline error for invalid formats.

## Prerequisites
- Files to modify: `src/components/RegistrationForm.tsx`, `src/components/__tests__/RegistrationForm.test.tsx`
- Design: inline error display confirmed (matches LoginForm pattern)

## Deliverables
- `RegistrationForm` validates email on blur and submit using zod
- Inline error message shown for invalid emails
- After this step: `npx vitest run --filter=RegistrationForm` passes with new validation tests

## Plan
- [x] `edit` src/components/RegistrationForm.tsx — import `z` from zod, add `emailSchema = z.string().email()`, validate in `handleSubmit` and `onBlur`
- [x] `edit` src/components/RegistrationForm.tsx — add `<span className="field-error">` below email input (matches LoginForm pattern)
- [x] `edit` src/components/__tests__/RegistrationForm.test.tsx — add tests: valid email submits, invalid email shows error, error clears on valid input
- [x] `bash` npx vitest run --filter=RegistrationForm — expect 0 failures

## Quality Checklist
- [x] Existing pattern identified — `LoginForm.tsx` inline `field-error` display
- [x] Contract understood — registration form validates email before submit
- [x] Reuse checked — zod already used for validation schemas
- [x] Error/edge cases handled — invalid format and error clearing covered
- [x] Security/data/performance risk reviewed — client validation only; server validation remains Step 2

## Validation Checklist
- [x] `npx tsc --noEmit` exits 0
- [x] `npx eslint src/components/RegistrationForm.tsx` exits 0

## Test Checklist
- [x] `npx vitest run --filter=RegistrationForm` — 6 tests pass (3 existing + 3 new)

## Implementation Notes
Added zod email validation with blur + submit triggers. Used same `field-error` class as LoginForm for consistency. Added 3 tests covering invalid format, valid format, and error clearing.

## Files Changed
`src/components/RegistrationForm.tsx`, `src/components/__tests__/RegistrationForm.test.tsx`
```

## `step-2-server-validation.md` — In-Progress Step Example

```markdown
# Step 2: Add server-side email validation to POST /register

> Status: IN_PROGRESS
> Created: 2026-04-28

## Goal
POST /register rejects invalid emails with 400 + error body using zod `registerSchema`.

## Prerequisites
- Step 1 completed — client-side validation in place
- Files to modify: `src/api/routes/auth.ts`, `src/api/schemas/auth.ts`, `src/api/__tests__/auth.test.ts`

## Deliverables
- `registerSchema` with email validation exported from schemas
- POST /register rejects invalid emails with 400 + error body
- After this step: `npx vitest run --filter=auth` passes with new registration tests

## Plan
- [ ] `edit` src/api/schemas/auth.ts — add `registerSchema = z.object({ email: z.string().email(), password: z.string().min(8) })`
- [ ] `edit` src/api/routes/auth.ts — import `registerSchema`, add `.safeParse(req.body)` at top of POST /register handler
- [ ] `edit` src/api/__tests__/auth.test.ts — add tests: valid registration, invalid email returns 400, missing email returns 400
- [ ] `bash` npx vitest run --filter=auth — expect 0 failures

## Quality Checklist
- [ ] Existing pattern identified
- [ ] Contract understood
- [ ] Reuse checked
- [ ] Error/edge cases handled
- [ ] Security/data/performance risk reviewed

## Validation Checklist
- [ ] `npx tsc --noEmit` exits 0
- [ ] `npx eslint src/api/` exits 0

## Test Checklist
- [ ] `npx vitest run --filter=auth` — all pass including 3 new registration tests

## Implementation Notes
[Fill after implementation]

## Files Changed
[Fill after implementation]
```

---

> Note: This example shows the new directory-based plan structure. Each step is a separate file under `steps/`, eliminating context noise from unrelated steps in a flat monolithic plan file. Plan discovery works by scanning plan directories and step statuses.
