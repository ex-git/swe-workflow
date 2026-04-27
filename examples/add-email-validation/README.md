# Example: Add Email Validation

This directory contains a realistic, populated set of workflow files for a small feature: adding an email validation utility to a TypeScript project.

It shows what the three `plans/` files look like mid-task — after Step 1 and Step 2 are COMPLETED, Step 3 is IN_PROGRESS, and the agent has just dumped context before asking the user whether to continue.

## Files

| File | What it shows |
|---|---|
| [`plan.md`](plan.md) | A complete task plan with 4 steps, Context & Learnings filled in, Implementation Log, and Deviations |
| [`repo-map.md`](repo-map.md) | Project-wide file inventory, Core + Related files, architecture notes |
| [`context.md`](context.md) | Session snapshot captured before pausing for user input |

## How to use

Read these as examples of what **good** workflow files look like after a few steps of real work. They're not templates — see the Plan Format section in [`SKILL.md`](../../SKILL.md#plan-format--required-structure) for that. They're models of the level of detail and structure that makes a plan useful for a resuming agent.

## Scenario

> **User request:** "Add a function that validates email addresses, with proper error messages for the different failure cases."

The agent ran `require-clarification`, got answers about scope (standalone utility, not integrated into any specific feature yet) and error shape (`{ valid: boolean; error?: string }`), then created a 4-step plan. Steps 1–2 are done, Step 3 is in progress.
