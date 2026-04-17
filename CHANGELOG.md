# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

_(nothing yet)_

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

[Unreleased]: https://github.com/ex-git/swe-workflow/compare/v1.3.0...HEAD
[1.3.0]: https://github.com/ex-git/swe-workflow/compare/v1.2.1...v1.3.0
[1.2.1]: https://github.com/ex-git/swe-workflow/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/ex-git/swe-workflow/compare/v1.0.0...v1.2.0
[1.0.0]: https://github.com/ex-git/swe-workflow/releases/tag/v1.0.0
