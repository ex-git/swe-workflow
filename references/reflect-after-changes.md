# Reflect After Changes

## Overview

Pause after every 2-3 steps to catch complexity before it compounds. Small refactors NOW prevent large rewrites later.

## When to Use

After every 2-3 COMPLETED steps, before continuing with the next step.

## Instructions

1. **Re-read all files modified** in the recent steps — not from memory:
   - Use `read` tool for each modified file
   - Don't skip "small" changes

2. **Look for warning signs:**

   **Repeated patterns:**
   - [ ] Same logic in multiple places
   - [ ] Similar function structures
   - [ ] Should extract into shared function?
   
   **Unclear naming:**
   - [ ] Variable names need reader to infer context
   - [ ] Function names don't describe behavior
   - [ ] Should rename for clarity?
   
   **Growing complexity:**
   - [ ] Functions longer than 30 lines
   - [ ] Files longer than 300 lines
   - [ ] Should break into smaller functions?
   
   **Tight coupling:**
   - [ ] Changes in one file always require changes in another
   - [ ] Hard to test in isolation
   - [ ] Should introduce interfaces/abstractions?
   
   **Workarounds:**
   - [ ] Comments explaining "why" a hack exists
   - [ ] TODO comments accumulating
   - [ ] Should address root cause?

3. **Refactor if needed:**
   - Extract duplicated code
   - Simplify convoluted logic
   - Improve naming
   - Reduce nesting depth
   - Introduce abstractions for coupling

4. **Update the plan** if reflection leads to structural changes:
   - Add new steps for refactoring
   - Document decisions in Implementation Notes
   - Check if deviation from original plan

## Refactoring Checklist

If refactoring, ensure:

- [ ] Behavior unchanged (tests still pass)
- [ ] No scope creep (just cleaning, not adding features)
- [ ] File count didn't explode (one module per function is wrong)
- [ ] Abstraction level is appropriate (not too deep)
- [ ] Re-run tests after refactoring

## Warning Signs Table

| Warning Sign | What It Means | Action |
|--------------|---------------|--------|
| Same code in 3 places | Duplication | Extract to shared function |
| Function > 50 lines | Complexity | Break into smaller functions |
| Nested if > 3 levels | Confusion | Extract conditions or use guard clauses |
| "temp", "data", "result" | Poor naming | Use descriptive names |
| Many imports from one file | Coupling | Consider interface or facade |
| Comments explaining hacks | Technical debt | Fix root cause |

## When to Add Plan Steps

Reflection may reveal need for plan changes:

```markdown
### Step N+1: Refactor validation module

**Status:** PENDING

**Plan:**
- Extract duplicated email validation logic
- Create shared validation utilities
- Update callers to use shared function

**Reason Added:** Reflection after Step N revealed duplicated code
```

## Constraints

- **Prefer refactoring over patching** — fix the root cause, not the symptom
- **Small refactors NOW** prevent large rewrites later
- **Do not introduce new features** during reflection — only simplify
- **Do not skip** because "we're almost done" — that's when complexity bites hardest
- **Do not rely on memory** — re-read the actual files
- Always re-run tests after refactoring

## Reflection Cadence

```
Step 1 → Step 2 → REFLECT → Step 3 → Step 4 → REFLECT → Step 5 → ...
```

Or after each step if working on complex logic.

## Mandatory Checks

Before continuing to next step:

- [ ] Re-read modified files with `read` tool
- [ ] Checked for repeated patterns
- [ ] Checked for unclear naming
- [ ] Checked for growing complexity
- [ ] Checked for tight coupling
- [ ] Refactored if needed (and ran tests)
- [ ] Updated plan if structure changed

## Next Step

After reflection (and any refactoring):

→ Proceed to: **execute-step** (next PENDING step)

If significant issues found:
→ Update plan with new refactoring steps
→ Then proceed to: **execute-step**