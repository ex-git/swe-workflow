# Enforce Tests

## Overview

Every step must have tests. No exceptions. Untested code is unfinished code.

## When to Use

After passing validate-step, before marking a step COMPLETED.

## Instructions

1. **Define expected behavior** for this step:
   - What are the inputs?
   - What should the outputs be?
   - What should happen on invalid input?

2. **Write tests** that verify the behavior:
   - Happy path (normal operation)
   - Edge cases (empty input, boundaries, nulls)
   - Error cases (invalid input, missing dependencies)

3. **Run the tests** — they must pass:
   ```bash
   # JavaScript/TypeScript
   npm test
   
   # Python
   pytest
   
   # Rust
   cargo test
   
   # Go
   go test ./...
   ```

4. **If no test framework exists:**
   - Document manual verification steps in the plan
   - Include: exact commands to run, expected output
   - This is acceptable for config changes, docs, or infrastructure

5. **Update the plan:**
   - Check off items in Test Checklist
   - Document any test-specific decisions in Implementation Notes

## Test Categories

### For New Functions/Modules
- [ ] Unit tests for main functionality
- [ ] Edge case tests
- [ ] Error handling tests
- [ ] Type tests (if TypeScript)

### For Bug Fixes
- [ ] Regression test that fails before fix
- [ ] Test that passes after fix
- [ ] Related edge cases

### For API Changes
- [ ] Integration tests for endpoints
- [ ] Request validation tests
- [ ] Response format tests
- [ ] Error response tests

### For Configuration Changes
- [ ] Verify config is applied correctly
- [ ] Test with valid config values
- [ ] Test with invalid config values (should fail gracefully)

### For Documentation
- Manual verification acceptable
- Document: "Read docs, verified accuracy against code"

## Writing Good Tests

### DO:
- Test behavior and outcomes, not implementation details
- Make tests independent (no manual setup required)
- Use descriptive test names (`should return error for invalid email`)
- Test one thing per test case

### DO NOT:
- Write tests that just assert function exists
- Copy production code into tests
- Create interdependent tests
- Skip tests because "it's simple code"

## If Tests Fail

1. Determine if the test is wrong or implementation is wrong
2. If implementation is wrong: fix it, update Implementation Notes
3. If test is wrong: fix the test
4. If unexpected behavior discovered: update plan to reflect reality
5. Re-run tests until all pass

## Constraints

- **A step CANNOT be marked COMPLETED without tests or documented manual verification**
- **Do not write tests that just assert the implementation exists** — test behavior and outcomes
- **Tests must be runnable independently** — no manual setup required
- **If writing a test reveals a bug**: fix the implementation, update notes
- Tests written now prevent bugs later — don't skip

## Manual Verification Template

If no test framework exists, document in plan:

```markdown
**Test Checklist:**
- [x] Manual: ran `node scripts/example.js` → expected output "success"
- [x] Manual: tested with invalid input → error message shown
- [x] Manual: verified UI renders correctly in browser
```

## Mandatory Checklist

Before proceeding to diff review:

- [ ] Tests written for the step's functionality
- [ ] Happy path covered
- [ ] Edge cases covered
- [ ] All tests pass
- [ ] Test Checklist items checked off in plan
- [ ] (If no framework) Manual verification documented

## Next Steps

After tests pass:

1. → Proceed to: **review-diff** (critical review of all changes)
2. → Then: **maintain-repo-map** (update file locations discovered)
3. → Then: **persist-plan** (mark step COMPLETED in plan)