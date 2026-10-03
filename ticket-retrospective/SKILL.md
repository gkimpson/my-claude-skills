---
name: ticket-retrospective
description: Generate a retrospective analysis after completing a ticket - what went well, what could improve, key success factors
---

# Ticket Retrospective

## Overview

After completing a ticket, analyze what made the work efficient (or inefficient) to improve future ticket documentation and workflows.

## When to Use

Use this skill when:
- You've just completed a JIRA ticket
- User asks "what went well?" or "what helped you understand this?"
- Before creating PR/commit to capture learnings
- User says: "retrospective", "what worked", "analyze the ticket"

## Process

### Step 1: Read Ticket Context

Read `_docs/jira-ticket.md` to understand:
- How the ticket was structured
- What information was provided
- What was missing or unclear

### Step 2: Review Your Work

Reflect on:
- How long did investigation take?
- What questions did you have to ask?
- What information saved time?
- What caused confusion or delays?
- What assumptions did you make?

### Step 3: Generate Retrospective

Create a structured analysis with these sections:

#### What Went Well (Star Rating ⭐⭐⭐⭐⭐)

For each helpful aspect:
- **What helped:** Specific ticket element (e.g., "Important Files section listed exact paths")
- **Impact:** Time saved or clarity gained (e.g., "Reduced exploration from 30min to 5min")

Common categories:
- Ticket documentation quality
- Existing code structure
- Test coverage
- Context provided (meeting notes, screenshots, links)
- User guidance ("work partially complete")
- Domain knowledge (CLAUDE.md, memory files)

#### What Could Have Been Better

For each friction point:
- What was unclear or missing
- What questions you had to ask
- What assumptions you made
- What caused delays

#### Key Success Factors (Ranked)

List 5-7 factors in priority order showing what made the biggest difference.

#### Lessons for Future Tickets

**What made this efficient:**
- ✅ Concrete patterns to replicate
- ✅ Ticket structure elements that helped
- ✅ Information that should always be included

**What would make it even better:**
- Suggestions for improvement
- Missing information that would have helped
- Process improvements

#### Overall Assessment

Rate these dimensions:
- **Speed:** How quickly could you complete the work?
- **Confidence:** How certain were you about correctness?
- **Clarity:** How well did you understand requirements?

Provide a summary sentence: "This ticket is a [good/excellent/poor] template because..."

### Step 4: Suggest Memory Updates

If this ticket revealed:
- New patterns worth remembering
- Project-specific conventions
- Effective communication techniques
- Common pitfalls

Ask the user if they want to add these to Claude memory.

## Example Output Template

```markdown
## Ticket Retrospective: OL-XXXX

### What Went Well ⭐⭐⭐⭐⭐

**1. [Aspect Name]** ⭐⭐⭐⭐⭐
- **What helped:** [Specific detail]
- **Impact:** [Concrete outcome]

**2. [Another Aspect]** ⭐⭐⭐⭐
- **What helped:** [Specific detail]
- **Impact:** [Concrete outcome]

### What Could Have Been Better

1. **[Issue]:** [Description and impact]
2. **[Another Issue]:** [Description and impact]

### Key Success Factors (Ranked)

1. [Most important factor]
2. [Second most important]
3. [Third...]

### Lessons for Future Tickets

**What made this efficient:**
- ✅ [Pattern to replicate]
- ✅ [Another pattern]

**What would make it even better:**
- [Suggestion]
- [Another suggestion]

### Overall Assessment

- **Speed:** [Fast/Medium/Slow] - [reason]
- **Confidence:** [High/Medium/Low] - [reason]
- **Clarity:** [Excellent/Good/Poor] - [reason]

**Summary:** This ticket is a [rating] template because [reason].
```

## Red Flags (When NOT to Use)

- Ticket just started (not completed yet)
- No jira-ticket.md exists
- User wants technical code review (use requesting-code-review instead)

## Integration with Other Skills

**Sequence:**
1. `investigate-jira-ticket` → Start work
2. Implementation skills → Complete work
3. `verification-before-completion` → Verify correctness
4. `ticket-retrospective` → THIS SKILL - Analyze what worked
5. `finishing-a-development-branch` → Decide on merge/PR

## Common Mistakes

❌ **Being too generic**: "The ticket was clear" → Not helpful
✅ **Being specific**: "Important Files section listed exact paths, saved 25 minutes of grepping"

❌ **Not quantifying impact**: "Tests helped" → Vague
✅ **Quantifying impact**: "6 tests documented all model types, eliminated need to read helper code"

❌ **Skipping the "better" section**: Always include constructive feedback
✅ **Honest assessment**: "Screenshot annotations mentioned but not provided"

## Output Format

Present retrospective in markdown with clear headings and emoji for visual scanning. Make it actionable - future tickets should be able to learn from this analysis.

## Success Criteria

Retrospective is complete when:
- ✅ Specific examples given (not generic praise)
- ✅ Impact quantified where possible
- ✅ Both positives and improvements listed
- ✅ Actionable lessons extracted
- ✅ Memory update suggestions offered
