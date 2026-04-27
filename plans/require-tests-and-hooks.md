# Plan: Require Tests and Pre-Commit Hooks

> Status: COMPLETED
> Created: 2026-04-26
> Last Updated: 2026-04-26

## Goal

Strengthen the swe-workflow skill so agents write tests for new utilities/functions and run configured pre-commit hooks such as lefthook during verification.

## Assumptions

- The user wants workflow guidance changes, not implementation code changes in another project.
- `SKILL.md` remains the canonical source of truth for top-level behavior.
- Detailed verification rules belong in `references/verify-step.md`.
- Source-of-truth workflow changes should be recorded in `CHANGELOG.md`.

## Open Questions


## Context & Learnings

> Important context discovered during implementation. Update after each step to preserve learnings for future sessions.

### Key Decisions
> Decisions made and why (prevents re-litigating in future sessions)

- Add concise top-level guard language in `SKILL.md` and detailed enforcement in `references/verify-step.md`: keeps the entrypoint readable while making verification actionable.

### Gotchas & Warnings
> Things that tripped us up or could cause issues

- Full workflow pre-edit gate applies because this task changes source-of-truth workflow rules.

### Patterns & Conventions
> Code patterns, naming conventions, architectural decisions discovered

- Changelog entries for unreleased workflow changes go under `## [Unreleased]`.

## Repo Map Reference

> Project-wide file map maintained separately. See `plans/repo-map.md` for all discovered files.

### Quick Reference
| Path | Purpose | Status |
|------|---------|--------|
| `SKILL.md` | Canonical workflow rules and behavioral guards | modified |
| `references/verify-step.md` | Detailed validation/test/review gate | modified |
| `CHANGELOG.md` | User-visible workflow change record | modified |
| `README.md` | Public overview, referenced for context | referenced |

---

## Steps

### Step 2: Bump release version and push

**Status:** COMPLETED

**Prerequisites:**
- Step 1 is completed and verified.
- Version target is inferred as patch release `1.4.1` for guidance-only changes.
- Target files are listed in `plans/repo-map.md`.

**Deliverables:**
- `SKILL.md` metadata version is `1.4.1`.
- `package.json` version is `1.4.1`.
- `CHANGELOG.md` has a dated `1.4.1` release section and updated comparison links.
- Changes are committed and pushed to the current branch.

**Plan:**
- [x] Update version metadata in `SKILL.md` and `package.json`.
- [x] Promote `[Unreleased]` changelog content to `1.4.1` with date and reset `[Unreleased]`.
- [x] Verify files, available checks/hooks, and diff.
- [x] Commit and push.

**Validation Checklist:**
- [x] Re-read all modified files.
- [x] Confirm version values and changelog links are consistent.
- [x] Review `git diff` for scope.
- [x] Confirm push succeeds.

**Test Checklist:**
- [x] Run available repository checks or document if none exist: no package scripts are configured.
- [x] Run configured pre-commit hooks or document if none exist: no lefthook/pre-commit config found.

**Implementation Notes:**
- Bumped `SKILL.md` metadata and `package.json` from `1.4.0` to `1.4.1`.
- Promoted changelog `[Unreleased]` notes into `## [1.4.1] - 2026-04-26`, reset `[Unreleased]`, and updated comparison links.
- Committed tracked release files with commit `81124b6` and pushed `main` to `origin`.

**Files Changed:**
- `SKILL.md`
- `package.json`
- `CHANGELOG.md`
- `references/verify-step.md`

---

### Step 1: Add test and hook requirements to workflow guidance

**Status:** COMPLETED

**Prerequisites:**
> What must be true before starting this step (files exist, previous steps done, etc.)

- Plan file exists and this step is marked `IN_PROGRESS`.
- Target files are listed in `plans/repo-map.md`.

**Deliverables:**
> What this step produces (files created/modified, functions added, etc.)

- `SKILL.md` explicitly instructs agents to write tests for new utilities/functions when a test framework exists.
- `references/verify-step.md` explicitly instructs agents to discover and run configured pre-commit hooks such as lefthook before completion.
- `CHANGELOG.md` documents the workflow guidance change.

**Plan:**
- [x] Add a behavioral guard in `SKILL.md` under Goal-Driven Execution for new functions/modules/utilities to include focused tests unless no framework exists or the user explicitly opts out.
- [x] Add verification details in `references/verify-step.md` for pre-commit hooks, including lefthook detection/commands.
- [x] Add an `[Unreleased]` changelog entry.

**Validation Checklist:**
- [x] Re-read all modified files.
- [x] Confirm guidance is concise, imperative, and non-conflicting.
- [x] Review `git diff` for scope and formatting.

**Test Checklist:**
- [x] Documentation/manual verification: confirmed Markdown structure and instructions cover utilities/functions plus pre-commit hooks.
- [x] Run available repository checks or document if none exist: no package scripts and no lefthook/pre-commit config are present.

**Implementation Notes:**
- Added top-level Goal-Driven Execution bullets in `SKILL.md` for tests on new utilities/functions/modules/components and configured quality gates.
- Expanded `references/verify-step.md` to require configured pre-commit hooks/quality gates and list lefthook/pre-commit commands.
- Updated `CHANGELOG.md` under `[Unreleased]`.
- Verification: re-read modified files, checked for package scripts and hook configs, grepped for expected guidance, and reviewed `git diff`.

**Files Changed:**
- `SKILL.md`
- `references/verify-step.md`
- `CHANGELOG.md`

---

## Implementation Log

| Date | Step | Summary |
|------|------|---------|
| 2026-04-26 | Step 1 | Added test and pre-commit hook requirements to skill guidance and changelog. |
| 2026-04-26 | Step 2 | Bumped release version to 1.4.1, committed, and pushed main. |

## Deviations from Original Plan

> Document any changes made during implementation that differ from initial plan


## Blocked Steps

> If any steps become blocked, document here

| Step | Reason | Date Blocked |
|------|--------|--------------|

---

*File locations tracked in: plans/repo-map.md*
*Session context tracked in: plans/context.md*
