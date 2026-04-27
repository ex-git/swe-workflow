# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.5.1] - 2026-04-26

### Added
- Behavioral guards persistence note: guards are active for the entire session, not just the first response.
- Step decomposition anti-pattern in SKILL.md: WRONG (horizontal slices) vs RIGHT (vertical slices).
- Vertical-slice step sizing guide in `create-plan.md` with visual comparison.
- Grilling intensity for clarification: walk each branch of the design tree, provide recommended answers, explore codebase instead of asking when possible.

## [1.5.0] - 2026-04-26

### Changed
- **Restructured SKILL.md for agent compliance.** Plan format and step format are now inlined directly in SKILL.md so agents get the required output structure on the first read. Previously the plan template was 3 hops away (SKILL.md → create-plan.md → plan-template.md), causing 72% of plans to be non-compliant.
- SKILL.md reduced from 248 to 150 lines while adding the inlined plan format.
- Behavioral guards condensed from multi-bullet sections to one-line summaries.
- Added WRONG vs RIGHT anti-pattern comparison for plan format.
- Reference files are now supplementary (consulted when needed), not sequential prerequisites.
- `create-plan.md` updated to reference SKILL.md for plan format instead of deleted template file.

### Removed
- `assets/` directory and all template files (`plan-template.md`, `context-template.md`, `repo-map-template.md`). Content is now inlined in SKILL.md and respective reference files.
- Operating Modes table, Full workflow decision test, Skill Chain diagram, "Why Full Workflow Works" section, and Compact Variant section from SKILL.md (content preserved in condensed form or reference files).

## [1.4.2] - 2026-04-26

### Fixed
- Fixed issue where agent would write only the first phase to the plan file when user says "start with phase 1" after approving a multi-phase plan in chat. Plan file now explicitly requires ALL steps for ALL phases before execution begins.
- Added Full Workflow Guard: plan file missing steps for later phases blocks execution until completed.

## [1.4.1] - 2026-04-26

### Changed
- Strengthened workflow guidance to require focused tests for new utilities/functions/modules when a test framework exists.
- Strengthened verification guidance to run configured pre-commit hooks and quality gates, including lefthook, before marking work complete.

## [1.4.0] - 2026-04-24

### Added
- Mandatory workflow triage in `SKILL.md`, requiring agents to declare Lightweight or Full mode before code-task edits.
- Explicit Full workflow triggers for broad cleanup, lint/type/build/test cleanup, refactors, file deletion/moves, backend + UI changes, API/config contracts, source-of-truth docs, ambiguous scope, and tasks expected to touch more than 3 files.
- Full workflow pre-edit hard stop requiring a plan, an `IN_PROGRESS` step, and an edit mapped to that step before task target files are changed.
- Compact Full workflow guidance for broad cleanup/refactor/lint tasks.
- `AGENTS.md` repo-level instruction template to make workflow expectations visible outside the skill file.

### Changed
- Updated `README.md` to document triage-first behavior and stronger Full workflow expectations.
- Updated package metadata and included `AGENTS.md` in the package file list.

## [1.3.1] - 2026-04-24

### Added
- Behavioral Guards that apply to all code-related tasks: think before coding, simplicity first, surgical changes, and goal-driven execution.

### Changed
- Reframed the skill into lightweight and full workflow modes so simple coding tasks use behavioral guidance without creating plan files.
- Updated package metadata to describe behavioral guards plus the structured planning workflow.

## [1.3.0] - 2026-04-17

### Added
- `LICENSE` file (MIT).
- `CHANGELOG.md`.
- `package.json` with project metadata.
- `references/verify-step.md` — consolidated validation + testing + diff review into a single gate.
- `examples/add-email-validation/` — worked example showing populated `plan.md`, `repo-map.md`, and `context.md` mid-task.

### Changed
- **Consolidated references.** `validate-step.md`, `enforce-tests.md`, and `review-diff.md` merged into `verify-step.md` (three passes, one gate). `protect-code.md` folded into `execute-step.md` as a rules-of-thumb section. Net reduction: 4 files, ~460 lines.
- **Rewrote `SKILL.md`** (288 → ~180 lines): removed duplicated tables, simplified the skill chain diagram, deduped content with README.
- **Rewrote `README.md`** (247 → ~110 lines): stripped duplication with `SKILL.md`; README now focuses on install, purpose, and pointers.
- **Tightened `resume-workflow.md`** (340 → ~240 lines): compressed example, replaced prose with tables, removed duplicated "why" sections.
- **Simplified repo-map sync in `persist-plan.md`:** single-source procedure now lives in `maintain-repo-map.md`; other references link to it instead of repeating.
- Removed duplicate Status Values and File Relationships sections from `persist-plan.md` (canonical versions live in `SKILL.md`).
- Bumped version to `1.3.0`.

### Fixed
- Typo: "syncronized" → "synchronized" in `references/persist-plan.md`.

## [1.2.1] - 2026-03-21

### Added
- Guard clause in `SKILL.md` to prevent over-invocation of the workflow on
  simple tasks (single-file edits, quick fixes, clear requests, etc.).
- "Handling Partial Answers" section in `require-clarification.md` so the
  agent re-asks unanswered questions instead of assuming.

## [1.2.0] - 2026-03-21

### Added
- **Context & Learnings** section in plans so decisions, gotchas, and
  patterns survive across sessions.
- Step isolation (explicit Prerequisites / Deliverables) enabling fresh
  sessions to pick up any step.
- Plan progress reminders after each step to prevent drift.
- Plan drift detection after unplanned work (bug fixes, user adjustments).
- Expanded documentation and workflow structure for context management.

### Changed
- Repo Map discipline is now enforced: a step cannot pass validation or be
  marked COMPLETED until the repo map reflects all touched files.
- Clarification workflow tightened — explicit user confirmation required
  before moving to planning.

## [1.0.0] - 2026-03-21

### Added
- Initial release of the SWE workflow skill for coding agents.
- Core references: `require-clarification`, `create-plan`, `execute-step`,
  `validate-step`, `enforce-tests`, `review-diff`, `protect-code`,
  `persist-plan`, `dump-context`, `maintain-repo-map`,
  `reflect-after-changes`, `global-reflection`, `resume-workflow`.
- Templates: `plan-template.md`, `repo-map-template.md`,
  `context-template.md`.

[Unreleased]: https://github.com/ex-git/swe-workflow/compare/v1.5.1...HEAD
[1.5.1]: https://github.com/ex-git/swe-workflow/compare/v1.5.0...v1.5.1
[1.5.0]: https://github.com/ex-git/swe-workflow/compare/v1.4.2...v1.5.0
[1.4.2]: https://github.com/ex-git/swe-workflow/compare/v1.4.1...v1.4.2
[1.4.0]: https://github.com/ex-git/swe-workflow/compare/v1.3.1...v1.4.0
[1.3.1]: https://github.com/ex-git/swe-workflow/compare/v1.3.0...v1.3.1
[1.3.0]: https://github.com/ex-git/swe-workflow/compare/v1.2.1...v1.3.0
[1.2.1]: https://github.com/ex-git/swe-workflow/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/ex-git/swe-workflow/compare/v1.0.0...v1.2.0
[1.0.0]: https://github.com/ex-git/swe-workflow/releases/tag/v1.0.0
