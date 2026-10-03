# holiday-research-agent Skill

## Status
✅ Completed (RED-GREEN-REFACTOR cycle passed)

## What This Skill Does

Structured methodology for comparing package holidays across multiple UK operators. The core principle: **never trust brochure descriptions alone**—always cross-verify against guest reviews and operator FAQs using explicit confidence scoring.

## When to Use

- Comparing holidays across TUI, Jet2, EasyJet, First Choice, Love Holidays, On The Beach, Kuoni
- Validating all-inclusive claims (especially drinks inclusion, quality, varieties)  
- Family-oriented resort research with specific requirements (waterpark, kids club, entertainment)
- Budget-constrained decisions requiring careful evaluation

## Key Features

- **Verification Cycle Framework:** Brochure → Operator FAQ → Reviews → Cross-check → Confidence Score → Rank
- **Drinks Validation Table:** Specific search terms to verify cocktails, premium spirits, snacks, ice cream
- **Confidence Scoring Rules:** Clear thresholds for HIGH/MEDIUM/LOW confidence (not subjective)
- **Rationalization Blockers:** Catches common shortcuts (silence ≠ confirmation, brochure language deception)
- **Family Requirements Checklist:** Mandatory (waterpark, kids club, entertainment) vs. bonus features

## Testing Results

**RED Phase (Baseline):** Agents initially refuse holiday research without clear methodology.

**GREEN Phase (Skill Applied):** Agents successfully:
- Search reviews for specific drink terms
- Cross-check brochure claims vs. guest feedback
- Reject hotels that fail mandatory requirements (cocktails as chargeable extra)
- Apply confidence scoring to separate verified facts from inferences
- Provide structured comparisons ranked by family-value

**REFACTOR Phase (Stress Test):** Skill correctly identifies:
- Conflicting reviews requiring clarification
- Silent reviews (absence of mention ≠ confirmation)
- Outdated or unverified claims
- Operator marketing language deception

## Files

- `SKILL.md` - Main reference guide with full methodology, examples, and output format

## Example Scenario

**Problem:** Brochure says "all-inclusive with premium spirits" but you found 1 recent review saying cocktails cost extra.

**Skill Application:**
1. Mark as CONFLICTING INFORMATION
2. Apply rule: "Cocktails mentioned as chargeable extra in ANY recent review = NOT included"
3. Confidence: LOW on premium spirits claim
4. Result: REJECT this hotel (fails mandatory requirement)

**Without skill:** Might assume brochure is correct and book it anyway (costly mistake).
