# Review Diff

## Overview

Critically review your own changes. Assume you made mistakes — because you probably did.

## When to Use

After passing enforce-tests, before marking a step COMPLETED.

## Instructions

1. **View the actual diff** of changed files:
   ```bash
   # If using git
   git diff
   git diff --staged
   
   # If not using git, compare against snapshots
   # (created during protect-code step)
   ```

2. **Check for problems systematically:**
   
   **Unnecessary edits:**
   - [ ] Formatting-only changes to unrelated files
   - [ ] Imports reorganized but not used
   - [ ] Whitespace changes mixed with logic changes
   - [ ] File renames without reason
   
   **Bad patterns:**
   - [ ] Hardcoded values that should be configurable
   - [ ] Magic numbers without explanation
   - [ ] Poor variable/function names
   - [ ] Deep nesting (more than 3 levels)
   - [ ] Long functions (more than 30 lines)
   
   **Duplication:**
   - [ ] Copy-pasted code that should be extracted
   - [ ] Similar logic in multiple places
   - [ ] Repeated constants
   
   **Missing handling:**
   - [ ] No error handling for operations that can fail
   - [ ] No null/undefined checks where needed
   - [ ] No edge case handling
   
   **Style inconsistency:**
   - [ ] Doesn't match existing codebase style
   - [ ] Inconsistent naming conventions
   - [ ] Mixed quote styles, spacing, etc.
   
3. **Check for artifacts:**
   - [ ] Remove `console.log` / `print` debugging statements
   - [ ] Remove `TODO` / `FIXME` comments (unless intentional)
   - [ ] Remove commented-out code
   - [ ] Remove unused imports
   - [ ] Remove test/debug configuration

4. **Verify scope:**
   - [ ] Only planned files were modified
   - [ ] No unexpected changes appeared
   - [ ] No files were accidentally deleted

5. **If improvements needed:** fix them NOW, before marking complete

## Diff Review Template

```markdown
## Files Changed
- path/to/file1.ts: [summary of changes]
- path/to/file2.ts: [summary of changes]

## Scope Check
- [ ] All changes match the plan
- [ ] No unrelated modifications

## Code Quality Check
- [ ] No debugging statements left
- [ ] No commented-out code
- [ ] Follows code style

## Issues Found
1. [Issue]: [Fix applied / Reason acceptable]

## Final Assessment
[Ready for commit / Needs fixes]
```

## Common Issues to Watch For

| Issue | Example | Fix |
|-------|---------|-----|
| Debugging code | `console.log("here")` | Remove |
| Magic numbers | `if (x > 100)` | Use named constant |
| Poor names | `let temp = ...` | Use descriptive name |
| Missing errors | `JSON.parse(data)` | Add try/catch |
| Copy-paste | Same 10 lines twice | Extract function |
| Hidden changes | Formatting + logic in one change | Separate commits |

## Constraints

- **Be critical** — your first draft is rarely your best
- **Do not approve without reading the actual diff** — not from memory
- **Use `git diff` or file comparison** — don't just look at files
- If the diff is larger than expected: investigate why, scope may have crept
- Fix issues NOW, not "later"

## Mandatory Checks

Before proceeding to persist-plan:

- [ ] Reviewed actual diff (not from memory)
- [ ] Only planned changes present
- [ ] No debugging artifacts
- [ ] No commented-out code
- [ ] Style matches existing code
- [ ] No copy-paste duplication
- [ ] Error handling present where needed

## Next Steps

After diff review passes:

1. → Proceed to: **persist-plan** (mark step COMPLETED)
2. → Then: Check if more steps remain
   - If yes: → **execute-step** (next PENDING step)
   - If no: → **global-reflection** (feature complete)