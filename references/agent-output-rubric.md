# Agent Output Rubric

Use this to review completed agent coding work.

Score each category from 1 to 5.

## 1. Workflow Compliance

5 = Correct mode, triage emitted, plan used when required, pre-edit gate respected, one step at a time.  
3 = Mostly followed, but missed minor state or documentation updates.  
1 = Jumped to code, skipped triage, ignored Full workflow trigger, or bypassed plan gate.

## 2. Correctness

5 = Meets success criteria, handles edge/failure cases, preserves existing behavior.  
3 = Happy path works but edge cases are weak.  
1 = Incorrect, incomplete, or breaks existing behavior.

## 3. Project Fit

5 = Matches local architecture, naming, error handling, logging, tests, and dependencies.  
3 = Mostly fits but introduces some inconsistent style or unnecessary abstraction.  
1 = Ignores local patterns or creates new architecture unnecessarily.

## 4. Validation

5 = Relevant tests/checks run and documented; skipped checks have reason and risk.  
3 = Some checks run but coverage incomplete.  
1 = No meaningful validation.

## 5. Safety

5 = Security, data, API, performance, and observability risks handled or explicitly ruled out.  
3 = Some risk review but incomplete.  
1 = No safety review.

## Suggested Interpretation

| Average | Meaning |
|---:|---|
| 4.5–5.0 | Production-reviewable |
| 3.5–4.4 | Useful but needs human review |
| 2.5–3.4 | Risky; likely needs rework |
| <2.5 | Failed workflow |

Do not reference this from the mandatory `SKILL.md` contract. It is for human or meta-review use.
