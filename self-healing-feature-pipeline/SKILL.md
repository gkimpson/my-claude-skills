---
name: self-healing-feature-pipeline
description: Use when implementing features end-to-end via subagents and test failures should never block you — agent must run tests after each change, detect regressions, diagnose, and fix without human intervention
---

# Self-Healing Feature Pipeline

## Overview

A disciplined workflow for subagent-driven feature implementation where **tests are not optional checkpoints but built-in circuit breakers**. If any test fails, the agent stops, diagnoses the failure, fixes the root cause, and re-runs the full suite before reporting completion.

**Core principle:** You should never see a test failure from a subagent. They fix their own regressions before reporting "task complete."

## When to Use

**Symptoms that signal you need this:**
- You're dispatching subagents to build features and discovering test failures after they report "done"
- Debugging cycles where agent says "task complete" but tests still fail
- Multiple rounds of "fix this failure" back-and-forth with agents
- Backlog of incomplete features waiting for manual test review

**When NOT to use:**
- Writing tests for the first time (pair with test-driven-development skill instead)
- Spike/exploration phases where tests are intentionally loose
- Single-file changes where tests are obvious

## Core Pattern

### Phase 1: Dispatch Subagent with Test-Aware Instructions

Instead of:
```
"Implement the ProfileCard component with these specs..."
```

Use:
```
"Implement the ProfileCard component with these specs. CRITICAL: After writing code, immediately run:
  php artisan test tests/Feature/Components/ProfileCard
Do NOT report task complete until:
  - All ProfileCard tests pass
  - The full test suite (php artisan test) still passes
  - You've verified no regressions in unrelated tests

If ANY test fails:
  1. Show the exact failure message
  2. Diagnose the root cause (look at what the test expects vs. what you coded)
  3. Fix the code, not the test
  4. Re-run the full suite
  5. Only then report completion"
```

### Phase 2: Agent Self-Corrects

**What the agent MUST do after implementation:**

```
After writing code:
  ✓ Run narrowly scoped tests (the feature's own test file)
  ✓ Run the full test suite (php artisan test)
  ✓ If green: report "task complete with [N] tests passing"
  ✓ If red: 
    1. Print exact failure output
    2. Read the test file to understand what it expects
    3. Read your implementation to find the mismatch
    4. Fix the implementation (not the test)
    5. Re-run suite from step 1
```

**What the agent MUST NOT do:**
- Report completion with a caveat ("tests might fail")
- Suggest "you can fix this later"
- Modify tests to make them pass
- Skip test running ("I'm confident it works")
- Run tests once and assume green

### Phase 3: You Receive Clean Code

When subagent reports "task complete," it includes:
- Final test count (e.g., "227 tests passing")
- No outstanding failures
- Ready to commit

---

## Quick Reference

| Situation | Action |
|-----------|--------|
| All tests pass after implementation | Report completion with count |
| Any test fails | Diagnose + fix + re-run (no human needed) |
| Unknown test failure | Read test to understand expectation |
| Tempted to "skip for now" | Illegal—must fix before reporting complete |
| Multiple failures | Fix all simultaneously, run suite once |

---

## Implementation

### Subagent Dispatch Template

```markdown
You are implementing [FEATURE]. Here are specs: [DETAILS]

CRITICAL TEST FLOW:
1. Write implementation code
2. Run: php artisan test tests/[path]/[FeatureTest]
3. If failures, diagnose and fix (loop until green)
4. Run full suite: php artisan test
5. Report completion ONLY when:
   - Feature tests pass
   - Full suite shows no regressions
   - You've verified no tests were modified

DO NOT:
- Report "done" with failing tests
- Suggest "user can fix this after"
- Modify test files
- Assume tests pass without running them

If stuck on a test failure:
  1. Print the failure message (show me exactly what failed)
  2. Read the test file to see what it expects
  3. Read your implementation
  4. Identify the mismatch
  5. Fix the code
  6. Re-run the suite

Report format when complete:
"Task complete. [N] tests passing. Files modified: [list]. No regressions detected."
```

### Integration with Your Workflow

This pairs with subagent orchestration:

```
1. Create tasks for each feature component
2. Dispatch subagents with self-healing instructions (above template)
3. Each subagent: implement → test → fix → report
4. You receive clean, tested code from each subagent
5. Optional: run full suite one final time to verify integration
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| "Tests after implementation" | Tests DURING—before reporting done |
| "I'll test locally later" | Tests are the circuit breaker, not optional |
| "One failure is OK, user can fix" | All failures must be fixed by agent |
| "Tests are flaky, skip this run" | Flaky tests need fixes too, not skips |
| "Modify test to match code" | Tests are truth, code adjusts to tests |
| "Report 'mostly done'" | "Done" means all tests pass, nothing else |

---

## Red Flags—STOP Before Reporting

If you're about to say "task complete" but ANY of these are true, **keep going**:

- [ ] Didn't run the test suite
- [ ] Ran tests but skipped failures ("we can fix later")
- [ ] Report says "tests might fail" or "assuming it works"
- [ ] Any test modified to pass (without implementation change)
- [ ] Full suite not run (only feature tests)
- [ ] Failures exist but "seem unrelated"

**If ANY box is checked:** Go back to step 1 of the self-correction loop.

---

## Why This Matters

**Without this pattern:**
- Subagents report done → tests fail → you debug → repeat
- Multiple rounds of "fix this failure"
- Feature not truly complete until human tests it
- Defects compound across features

**With this pattern:**
- Subagents catch their own bugs
- You receive fully tested code
- Integration is cleaner (fewer cross-feature issues)
- Parallel subagents don't block each other on test failures

---

## Real-World Impact

From your insights report: "7 buggy-code friction events requiring multiple fix rounds." This skill eliminates those by making the agent responsible for running tests, detecting failures, and fixing them before reporting completion. Your session where you hit test failures would have been non-blocking—each subagent would self-correct and report only when green.

Expected improvement: **From 2-3 rounds of debugging per feature → 0, tests passing on first subagent report.**
