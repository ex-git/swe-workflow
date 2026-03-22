# Dump Context

## Overview

Preserve session state before pausing for user input. This allows future sessions to resume with full context, including work done, decisions made, and current state.

## When to Use

- Before asking user "Should I continue?"
- After completing 2-3 steps
- When significant decisions have been made
- Before any pause that might span sessions

## Why Dump Context?

- **Resume seamlessly** — Next session picks up exactly where you left off
- **No memory loss** — All decisions, discoveries, and progress preserved
- **Better handoffs** — Another agent can understand current state
- **Context limits** — Large projects accumulate context; dumping frees up space

## Instructions

1. **Create contexts directory if needed:**
   ```bash
   mkdir -p plans/
   ```

2. **Determine context filename:**
   - Use: `plans/context.md`
   - This file always represents the latest context

3. **Write context file using the `write` tool:**

```markdown
# Session Context

> Last Updated: [YYYY-MM-DD HH:MM]
> Session: [Brief session description]

## Current Task

[One sentence describing what you're working on]

## Completed Steps

| Step | Summary | Files Changed |
|------|---------|---------------|
| [Step N] | [Brief description] | [file1, file2] |

## Current Step

**Step N: [Title]**
**Status:** IN_PROGRESS | PENDING

### What Was Done
- [Action taken]
- [Next action planned]

### Decisions Made
- [Decision]: [Rationale]

### Active Files
| Path | Status | Purpose |
|------|--------|---------|
| [path] | [reading/modifying/created] | [why it matters] |

## Key Learnings

### Gotchas
- [Thing that caused issues]: [How to avoid/handle]

### Patterns
- [Pattern discovered]: [Where it applies]

### Warnings
- [Important warning]: [Context]

## Open Questions

> Questions waiting for user input

- [ ] [Question 1]
- [ ] [Question 2]

## Next Actions

> Ordered list of immediate next steps

1. [Next action]
2. [Following action]
3. [And so on]

## Repo Map Summary

> Quick reference to key files (full map in plans/repo-map.md)

| Path | Purpose |
|------|---------|
| [key file 1] | [what it's for] |
| [key file 2] | [what it's for] |
```

4. **Verify context was written:**
   - Use `read` tool to confirm the file exists and is complete
   - Ensure all sections are populated

## When to Update

| Trigger | Action |
|---------|--------|
| Before asking "Should I continue?" | Full context dump |
| After completing a step | Update Completed Steps, Current Step |
| Made a significant decision | Update Decisions Made |
| Discovered a gotcha | Update Key Learnings |
| Added new file to scope | Update Active Files |

## Context File vs Plan File

| Aspect | Context File | Plan File |
|--------|--------------|-----------|
| Purpose | Current session state | Long-term task definition |
| Scope | Recent work, decisions | All steps, all details |
| Updates | Every pause | After each step |
| Audience | Resuming agent | Any agent |
| Lifetime | Overwritten each session | Persists across sessions |

## What NOT to Include

- Full file contents (reference paths instead)
- Detailed diff output (summarize changes)
- Temporary analysis (only preserve decisions/findings)
- Obsolete information (clean up old context)

## Example Usage

### Before Asking User to Continue

After completing Step 2 and starting Step 3:

```markdown
# Session Context

> Last Updated: 2024-01-15 14:30
> Session: Add user authentication

## Current Task

Implementing JWT-based user authentication.

## Completed Steps

| Step | Summary | Files Changed |
|------|---------|---------------|
| Step 1: Create user model | Added User schema with email/password | src/models/User.ts |
| Step 2: Add password hashing | Integrated bcrypt for secure passwords | src/utils/password.ts |

## Current Step

**Step 3: Implement login endpoint**
**Status:** IN_PROGRESS

### What Was Done
- Created login route skeleton in src/routes/auth.ts
- Started JWT token generation logic

### Decisions Made
- Using JWT with 24h expiry: Balances security and usability
- Refresh tokens stored in httpOnly cookies: Prevents XSS

### Active Files
| Path | Status | Purpose |
|------|--------|---------|
| src/routes/auth.ts | modifying | Login endpoint implementation |
| src/utils/jwt.ts | created | JWT generation/validation utilities |

## Key Learnings

### Gotchas
- User model needs lean() for JSON serialization: Otherwise Mongoose adds _id issues

### Patterns
- Auth middleware pattern: checkToken → validateUser → next

## Open Questions

- [ ] Should we add rate limiting to login endpoint?

## Next Actions

1. Complete JWT token generation in src/utils/jwt.ts
2. Wire up login route to use JWT utils
3. Add auth middleware for protected routes

## Repo Map Summary

| Path | Purpose |
|------|---------|
| src/models/User.ts | User schema definition |
| src/utils/password.ts | Password hashing utilities |
| src/routes/auth.ts | Authentication routes |
| src/utils/jwt.ts | JWT handling |
```

## Integration with Other References

| Reference | When Used | Context Interaction |
|-----------|-----------|---------------------|
| resume-workflow | Start of session | Reads context if available |
| persist-plan | After each step | Context mentions plan existence |
| maintain-repo-map | During execution | Context has quick summary |
| reflect-after-changes | Every 2-3 steps | Context captures reflect findings |

## Mandatory Elements

Before completing a context dump, verify:

- [ ] Last Updated date/time is current
- [ ] Current Task is stated
- [ ] Completed Steps lists all finished work
- [ ] Current Step clearly shows progress
- [ ] Decisions Made captures all key decisions
- [ ] Next Actions is ordered and actionable
- [ ] Active Files lists files being worked on

## After Dumping Context

When you've dumped context and need to ask the user:

1. Print a brief summary of current progress
2. Ask your question
3. Mention that context has been preserved

Example:
> "I've completed Steps 1-2 and started Step 3 (login endpoint). Context saved to plans/context.md. Should I continue with the JWT implementation?"

## Next Step

→ Continue with: **Ask user for input** or **execute-step** (if proceeding)