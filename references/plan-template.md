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
> Must be empty before implementation begins.

## Context & Learnings
### Key Decisions
- [Decision]: [Rationale]
### Gotchas & Warnings
- [Warning]: [What to watch out for]

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
- [ ] `Goal`, `Assumptions`, `Open Questions` (empty before execution)
- [ ] `Context & Learnings`
- [ ] `Implementation Log` table (appended to as steps complete)

## Violations to avoid

- `### Step N — Title [STATUS]` headers with inline status brackets instead of a `**Status:**` field.
- Steps missing any of the seven required fields.
- Horizontal slicing (Step 1 = all types, Step 2 = all logic, Step 3 = all tests). Each step must be a thin vertical slice through the necessary layers — independently verifiable.
- Marking a step `COMPLETED` with known validation or test failures. Either fix them or mark `BLOCKED`.
- Batching multiple steps in a single execution pass.
