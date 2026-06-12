[$T1=risk-classification.md, $T2=command-discovery.md, $T3=code-quality.md, $T4=steps/step-N.md]
# Execute Step

## Contract — Read This First

1. Read `plans/<YYYY-MM-DD>-<slug>/plan.md`. Confirm `Open Questions` is `None.` — if not, STOP and ask.
2. Select the first PENDING step file (`$T4`). Mark it `IN_PROGRESS`. Update the plan's `Last Updated` and Steps Overview.
3. Re-read target files before editing — do not rely on memory.
4. Evidence-before-edit: target read, impact search (or `N/A — isolated`), validation command (or skipped reason).
5. Protect code: `git commit` or `git stash` before changes.
6. Use `edit` for existing files, `write` for new files. Never `write` over an existing file.
7. Implement ONLY what this step requires. Do not touch unrelated files.
8. If scope needs to expand: STOP, update the plan, notify user. Never silently expand.

## Instructions

### 1. Open Questions Gate

Read the plan overview. If `Open Questions` is not `None.`, stop and ask.

### 2. Select and Mark Step

Find the first PENDING step file with no unmet prerequisites. Read it. Mark `IN_PROGRESS`. Update plan overview's Steps Overview table and `Last Updated`.

### 3. Evidence-Before-Edit

| Check | Record |
|-------|--------|
| Target read | Re-read files being modified (not from memory) |
| Impact search | Search callers/usages of shared behavior; `N/A — isolated` if local |
| Validation identified | Discover commands via [`$T2`]($T2), or record `skipped — <reason>` + risk |
| Conventions | Check imports/exports and nearby code for local patterns |
| Reuse check | Search for existing equivalents before creating new code |

Record evidence in Working Set and Verified Facts.

### 4. Quality Gate

- Identify the local pattern this change should follow
- Identify the contract: inputs, outputs, errors, side effects
- Classify risk with [`$T1`]($T1) and record mitigation for non-none risks
- If design-sensitive (UI, schema, API): verify approach matches conventions or Design Decisions table; ask if ambiguous

### 5. Protect Code

```bash
git add -A && git commit -m "WIP: before step N"
```

Tool rules: `edit` for changes, `write` for new files only.

### 6. Implement

Apply [`references/$T3`]($T3) throughout. Key constraints:

- Stay within scope — only files in the step's plan
- Follow existing formatting, naming, conventions
- Do not add unrequested features, abstractions, or dependencies
- Handle edge cases and errors consistently with the project
- Do not complete with unverified imports, packages, or APIs

### 7. Update Evidence

- Add modified files to Working Set with role and evidence
- Record facts in Verified Facts with tool proof
- Fill Implementation Notes and Files Changed in the step file

## Scope Changes During Execution

| Type | Action |
|------|--------|
| **Direct dependency** (step can't complete without it) | Small → include and document. Significant → STOP, add prerequisite step, notify user. |
| **Optional improvement** | Note in Implementation Notes as "Future consideration." Do NOT implement. |
| **Separate concern** | Add as new step. Complete current step first. |
| **Plan drift signals** | Bug fix took 30+ min, 3+ user adjustments, steps seem wrong → pause, re-assess, communicate. |

### Blocker

Mark `BLOCKED`, document reason + unblock path, proceed to next safe step or report.

## Mandatory Checklist

- [ ] Code changes complete and in scope
- [ ] Evidence-before-edit recorded
- [ ] Working Set and Verified Facts updated
- [ ] Implementation Notes and Files Changed filled
- [ ] Design choices confirmed (if design-sensitive)
- [ ] Deviations documented

## Next Step

→ **verify-step** → **persist-plan** → next step or **global-reflection**
