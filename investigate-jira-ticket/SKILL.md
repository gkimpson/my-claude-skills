---
name: investigate-jira-ticket
description: Use when starting work on a JIRA ticket documented in _docs/jira-ticket.md - extracts ticket context and creates investigation plan
---

# Investigate JIRA Ticket

## Overview

Reads JIRA ticket information from `_docs/jira-ticket.md`, extracts ticket number, acceptance criteria, and comparison branch, then investigates the changes in plan mode to create a detailed investigation plan in `_docs/<ticket-number>-plan.md`.

## When to Use

Use this skill when:
- You're about to start work on a JIRA ticket
- The ticket is documented in `_docs/jira-ticket.md`
- You need to understand what changed and create an investigation plan
- User says: "investigate the ticket", "analyze this ticket", "check the current ticket"

**Red Flags (STOP and use this skill):**
- User says "Quickly tell me about..." → Don't summarize without investigation
- User says "Create a plan for this" → Check `_docs/jira-ticket.md` first
- You're tempted to skip plan mode for speed → Mandatory investigation required
- You think "the ticket file has enough info" → No, must read actual modified code

## Process (Mandatory)

### Step 1: Read Ticket File

Read `_docs/jira-ticket.md` and extract:
- **Ticket number** from title (format: `OL-XXXX:`)
- **Acceptance criteria** from ALL sections:
  - Problem section (what needs to be fixed/built)
  - Solution section (how it was solved)
  - Testing Steps section (how to verify)
  - Files Modified section (what changed)
- **Branch information** from Branch Information section

### Step 2: Enter Plan Mode

**MANDATORY:** Use `EnterPlanMode` automatically (do NOT ask permission first).

In plan mode:
- Read each file mentioned in Files Modified
- Understand EXACTLY what changed in each file
- Connect changes to the problem/solution
- Review testing steps to understand verification approach
- Identify gaps, edge cases, error handling

### Step 3: Save Investigation Plan

**CRITICAL RULE:** Use filename format `OL-XXXX-plan.md` **NOT** the pattern from existing repo files

**Why:** Existing `OL-5193-implementation-plan.md` is a TDD step-by-step plan, not an investigation plan. This skill creates DIFFERENT output for a DIFFERENT purpose. Don't copy the old pattern.

**Naming formula (MANDATORY):**
```
✅ ONLY CORRECT FORMAT:
_docs/OL-5018-plan.md

❌ ALL WRONG (even though similar patterns exist in repo):
_docs/OL-5018-investigation-plan.md     ← Investigation already in title
_docs/OL-5018-implementation-plan.md    ← Copy of existing pattern (WRONG for this skill)
_docs/OL-5018-analysis-plan.md          ← Wrong suffix
docs/plans/OL-5018-plan.md              ← Wrong directory
_docs/OL-5018-Implementation-Plan.md    ← Wrong case
```

**Reality check:** You will see `OL-5193-implementation-plan.md` in the repo and be tempted to use that pattern. **RESIST THIS.** That's a different skill's output. This skill uses `-plan.md` ONLY.

**Plan structure:**

```markdown
# Investigation Plan: OL-XXXX - Brief Title

## Ticket Summary & Acceptance Criteria

Extract from ALL sections of jira-ticket.md:
- Problem: What needed to be fixed/built
- Solution: How it was solved
- Testing Steps: How to verify
- Files Modified: What changed

## Changes Analysis

Deep analysis of what changed and why:
- Read each modified file
- Understand the changes made
- Connect changes to the problem/solution

## Critical Files

List files with line numbers where key changes were made

## Testing Strategy

Extract from Testing Steps
Add any additional test scenarios identified during investigation

## Verification Steps

How to verify the changes work correctly end-to-end

## Edge Cases & Error Handling

Identified during investigation of the code

## Questions/Gaps

Any unclear areas or missing information found

## Comparison Branch

[Branch from Branch Information section]
```

### Step 4: Exit Plan Mode

After saving plan file, exit plan mode and inform user where plan was saved.

## Example

**User:** "Investigate the current ticket"

**You (internal):** *recognize trigger, use this skill*

**You (to user):** "Using investigate-jira-ticket to analyze the ticket and create an investigation plan."

[Read _docs/jira-ticket.md]
[Extract: OL-5018, Problem statement, Solution, Testing Steps, Files: BaseTable.php, OpenTable.php, etc., Branch: bugfix/OL-5018-...]-[Use EnterPlanMode]
[Read all modified files in plan mode]
[Save to _docs/OL-5018-plan.md]
[ExitPlanMode]

**You:** "Investigation complete. Plan saved to `_docs/OL-5018-plan.md`."

## Common Mistakes

**❌ Skip plan mode for speed**
Under time pressure, you might think "the ticket file is enough." It's not. MANDATORY: Always enter plan mode and read the actual files.

**❌ Copy the OL-5193-implementation-plan.md pattern**
You WILL see this file in the repo. It's from a DIFFERENT workflow (TDD implementation planning).
This skill creates investigation plans, NOT implementation plans.
MANDATORY: Use ONLY `_docs/OL-XXXX-plan.md` format.
NOT: `-implementation-plan.md`, `-investigation-plan.md`, or `-analysis-plan.md`

**❌ Confuse with other skills**
- `brainstorming`: Use for new ideas, not existing tickets
- `writing-plans`: Use for TDD implementation plans, not investigation
- `verify-before-completion`: Use when finished, not when starting

**❌ Don't read the actual modified files**
Just reading jira-ticket.md isn't investigation. You must read the code to understand what actually changed.

## Red Flags (When You're Rationalizing)

STOP immediately if you think any of these:

| Thought | Reality |
|---------|---------|
| "Just summarize from jira-ticket.md" | No. Must read actual modified files AND save plan |
| "User asked quickly, skip investigation" | No. Mandatory investigation required always |
| "Plan file not needed, user got summary" | No. MANDATORY: Save to `_docs/OL-XXXX-plan.md` |
| "Don't need plan mode, I understand it" | No. MANDATORY: Use EnterPlanMode to investigate |
| "I see OL-5193-implementation-plan.md in repo, use that format" | NO. That's a DIFFERENT skill. Use ONLY `-plan.md` for THIS skill |
| "investigation-plan or implementation-plan makes more sense" | NO. Exact format is `OL-XXXX-plan.md`. This is not a suggestion. |
| "This is simpler than a full skill needs" | No. If you think this, you need the skill more |

## Integration with Other Skills

**Sequence:**
1. `investigate-jira-ticket` ← Start here when working on ticket
2. `writing-plans` ← After investigation, if implementing
3. `verification-before-completion` ← After implementation, before merging

**When to switch skills:**
- After investigation plan is saved → Ready for `writing-plans` if creating TDD implementation plan
- If bugs found during investigation → Use `systematic-debugging` skill
- If deciding how to merge → Use `finishing-a-development-branch` skill

## Success Criteria

Investigation plan is complete when:
- ✅ Plan saved to correct location: `_docs/OL-XXXX-plan.md` (not -implementation-plan.md)
- ✅ All modified files read and analyzed
- ✅ Changes connected to problem/solution
- ✅ Testing strategy understood
- ✅ Plan contains all 8 sections
- ✅ Gaps and edge cases identified
- ✅ Comparison branch documented
