# Example: Add Email Validation

This directory contains a realistic, populated set of workflow files for a small feature: adding an email validation utility to a TypeScript project. The files demonstrate the **directory-based plan structure** introduced in v1.10.0.

It shows what Full workflow files look like mid-task — after Step 1 and Step 2 are COMPLETED, Step 3 is IN_PROGRESS, and the agent has just dumped context before asking the user whether to continue.

## Directory Layout

```
examples/add-email-validation/
├── README.md                                    # this file
├── context.md                                   # session snapshot
└── 2024-11-04-add-email-validation/             # task directory (date-prefixed slug)
     ├── plan.md                                 # task overview
     └── steps/
          ├── step-1.md                          # Define error codes (COMPLETED)
          ├── step-2.md                          # Implement validateEmail (COMPLETED)
          ├── step-3.md                          # Test suite (IN_PROGRESS)
          └── step-4.md                          # Docs + CHANGELOG (PENDING)
```

## Files

| File | What it shows |
|---|---|
| [`2024-11-04-add-email-validation/plan.md`](2024-11-04-add-email-validation/plan.md) | Task overview: Goal, Assumptions, Design Decisions, Steps Overview table, Working Set, Verified Facts, Implementation Log |
| [`2024-11-04-add-email-validation/steps/step-1.md`](2024-11-04-add-email-validation/steps/step-1.md) | Completed step — every field filled in with implementation evidence |
| [`2024-11-04-add-email-validation/steps/step-2.md`](2024-11-04-add-email-validation/steps/step-2.md) | Completed step with documented deviation (mega-regex split) |
| [`2024-11-04-add-email-validation/steps/step-3.md`](2024-11-04-add-email-validation/steps/step-3.md) | In-progress step — partial Plan checklist, partial Quality Checklist |
| [`2024-11-04-add-email-validation/steps/step-4.md`](2024-11-04-add-email-validation/steps/step-4.md) | Pending step — fully scoped but not yet started |
| [`context.md`](context.md) | Session snapshot captured before pausing, including active files and evidence |

## How to use

Read these as examples of what **good** workflow files look like after a few steps of real work. They're not templates — copy [`references/task-overview-template.md`](../../references/task-overview-template.md) and [`references/step-template.md`](../../references/step-template.md) for new plans. They're models of the level of detail and structure that makes a plan useful for a resuming agent.

## Scenario

> **User request:** "Add a function that validates email addresses, with proper error messages for the different failure cases."

The agent ran `require-clarification`, got answers about scope (standalone utility, not integrated into any specific feature yet) and error shape (`{ valid: boolean; error?: EmailErrorCode }`), then created a 4-step plan. Steps 1–2 are done, Step 3 is in progress.
