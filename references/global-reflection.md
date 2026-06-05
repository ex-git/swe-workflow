# Global Reflection

## Contract — Read This First

1. Run once when ALL steps are COMPLETED — this is the last gate before delivery.
2. Re-read the plan overview AND every step file AND every modified/created file with `read`.
3. Verify goal achieved, evidence accurate, architecture clean, no artifacts.
4. Verify `plan.md` Steps Overview and step files agree on final state.
5. Fix issues found. Minor → fix directly. Major → create new steps. Critical → STOP and flag.
6. Would you hand this to another developer to maintain? If not, fix the uncomfortable parts.

## Instructions

1. **Re-read the plan overview** (`plans/<YYYY-MM-DD>-<slug>/plan.md`) — verify all entries in Steps Overview are COMPLETED, review Implementation Log, check deviations, review Working Set/Verified Facts for evidence gaps, review Context & Learnings for completeness.

2. **Re-read every step file** (`plans/<YYYY-MM-DD>-<slug>/steps/step-N.md`) — verify each has `Status: COMPLETED`, all 10 fields populated, Implementation Notes filled, Files Changed accurate.

3. **Verify plan/step consistency** — confirm every step file status matches the Steps Overview table and all steps are `COMPLETED`.

4. **Re-read ALL modified/created files** with `read` — in the order changes were made.

5. **Evaluate:**

   | Area | Check |
   |------|-------|
   | **Goal** | Achieved? All requirements met? Solution complete? |
   | **Evidence** | Working Set accurate? Verified Facts still true? Files Changed complete? |
   | **Architecture** | Clean design? Could be simpler? Appropriate abstractions? Sensible file organization? |
   | **Code quality** | Technical debt? Error handling sufficient? Edge cases covered? Clear naming? |
   | **Tests** | All passing? Adequate coverage? Edge cases covered? |
   | **Artifacts** | Commented-out code? TODO/FIXME? Debug statements? Hardcoded values? |
   | **Security** | Input validation? Auth checks? Injection risks? |
   | **Performance** | N+1 queries? Unnecessary loops? Memory leaks? |

4. **Fix what you find:**

   | Severity | Action |
   |----------|--------|
   | Minor (typos, small improvements) | Fix directly, update plan notes |
   | Major (architecture, missing features) | Create new plan steps, execute through full workflow |
   | Critical (security, data loss risk) | STOP, flag to user, do not proceed |

## Mandatory Checklist

- [ ] Re-read entire plan overview (`plan.md`)
- [ ] Re-read every step file (`steps/step-N.md`)
- [ ] Verified `plan.md` Steps Overview matches step file statuses
- [ ] Re-read ALL modified/created files
- [ ] Goal achieved with evidence
- [ ] Context & Learnings complete (decisions, gotchas, patterns)
- [ ] Working Set, Verified Facts, Files Changed accurate
- [ ] Architecture clean, no unnecessary complexity
- [ ] No artifacts (debug statements, commented code, stale TODOs)
- [ ] All tests passing
- [ ] Error handling sufficient
- [ ] Comfortable handing to another developer

## After Reflection

If all checks pass:
1. Feature is **DONE**.
2. Update plan overview: set `Status: COMPLETED` and update `Last Updated`.
3. Consider: `git add -A && git commit -m "feat: [description]"`

If issues found and fixed: update Implementation Log, re-run affected tests.

## This is the End

No more references to load. The workflow is finished.
