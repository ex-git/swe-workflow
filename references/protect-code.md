# Protect Code

## Overview

Prevent accidental deletions and overwrites. Take snapshots before changing anything. This is your safety net.

## When to Use

At the start of execute-step, before making any code changes.

## Instructions

### Step 1: Identify Files to Modify

Before writing any code:

1. Read the step's Plan section carefully
2. List all files you intend to:
   - Create (new files)
   - Modify (edit existing files)
   - Delete (remove files)

3. Use `find` or `ls` to verify files exist before modification:
   ```bash
   # List specific files to check they exist
   ls -la src/path/to/file.ts
   
   # Find files matching a pattern
   find src -name "*.ts" -type f
   ```

### Step 2: Take a Snapshot

Choose ONE method:

**Option A: Git (preferred if available)**
```bash
# Stage current state
git add -A
git stash push -m "snapshot before step N: [step name]"

# Or create a checkpoint commit
git add -A
git commit -m "WIP: before step N - [step name]"
```

**Option B: Copy to snapshots directory**
```bash
# Create snapshot directory
mkdir -p snapshots/step-N

# Copy files you'll modify
cp src/existing/file.ts snapshots/step-N/file.ts
```

**Option C: Read and note current state**
- If no git and no file copy possible:
- Use `read` tool to read the entire file
- Save the content in your context/memory
- Mark in your notes: "File content preserved: [filename]"

### Step 3: Proceed with Implementation

- Make changes using `edit` tool for surgical changes
- Use `write` tool for new files
- Avoid rewriting entire files when editing

### Step 4: Verify Against Snapshot

After implementing:

1. **Compare against snapshot:**
   ```bash
   # Git diff
   git diff
   
   # Compare against backup
   diff -u snapshots/step-N/file.ts src/existing/file.ts
   ```

2. **Check for accidental changes:**
   - [ ] No unrelated code was deleted
   - [ ] No functions outside scope were modified
   - [ ] No imports accidentally removed
   - [ ] No config changes beyond scope
   - [ ] No files deleted unexpectedly

3. **If accidental damage detected:**
   - **STOP IMMEDIATELY**
   - Restore from snapshot
   - Re-implement more carefully
   - Use smaller edits

## Snapshot Restoration

If something went wrong:

```bash
# From git stash
git stash pop

# From git checkpoint commit
git reset --hard HEAD~1

# From backup file
cp snapshots/step-N/file.ts src/existing/file.ts
```

## Which Files to Protect

| Change Type | Protection Needed |
|-------------|-------------------|
| New file | Low risk - just create it |
| Edit existing file | Medium risk - snapshot it |
| Delete file | High risk - verify intent |
| Rename file | Medium risk - verify references |
| Config file | High risk - backup critical |

## Constraints

- **Never modify files outside the plan step's scope**
- **All deletions must be intentional** and traceable to the plan
- **Prefer surgical edits** (`edit` tool) over full file rewrites (`write` tool)
- **When rewriting a file**: verify line-by-line against snapshot first
- **If unsure whether a deletion is safe**: don't delete — ask first
- **Always have a rollback plan** before changing files

## Common Mistakes to Avoid

- Using `write` tool to "edit" a file (loses content not in your new version)
- Deleting functions "because they're not used" (may have subtle callers)
- Changing imports without checking where they're used
- Reformatting whole files "while I'm here"
- Copy-pasting over files instead of editing

## Decision Guide

```
Changing 1-5 lines?  → Use edit tool
Changing 6-20 lines? → Use edit tool (multiple edits if needed)
Changing 21+ lines?  → Consider rewrite, but VERIFY with snapshot first
New file?            → Use write tool
Deleting a file?     → Verify scope, then delete
```

## Next Step

After protecting and implementing:

→ Proceed to: **validate-step** (verify correctness)

If damage detected during snapshot comparison:
→ Restore from snapshot
→ Re-implement with smaller, more careful changes