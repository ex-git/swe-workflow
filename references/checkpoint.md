[$T1=plans/context.md]
# Checkpoint

> Use every 2–3 completed steps and before any session pause.

## Contract

1. Re-read all recently modified files — not from memory.
2. Check for complexity drift: duplication, naming, nesting, coupling.
3. Verify remaining PENDING steps still make sense.
4. Write `$T1` before any pause that might span sessions.

## Reflect on Recent Changes

1. **Re-read files modified** in recent steps with `read`.

2. **Check for warning signs:**

   | Sign | Threshold | Action |
   |------|-----------|--------|
   | Same logic in multiple places | 3+ occurrences | Extract to shared function |
   | Function length | >30 lines | Break into smaller functions |
   | Nesting depth | >3 levels | Guard clauses or extract |
   | Poor names | `temp`, `data`, `result` | Rename for clarity |
   | Hack comments accumulating | TODO/FIXME | Fix root cause |

3. **Refactor if needed** — extract, simplify, rename. Run tests after. No scope creep.

4. **Verify remaining plan:**
   - Re-read PENDING steps — do they still fit current implementation?
   - **Drift signals:** remaining steps reference changed code, descriptions seem outdated, previous steps took much longer than expected.
   - If drifted: summarize to user, propose updates, get confirmation.

## Dump Context Before Pause

Write `$T1` with:

```markdown
# Session Context

> Last Updated: [YYYY-MM-DD HH:MM]

## Current Task
[One sentence]

## Completed Steps
| Step | Summary | Files Changed |
|------|---------|---------------|

## Current Step
**Step N: [Title]** — Status: [IN_PROGRESS|PENDING]
- What was done
- Decisions made

## Key Learnings
- [Gotcha or pattern]

## Next Actions
1. [Ordered next steps]
```

Do NOT include full file contents, detailed diffs, or temporary analysis — reference paths only.

## After Checkpoint

Print brief progress, ask question or continue:
> "Completed Steps 1-N. Context saved. Continue with [next action]?"

## Next Step

→ **execute-step** (next PENDING) or ask user.
