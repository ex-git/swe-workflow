# Require Clarification

## Overview

Ensure the request is fully understood before any planning or coding begins. Ambiguity caught here saves hours of wasted work later.

## When to Use

Load this reference when:
- Receiving any new task or feature request
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

## Handling Partial Answers

When you ask multiple questions and the user only answers some of them:

| Situation | Wrong Response | Correct Response |
|-----------|----------------|------------------|
| User answered 2 of 4 questions | Assume answers for unanswered questions | Re-ask the unanswered questions |
| User gave vague answer | Interpret what they meant | Ask for clarification |
| User said "figure it out" | Make assumptions | Provide options and ask which to use |
| User skipped a question entirely | Proceed without that answer | Explicitly ask that question again |

**Example:**

```
Agent: "I need to clarify a few things:
       1. What should happen to Channel Digest?
       2. Should auto-replier work the same for Slack and Google Chat?
       3. What about Task Listener?"

User: "Channel Digest should be removed. Auto-replier same behavior."

Agent: "Got it. Two clarifications:
       1. Channel Digest → remove ✓
       2. Auto-replier → same behavior ✓
       
       You didn't mention Task Listener - what should happen to it?
       a) Remove entirely
       b) Move somewhere else
       c) Keep as-is"
```

**DO NOT proceed until ALL questions are answered.**

## Constraints

- **Never assume missing requirements** — ask instead
- **Never skip to planning with open questions**
- **Analyze every answer** — check if it introduces new requirements or changes scope
- **Read the codebase first** — don't ask questions you can answer yourself
- If user says "just do it": acknowledge urgency but still flag the top 1-2 risks of ambiguity
- One question at a time — don't overwhelm the user
- **Must get explicit confirmation** — no confirmation = no plan

## Red Flags — You're Skipping This Phase

- Starting to write a plan without understanding the full scope
- Saying "I'll assume X" for anything non-trivial
- Jumping straight to code after reading a one-line request
- Not reading the codebase before asking clarifying questions
- Treating "user answered my question" as "all questions resolved"
- Not analyzing whether an answer introduces new requirements or changes scope
- Proceeding to create-plan without explicit user confirmation
- **Assuming answers for questions the user didn't answer**
- **Crossing out questions because user didn't mention them**

## Next Step

→ Proceed to: **create-plan** (break down into structured steps)

**Only proceed when:** All questions are resolved and user has confirmed your understanding.