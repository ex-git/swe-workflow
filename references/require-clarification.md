# Require Clarification

## Overview

Ensure the request is fully understood before any planning or coding begins. Ambiguity caught here saves hours of wasted work later.

## When to Use

Load this reference when:
- Receiving any new task or feature request
- The request seems vague or underspecified
- You're tempted to start coding immediately
- Multiple interpretations are possible

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

4. **Once all questions are resolved:**
   - Summarize your understanding back to the user
   - Get explicit confirmation before moving to planning

## Mandatory Checklist

Before marking this reference complete, verify:

- [ ] All scope boundaries are defined
- [ ] Inputs and outputs are specified
- [ ] Success criteria are clear and testable
- [ ] No assumptions are being made about unclear requirements
- [ ] User has confirmed your understanding

## Constraints

- **Never assume missing requirements** — ask instead
- **Never skip to planning with open questions**
- **Read the codebase first** — don't ask questions you can answer yourself
- If user says "just do it": acknowledge urgency but still flag the top 1-2 risks of ambiguity
- One question at a time — don't overwhelm the user

## Red Flags — You're Skipping This Phase

- Starting to write a plan without understanding the full scope
- Saying "I'll assume X" for anything non-trivial
- Jumping straight to code after reading a one-line request
- Not reading the codebase before asking clarifying questions

## Next Step

→ Proceed to: **create-plan** (break down into structured steps)

**Only proceed when:** All questions are resolved and user has confirmed your understanding.