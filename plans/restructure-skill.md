# Plan: Restructure Skill for Compliance

> Status: DRAFT
> Created: 2026-04-26
> Last Updated: 2026-04-26

## Goal

Restructure swe-workflow so agents reliably follow the plan template and workflow rules on the first read of SKILL.md, eliminating the 3-hop progressive disclosure chain that causes non-compliance.

## Problem Statement

Agents see only the skill description in the system prompt. When they read SKILL.md (248 lines), it tells them to load `references/create-plan.md`, which tells them to load `assets/plan-template.md`. This 3-hop chain means the plan template — the single most important output format — is never reached. Evidence: 31 of 43 plans in the ari project (72%) scored 0/5 on template compliance.

Mattpocock's skills demonstrate the fix: SKILL.md must be self-contained for the critical path. Templates are inlined. Reference files hold supplementary domain knowledge, not sequential workflow steps.

## Assumptions

- The core workflow logic (behavioral guards, triage, modes, phases) is sound — the problem is structure, not content.
- Agents will reliably read SKILL.md when triggered. The failure point is reading beyond SKILL.md.
- Reference files should still exist for deep-dive content — but the critical path must not depend on them.
- Backward compatibility with existing `plans/` files in user projects is important.

## Design Decisions

### What goes in SKILL.md (critical path — agent must get this on first read)
- Workflow triage block
- Behavioral guards (condensed)
- Full workflow plan format (inlined template with required sections)
- Step format (inlined with required fields)
- Anti-patterns ("WRONG vs RIGHT" comparison, borrowed from mattpocock's TDD skill)
- Status values and core constraints
- Resume protocol (condensed)

### What moves to references (supplementary — agent consults when needed)
- Detailed clarification interview technique → `require-clarification.md`
- Detailed execution procedure (code protection, discovery handling) → `execute-step.md`
- Verification passes (validate, test, review diff) → `verify-step.md`
- Repo map maintenance → `maintain-repo-map.md`
- Context dumping → `dump-context.md`
- Reflection procedures → `reflect-after-changes.md`, `global-reflection.md`
- Resume edge cases → `resume-workflow.md`
- Plan persistence details → `persist-plan.md`

### What gets removed or merged
- `assets/plan-template.md` — inlined into SKILL.md, file deleted
- `assets/context-template.md` — inlined into `dump-context.md`, file deleted
- `assets/repo-map-template.md` — inlined into `maintain-repo-map.md`, file deleted
- `references/create-plan.md` — critical parts inlined into SKILL.md; file becomes a supplementary reference for step-sizing, exploration, and constraints

### Target: SKILL.md ≤ 150 lines
Mattpocock recommends <100 lines. Our skill is more complex (two modes, behavioral guards, plan format, step format), so 150 is a realistic target. Current: 248 lines without the template; target includes the inlined template.

## Success Criteria

- SKILL.md is self-contained: an agent reading only SKILL.md produces a plan matching the required format
- SKILL.md ≤ 150 lines
- No critical-path content requires a second file read
- Reference files are supplementary, not sequential prerequisites
- All existing reference content is preserved (moved, not deleted)
- README, CHANGELOG, AGENTS.md updated
- Existing `plans/` files in user projects remain compatible

## Steps

### Step 1: Rewrite SKILL.md with inlined critical content

**Status:** COMPLETED

**Prerequisites:**
- Current SKILL.md, plan-template.md, and create-plan.md fully read (done in investigation)

**Deliverables:**
- New SKILL.md ≤ 150 lines with inlined plan format and step format

**Plan:**
- [x] Write new SKILL.md from scratch
- [x] Verify ≤ 150 lines
- [x] Verify all plan format requirements are visible without reading another file
- [x] Verify behavioral guards are present (condensed)
- [x] Verify reference links are correct

**Validation Checklist:**
- [x] An agent reading only SKILL.md can produce a compliant plan
- [x] No required content depends on a second file read
- [x] Line count ≤ 150 (exactly 150)

**Test Checklist:**
- [x] Manual: read SKILL.md — plan format is unambiguous, all required sections visible
- [x] Manual: all 10 reference file links resolve correctly

**Implementation Notes:**
- Rewrote SKILL.md from 248 → 150 lines
- Inlined full plan template as a fenced markdown block (Goal, Assumptions, Open Questions, Context & Learnings, Steps with Prerequisites/Deliverables/Plan/Validation/Test/Notes/Files Changed, Implementation Log)
- Inlined step format showing exact required fields
- Added WRONG vs RIGHT anti-pattern comparison (borrowed from mattpocock's TDD skill pattern)
- Condensed behavioral guards from 4 multi-bullet sections → 4 one-line summaries
- Removed Operating Modes table, Full workflow decision test, Skill Chain diagram, "Why Full Workflow Works" section, and Compact Variant section
- Condensed status values into inline format
- Removed duplicate references to templates (plan-template, repo-map-template, context-template)
- All 10 reference links verified working

**Files Changed:**
- SKILL.md (rewritten)

---

### Step 2: Update reference files to be supplementary

**Status:** COMPLETED

**Prerequisites:**
- Step 1 COMPLETED (new SKILL.md exists)

**Deliverables:**
- `references/create-plan.md` updated: removed template-reading instructions, points to SKILL.md for plan format
- All cross-references verified

**Plan:**
- [x] Update `create-plan.md`: remove template-reading instructions, add note that plan format is defined in SKILL.md
- [x] Inline context template into `dump-context.md`, remove reference to assets/context-template.md
- [x] Inline repo-map template into `maintain-repo-map.md`, remove reference to assets/repo-map-template.md
- [x] Verify all cross-references between files are correct
- [x] Verify no reference file assumes the agent hasn't read SKILL.md

**Validation Checklist:**
- [x] No reference file tells the agent to read another file for critical-path content
- [x] All cross-references resolve
- [x] Zero references to `assets/` remain in any reference or SKILL.md

**Test Checklist:**
- [x] grep for `assets/` in references/*.md and SKILL.md — zero results
- [x] grep for `read.*template` in references/*.md — zero critical-path results

**Implementation Notes:**
- `create-plan.md` was the only file referencing asset templates
- Removed step 6 ("Read the plan template"), renumbered 7-12 to 6-11
- Changed step 8 (repo-map init) to point to maintain-repo-map.md instead of asset template
- Changed step 9 (write plan) to reference SKILL.md Plan Format section
- Changed constraint from "proceed without reading the template" to "invent your own plan format"
- `dump-context.md` and `maintain-repo-map.md` already had inline content — no changes needed

**Files Changed:**
- references/create-plan.md (updated)

---

### Step 3: Delete redundant asset files

**Status:** COMPLETED

**Prerequisites:**
- Step 2 COMPLETED (templates inlined into their respective files)

**Deliverables:**
- `assets/plan-template.md` deleted
- `assets/context-template.md` deleted
- `assets/repo-map-template.md` deleted
- `assets/` directory removed

**Plan:**
- [x] Delete `assets/plan-template.md`
- [x] Delete `assets/context-template.md`
- [x] Delete `assets/repo-map-template.md`
- [x] Remove `assets/` directory
- [x] Verify no remaining file references deleted assets

**Validation Checklist:**
- [x] No reference or SKILL.md references `assets/` paths
- [x] `assets/` directory does not exist

**Test Checklist:**
- [x] `grep -rn "assets/" references/*.md SKILL.md` returns nothing
- [x] `ls assets/` fails (directory gone)

**Implementation Notes:**
- Deleted all 3 asset files and removed the directory
- Two stale references remain in README.md and examples/add-email-validation/README.md — will be fixed in Step 4 (docs update)

**Files Changed:**
- assets/plan-template.md (deleted)
- assets/context-template.md (deleted)
- assets/repo-map-template.md (deleted)
- assets/ (directory removed)

---

### Step 4: Update README, CHANGELOG, AGENTS.md, package.json

**Status:** COMPLETED

**Prerequisites:**
- Steps 1-3 COMPLETED

**Deliverables:**
- README.md structure diagram updated (removed assets/)
- CHANGELOG.md has v1.5.0 entry
- package.json version bumped to 1.5.0, assets/ removed from files list
- examples/add-email-validation/README.md updated (removed dead asset link)

**Plan:**
- [x] Update README structure diagram (remove assets/)
- [x] Update README description to reflect self-contained SKILL.md design
- [x] Add CHANGELOG v1.5.0 entry describing the restructure
- [x] Bump package.json version
- [x] Fix examples/add-email-validation/README.md dead link
- [x] Review AGENTS.md for any needed updates

**Validation Checklist:**
- [x] README structure matches actual file tree
- [x] CHANGELOG has complete v1.5.0 entry
- [x] package.json version is 1.5.0 and assets/ removed from files list
- [x] Zero stale assets/ references (except CHANGELOG describing the removal)

**Test Checklist:**
- [x] grep for assets/ across all non-plan .md and .json files returns only CHANGELOG

**Implementation Notes:**
- AGENTS.md needed no changes — it references SKILL.md, not templates
- README structure diagram updated and comment changed from "start here" to "plan format inlined"
- Example README now points to SKILL.md#plan-format--required-structure instead of deleted asset

**Files Changed:**
- README.md (updated structure diagram)
- CHANGELOG.md (added v1.5.0 entry, updated version links)
- package.json (version bump, removed assets/ from files)
- examples/add-email-validation/README.md (fixed dead link)

---

### Step 5: Validate the restructure end-to-end

**Status:** COMPLETED

**Prerequisites:**
- Steps 1-4 COMPLETED

**Deliverables:**
- Full validation that SKILL.md alone produces compliant plan format
- All cross-references verified
- No broken links
- Line count verified

**Plan:**
- [x] Read final SKILL.md and trace: "If I were an agent seeing only this, could I produce the right plan format?"
- [x] Verify all required plan sections present in SKILL.md
- [x] Verify all required step fields present in SKILL.md
- [x] Verify all reference links resolve
- [x] Verify zero stale assets/ references
- [x] Verify SKILL.md ≤ 150 lines
- [x] Count total lines across all files

**Validation Checklist:**
- [x] SKILL.md self-contained for plan format (all 6 plan sections + 8 step fields + implementation log)
- [x] Zero broken links (all 10 reference files verified)
- [x] 150 lines in SKILL.md (exactly at target)
- [x] All reference files still provide useful supplementary content
- [x] Anti-pattern WRONG/RIGHT comparison present
- [x] "Do NOT invent your own format" warning present

**Test Checklist:**
- [x] Agent simulation: reading only SKILL.md, all required output fields identifiable
- [x] `grep -rn "assets/"` returns only CHANGELOG (describing removal)
- [x] All 10 reference file links resolve to existing files

**Implementation Notes:**
- All validation passed
- Total lines: 3,031 → 2,822 (-7%)
- SKILL.md: 248 → 150 lines (-40%)
- Hops to plan template: 3 → 0
- Net diff: -566 lines removed, +211 added across 10 files

**Files Changed:**
- None (validation only)

---

## Implementation Log

| Date | Step | Summary |
|------|------|---------|
| | | |

## Deviations from Original Plan

> None yet.
