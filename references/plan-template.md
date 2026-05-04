# Plan Template

> Copy this file verbatim to `plans/<task>.md` for every new Full-workflow task. Do not reconstruct from memory. Do not invent your own format. All labeled sections and per-step fields below are required.

---

```markdown
# Plan: [Task Name]

> Status: DRAFT | IN_PROGRESS | COMPLETED
> Created: YYYY-MM-DD
> Last Updated: YYYY-MM-DD

## Goal
[One sentence describing what "done" looks like.]

## Assumptions
- [What we're taking for granted — verify or make explicit.]

## Open Questions
None.

## Context & Learnings
### Key Decisions
- [Decision]: [Rationale]
### Gotchas & Warnings
- [Warning]: [What to watch out for]
### Working Set
| Path | Role in this task | Evidence |
|------|-------------------|----------|
| [path] | [why this file matters] | [read/rg/test/config check used to verify] |
### Verified Facts
- [Fact] — verified by [tool/read/search/config], [date or step].

## Steps

### Step 1: [Title]
**Status:** PENDING | IN_PROGRESS | COMPLETED | BLOCKED
**Prerequisites:**
- [What must be true before starting.]
**Deliverables:**
- [What this step produces.]
**Plan:**
- [ ] [Specific action.]
**Validation Checklist:**
- [ ] [How to verify the step works.]
**Test Checklist:**
- [ ] [What tests to write or run. If no test framework exists, write `N/A`.]
**Implementation Notes:**
[Fill after implementation — what actually happened, surprises, deviations.]
**Files Changed:**
[List exact paths after implementation.]

<!-- Repeat the full block above for each step. -->

## Implementation Log
| Date | Step | Summary |
|------|------|---------|
```

## Required fields checklist

For `## Open Questions`, the valid content is exactly `None.`. If any known question would be listed there, do not create or finalize the plan; ask the user in chat first. `DRAFT` means plan review/approval after clarification is complete, not unresolved requirements.

Before marking any step `COMPLETED`, confirm the step has all seven fields populated:

- [ ] `Status` — one of `PENDING`, `IN_PROGRESS`, `COMPLETED`, `BLOCKED`
- [ ] `Prerequisites`
- [ ] `Deliverables`
- [ ] `Plan` (action checklist)
- [ ] `Validation Checklist`
- [ ] `Test Checklist` (use `N/A` if no test framework exists)
- [ ] `Files Changed`

The top-level plan file must also have:

- [ ] Header block with `Status`, `Created`, `Last Updated`
- [ ] `Goal`, `Assumptions`, `Open Questions` (exactly `None.` before plan creation/finalization and execution)
- [ ] `Context & Learnings`, including `Working Set` and `Verified Facts`
- [ ] `Implementation Log` table (appended to as steps complete)

## Violations to avoid

- `### Step N — Title [STATUS]` headers with inline status brackets instead of a `**Status:**` field.
- Steps missing any of the seven required fields.
- Horizontal slicing (Step 1 = all types, Step 2 = all logic, Step 3 = all tests). Each step must be a thin vertical slice through the necessary layers — independently verifiable.
- Marking a step `COMPLETED` with known validation or test failures. Either fix them or mark `BLOCKED`.
- Recording guessed paths, APIs, dependencies, or conventions as facts without read/search/tool evidence.
- Listing unresolved requirement/design questions under `## Open Questions` instead of asking them in chat before planning.
- Adding a step whose purpose is to resolve open questions. Clarification is a pre-plan gate, not implementation work.
- Treating `DRAFT` as permission to leave requirements unresolved. `DRAFT` is only for plan review/approval after clarification is complete.
- Batching multiple steps in a single execution pass.
