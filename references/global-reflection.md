# Global Reflection

## Overview

Full feature-level review before declaring done. Step back and see the whole picture. This is the last gate before delivery.

## When to Use

When ALL steps in the plan are marked COMPLETED. This runs once at the end.

## Instructions

1. **Re-read the entire plan file:**
   - Use `read` tool on `plans/<name>.md`
   - Verify all steps are COMPLETED
   - Check Implementation Log for context
   - Note any Deviations from Original Plan
   - Review Repo Map for completeness

2. **Re-read ALL files created or modified:**
   - Use `read` tool for each file
   - Don't skip "small" changes
   - Read in the order changes were made

3. **Evaluate the implementation:**

   **Goal alignment:**
   - [ ] Does this achieve the stated Goal?
   - [ ] Are all requirements met?
   - [ ] Is the solution complete?
   
   **Repo Map:**
   - [ ] Are all Core Files listed?
   - [ ] Are all Related Files listed?
   - [ ] Are Key Directories documented?
   - [ ] Is the map accurate and complete?
   
   **Architecture:**
   - [ ] Is the overall design clean?
   - [ ] Could it be simpler?
   - [ ] Are abstractions appropriate? (not too abstract, not too concrete)
   - [ ] Is the file/module organization sensible?
   
   **Code quality:**
   - [ ] Is there technical debt that should be addressed now?
   - [ ] Is error handling sufficient?
   - [ ] Are edge cases covered?
   - [ ] Is naming clear throughout?
   
   **Tests:**
   - [ ] Are all tests passing?
   - [ ] Is test coverage adequate?
   - [ ] Do tests cover edge cases?
   - [ ] Would another developer understand the tests?

4. **Check for overlooked issues:**
   - [ ] Any commented-out code?
   - [ ] Any TODO/FIXME comments remaining?
   - [ ] Any debugging statements left?
   - [ ] Any hardcoded values that should be configurable?
   - [ ] Any security concerns?
   - [ ] Any performance concerns?

5. **Final question:**
   > Would you be comfortable handing this to another developer to maintain?
   
   If not, fix the uncomfortable parts.

## Evaluation Template

```markdown
## Global Reflection

### Goal Achievement
- Goal: [restated goal]
- Status: [ACHIEVED / PARTIAL / FAILED]
- Evidence: [how we know]

### Files Reviewed
- [x] path/to/file1.ts
- [x] path/to/file2.ts
- [x] ...

### Architecture Assessment
- Design: [clean / needs work]
- Concerns: [any issues]

### Code Quality Assessment
- [ ] No commented-out code
- [ ] No TODO/FIXME remaining
- [ ] No debugging statements
- [ ] Error handling sufficient
- [ ] Edge cases covered

### Test Assessment
- Coverage: [adequate / needs more]
- All passing: [yes / no]
- Edge cases: [covered / missing X]

### Issues Found
1. [Issue]: [Sev: high/medium/low] - [Action taken]

### Final Assessment
- Ready for delivery: [YES / NO]
- Developer comfort level: [HIGH / MEDIUM / LOW]
- Notes: [any additional context]
```

## If Issues Found

**Minor issues** (typos, small improvements):
- Fix directly
- Update plan notes with what was fixed
- Continue to completion

**Major issues** (architecture, missing features):
- Create new plan steps
- Execute them through the full workflow
- Don't declare done until resolved

**Critical issues** (security, data loss risk):
- STOP
- Flag clearly to user
- Don't proceed until resolved

## Constraints

- **Do not introduce regressions** during final refactoring — run all tests after changes
- **Do not add features** that weren't in the plan — scope creep is real
- **If major issues found**: flag them clearly to the user rather than silently patching
- **This is the last gate before delivery** — take it seriously
- **Do not rely on memory** — re-read actual files
- **Do not skip because "it's a small feature"** — bugs hide in small features

## Common Issues to Check

| Category | Check |
|----------|-------|
| Security | Input validation, auth checks, SQL injection, XSS |
| Performance | N+1 queries, unnecessary loops, memory leaks |
| Edge cases | Empty inputs, null values, max sizes, concurrent access |
| Error handling | Try/catch, error messages, fallbacks |
| Documentation | README updates, inline comments, API docs |
| Config | Environment variables, defaults, feature flags |

## After Global Reflection

If all checks pass:

1. Consider the feature **DONE**
2. Consider creating a commit:
   ```bash
   git add -A
   git commit -m "feat: [feature description]"
   ```
3. Archive or close the plan if desired

If issues were found and fixed:

1. Update Implementation Log with final fixes
2. Re-run any affected tests
3. Consider another quick reflection pass

## Mandatory Checklist

Before declaring feature done:

- [ ] Re-read entire plan file
- [ ] Re-read ALL modified/created files
- [ ] Verified Goal is achieved
- [ ] Repo Map is accurate and complete
- [ ] Architecture is clean
- [ ] No critical technical debt remaining
- [ ] All tests passing
- [ ] Error handling sufficient
- [ ] No debugging artifacts remaining
- [ ] Comfortable handing to another developer

## This is the End

After global reflection passes:

**Feature is COMPLETE.**

No more references to load. The workflow is finished.

Optional next steps (not part of workflow):
- Create a pull request
- Update documentation
- Communicate completion to stakeholders
- Archive the plan file