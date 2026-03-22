# Require Clarification

## ⚠️ THIS IS THE FIRST STEP

**Before any planning, before any coding, you MUST clarify the request.**

```
┌─────────────────────────────────────────────────────────────────┐
│  USER REQUEST RECEIVED                                          │
│         ↓                                                       │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  STOP. Do NOT start coding.                               │  │
│  │  STOP. Do NOT start planning.                             │  │
│  │                                                           │  │
│  │  INVOKE require-clarification FIRST                       │  │
│  │  → Ask questions                                          │  │
│  │  → Understand scope, inputs, outputs, success criteria    │  │
│  │  → Get user confirmation                                  │  │
│  │  → ONLY THEN proceed to create-plan                       │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

## Overview

Ensure the request is fully understood before any planning or coding begins. Ambiguity caught here saves hours of wasted work later.

## When to Use

Load this reference when:
- **Receiving ANY new task or feature request** (this is the first step)
- The request seems vague or underspecified
- You're tempted to start coding immediately
- Multiple interpretations are possible

## Before Starting: Check for Active Plans

Before clarifying a new request, check if there's an existing plan in progress:

1. **Check for existing plans:**
   ```bash
   ls plans/*.md 2>/dev/null
   ```

2. **If a plan exists:**
   - Read the plan to check status
   - If IN_PROGRESS steps exist: **remind user of active plan**
   - Ask: "You have an active plan for [goal] with [N] steps remaining. Should I continue that, or switch to this new request?"
   - Do NOT assume user wants to abandon existing work

3. **If plan exists but user wants to switch:**
   - Ask if they want to pause or close the current plan
   - Document the status of incomplete work
   - Then proceed with new request clarification

4. **If no plan exists:**
   - Proceed with clarification as normal

## Instructions

1. **Analyze the request** for completeness:
   - Is the scope defined? (what's included, what's NOT included)
   - Are inputs and outputs clear?
   - Are constraints specified? (performance, compatibility, dependencies)
   - Is success criteria stated? (how do we know it's done?)

2. **Read the codebase first:**
   - Check existing patterns and conventions
   - Look for related functionality
   - Understand the architecture context
   - Only ask questions you CAN'T answer by reading

3. **If ANY ambiguity exists:**
   - Ask specific, targeted questions
   - Prefer multiple-choice when possible (easier to answer)
   - Ask ONE question at a time — do not overwhelm
   - WAIT for response before proceeding

4. **After receiving an answer, ANALYZE it:**
   - Does this answer fully resolve the question?
   - Did it introduce new requirements or change scope?
   - Did it add details that need clarification themselves?
   - If the answer changes understanding → update your mental model and check if previous assumptions are still valid
   - If new ambiguity introduced → ask follow-up question
   - Only proceed to step 5 when answers are complete and don't generate new questions

5. **Summarize and confirm understanding:**
   - Summarize your complete understanding back to the user
   - Include: scope, inputs/outputs, success criteria, key decisions
   - Explicitly state any assumptions you're making
   - Ask: "Is this understanding correct? Ready to proceed to planning?"
   - WAIT for explicit user confirmation before moving to create-plan

## Mandatory Checklist

Before marking this reference complete, verify:

- [ ] All scope boundaries are defined
- [ ] Inputs and outputs are specified
- [ ] Success criteria are clear and testable
- [ ] No assumptions are being made about unclear requirements
- [ ] All answers analyzed for scope changes and new requirements
- [ ] User has confirmed your understanding explicitly

## Constraints

- **Never assume missing requirements** — ask instead
- **Never skip to planning with open questions**
- **Analyze every answer** — check if it introduces new requirements or changes scope
- **Read the codebase first** — don't ask questions you can answer yourself
- If user says "just do it": acknowledge urgency but still flag the top 1-2 risks of ambiguity
- One question at a time — don't overwhelm the user
- **Must get explicit confirmation** — no confirmation = no plan

## Red Flags — You're Skipping This Phase

If you catch yourself doing any of these, STOP and go back to clarification:

| Red Flag | Correct Action |
|----------|----------------|
| Starting to write a plan without understanding full scope | Ask clarifying questions first |
| Saying "I'll assume X" for anything non-trivial | Ask instead of assuming |
| Jumping straight to code after reading a one-line request | Read codebase, then ask questions |
| Not reading the codebase before asking questions | Read codebase first to understand context |
| Treating "user answered my question" as "all questions resolved" | Analyze if answer introduces new questions |
| Proceeding to create-plan without explicit user confirmation | Get explicit "yes, proceed" first |

## Wrong vs Right

### ❌ WRONG: Skipping Clarification

```
User: "Remove the Slack Integration tab and move auto-replier to Listeners"

Agent: "I'll create a quick plan and start implementing:
       # Plan: Remove Slack Integration Tab
       ## Steps
       1. Remove nav item
       2. Delete page
       3. Add to listeners
       [starts coding...]"
```

**Problems:**
- No clarification
- No scope understanding
- No success criteria
- Will likely miss edge cases

### ✓ RIGHT: Clarifying First

```
User: "Remove the Slack Integration tab and move auto-replier to Listeners"

Agent: "Before I plan this, I need to clarify a few things:

       1. What about the Channel Digest feature that's currently on the Slack tab?
          a) Remove it entirely
          b) Move it somewhere else (where?)
          c) Keep it but different UI

       2. For auto-replier, should it work the same way for Slack and Google Chat?
          - Same rules UI? Separate configuration per platform?

       3. The Task Listener redirect functionality - should that be removed too?"

[Wait for user response, then summarize understanding and get confirmation]
```

## Common Questions to Ask

### For Feature Changes
- What should happen to existing data/configurations?
- Is this backwards compatible?
- What's the migration path?
- Are there related features that need updating?

### For Refactoring
- What's the desired end state?
- Are there parts that should NOT change?
- Is this incremental or one-shot?
- What tests need to pass?

### For Bug Fixes
- What's the expected vs actual behavior?
- When did it start happening?
- Are there workarounds in place?
- What's the impact severity?

### For New Features
- What triggers the feature?
- What are the inputs and outputs?
- Are there permission/auth requirements?
- What happens on error?

## Next Step

→ Proceed to: **create-plan** (break down into structured steps)

**Only proceed when:** All questions are resolved and user has confirmed your understanding.

**DO NOT proceed if:** You still have open questions or haven't gotten explicit confirmation.