# Definition of Done

Use this file during the final verification phase. Do not mark a task or step complete until all applicable items are satisfied.

## Contract

1. The diff contains only changes required by the task.
2. Modified files were re-read after editing.
3. The actual git diff was reviewed.
4. Relevant validation commands were run, or skipped with reason and risk.
5. Tests were added or updated for changed behavior when applicable.
6. API, data, security, performance, and observability risks were reviewed.
7. The final response lists files changed, validation run, and residual risks.
8. Repo-related final claims distinguish Verified facts, Assumptions, Unknowns, and Recommendations when uncertainty matters.
9. Completion does not hide shortcuts: target reads, relevant impact searches, validation/skipped reasons, and residual risks are visible in notes or final response.

## Scope

- No unrelated refactors.
- No formatting-only churn outside touched code.
- No dependency changes unless explicitly required.
- No public contract changes unless planned and approved.

## Correctness

- The implementation satisfies the success criteria.
- Existing behavior is preserved unless the task explicitly changes it.
- Edge cases and failure cases are handled consistently with nearby code.

## Project Fit

- The change follows local naming, layering, error handling, logging, and testing conventions.
- Existing helpers, components, services, types, and utilities were reused where appropriate.
- New abstractions are justified by current use, not speculative future use.

## Validation

Record:

| Check | Command | Result | Notes |
|---|---|---|---|
| Typecheck |  |  |  |
| Lint |  |  |  |
| Test |  |  |  |
| Build |  |  |  |

If a command is skipped, record why.

For noisy command output, validation evidence may use the compact command-evidence schema instead of pasting raw logs:

- `Command`: exact command run
- `Exit`: exit code
- `Output`: `/tmp/...` file path, or `inline` for small output
- `Summary`: one-line result
- `Freshness`: current workspace/session, stale after relevant changes unless rerun

## Anti-Hallucination Completion Gate

Before saying a step or task is complete, confirm:

- **Target read:** modified files, or the relevant sections, were re-read after changes.
- **Impact search:** callers/usages/contracts were searched when shared behavior changed; otherwise record `N/A — isolated/local change`.
- **No guessed facts:** unverified repo claims are labeled `Assumption`, `Unknown`, or `Recommendation`, not `Verified`.
- **Validation:** checks were run with command evidence, or skipped with reason and residual risk.
- **Visible unknowns:** remaining uncertainty is listed in Implementation Notes or the final response.

## Final Response Requirements

Use this compact shape unless the user asked for a different format:

```markdown
Summary:
- [What changed]

Verified:
- [Key files/commands checked]

Validation:
- [Command/result, or skipped with reason]

Risks/Unknowns:
- [Residual risk, assumption, or `None known`]
```

The final response must include:

- Summary of changes
- Files changed when files changed
- Validation run or skipped reason
- Verified facts (files/commands checked) when making repo claims
- Assumptions or unknowns, if any
- Known limitations or residual risks
