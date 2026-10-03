# Self-Healing Feature Pipeline - Quick Start

## How to Use

When dispatching a subagent to implement a feature, use this dispatch template:

```markdown
You are implementing [FEATURE NAME]. Here's what it should do:

[FEATURE SPEC - 2-3 sentences describing the feature]

SELF-HEALING TEST FLOW (non-negotiable):
1. Write implementation code
2. Run: php artisan test tests/[path]/[FeatureTest]
3. If any tests fail:
   a. Print the exact failure message
   b. Read the test file to understand what it expects
   c. Read your implementation to find the mismatch
   d. Fix the implementation (do NOT modify tests)
   e. Go back to step 2
4. When feature tests pass, run full suite: php artisan test
5. Report completion ONLY when ALL tests pass

Report format:
"Task complete. [N] tests passing. Files changed: [list]. No regressions."
```

## Example in Practice

**Before (without skill):**
```
User: Implement WTN signature feature
Subagent: Task complete! (tests fail later)
User: Fix the signature validation test
Subagent: Fixed! (still fails, now middleware broken)
User: Check middleware... (3 rounds of debugging)
```

**After (with skill):**
```
User: Implement WTN signature feature [+ self-healing instructions above]
Subagent: Implemented → Test failed: signature length mismatch → Fixed code → Tests pass → Full suite green → Report: "Task complete. 234 tests passing"
User: Code is ready, no debugging needed
```

## Integration with Your Subagent Workflow

This skill enhances your existing pattern:

1. **Create tasks** (use TaskCreate)
2. **Dispatch subagents** (use Agent) **with self-healing instructions**
3. **Receive clean code** (tests already passing)
4. **Commit** (no debugging phase)

## Files in This Skill

- `SKILL.md` — Full reference guide and patterns
- `README.md` — This quick start guide

## Testing This Skill

Try it on your next feature. When subagents self-correct and report passing tests, the skill is working.
