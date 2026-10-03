---
name: holiday-research
description: Search and compare family package holidays across UK providers with specific requirements including waterparks, all-inclusive packages, and UK-focused resorts. Use when user wants to research, compare, or find family holidays with detailed criteria.
---

# Holiday Research

Search and compare package holidays across multiple UK travel providers, filtering by detailed family requirements including waterparks, all-inclusive drink packages, and UK-friendly resorts.

## When to Use This Skill

Use this skill when the user:
- Wants to research or compare family package holidays
- Has specific requirements (waterpark, all-inclusive, star rating, etc.)
- Needs holiday options from Birmingham (BHX) or other UK airports
- Mentions "holiday research", "package holiday comparison", or "find me a holiday"
- Wants to evaluate hotels based on reviews, not just brochure descriptions

## Step 0: Confirm Search Parameters (do this before any searching)

Do NOT assume defaults. Confirm these inputs with the user first — they change the entire search and are cheap to get wrong. Use AskUserQuestion to confirm in one batch, then proceed.

| Parameter | Default (confirm, don't assume) | Notes |
|-----------|--------------------------------|-------|
| Travel dates / month | — | **Sanity-check against today's date.** If the requested month is within ~8 weeks it is a *last-minute* booking: availability, not price, becomes the binding constraint — flag this. |
| Departure airport(s) | BHX | Verify the chosen operators actually fly the route from this airport before deep-diving. |
| Party + child ages | 2 adults + 2 children | Ages drive kids-club bands and whether you need big slides vs. toddler splash zones. |
| Nights | 7 | 7–10 typical. |
| Budget ceiling (total package) | £3,500 | See Pricing Reality — a target for ranking, not a hard live filter. |

## Pricing Reality (read before promising any price)

Live package prices live in operators' JavaScript booking engines (easyJet/TUI/Jet2) that re-price per date, party and airport. **An agent with web search cannot reliably pull an exact live quote.** Therefore:

- **Never fabricate a precise price** (e.g. "£3,287") and present it as verified. This is the most damaging failure mode for this skill.
- Present prices as **indicative ranges** for the season, and assign **price confidence = Low** unless a quote is genuinely retrieved.
- Frame the deliverable as a **shortlist to verify live**, not a booking. End with a clear "confirm the live price/availability for your dates" next step.
- The budget figure drives **ranking and escalation logic**, not a hard cutoff against numbers you cannot verify.

## Research Strategy

### Provider Prioritization

**Primary providers (research first)**:
1. EasyJet Holidays
2. TUI (including First Choice family resorts)
3. Jet2 Holidays

**Secondary providers (if needed)**:
4. Love Holidays
5. On The Beach

### Geographic Constraints

**Target destinations only**:
- Turkey
- Egypt
- Greece
- Canary Islands
- Tunisia

**Mandatory**: Must include beach access

### Search Workflow

For each provider (in priority order), using the **confirmed Step 0 parameters**:

1. **Search query**: "[airport] to [destination] [month/year] family all inclusive waterpark"
2. **Apply filters**: 4-5 star, All Inclusive, confirmed nights, confirmed party
3. **Budget**: rank against the confirmed ceiling; do NOT hard-filter on prices you cannot verify (see Pricing Reality)
4. **Check the route exists**: confirm the operator flies this destination from the chosen airport
5. **Quick rejection**: discard if any mandatory requirement fails
6. **Deep dive**: for remaining candidates, extract details and cross-check reviews
7. **Verify waterpark season**: confirm the waterpark (and any free-access sister park) is open on the travel dates — many have season windows (e.g. May–Oct)
8. **Document**: record all data points for scoring, noting verified vs. inferred

## Mandatory Requirements Checklist

Reject any hotel that fails these:

### Accommodation
- Minimum 4 star (prefer 5 star)
- All Inclusive package only
- Family-oriented resort
- Transfer time under 1.5 hours
- Flight time under 5.5 hours each way

### Location
- Turkey, Egypt, Greece, or Canary Islands
- Beach access available

### Waterpark
- Main pool
- Kids pool
- Minimum 3 slides or splash features
- Lazy river = major bonus

### Food & Drink
- All Inclusive includes:
  - Alcoholic drinks (beyond basic beer and house wine)
  - Cocktails included in standard package
  - Branded/premium spirits where available
  - Snacks between meals
  - Ice cream for children

**Reject if**:
- Cocktails are chargeable extras
- Premium drinks require separate upgrade
- Reviews repeatedly mention weak/poor drinks selection
- All Inclusive is effectively limited to beer, wine, local spirits only

### Children's Facilities
- Kids club suitable for ages 7-8
- Evening children's entertainment
- Daytime family activities

### Entertainment
- Family entertainment
- Evening entertainment
- Preferred: Karaoke, live shows, quizzes, interactive activities

### Guest Profile
Prioritize resorts with:
- Strong English-speaking support
- Large UK visitor base
Evidence from:
- Number of UK reviews
- Review language trends
- TUI/First Choice family-focused branding
- Guest comments about British families
- English-speaking staff feedback

## Scoring System (0-100)

Score each candidate and rank by total, not by price.

### Requirements Met: 40 points
- All mandatory criteria (15 points)
- Waterpark minimum 3+ slides (10 points)
- Beach access confirmed (5 points)
- Transfer + flight time limits (5 points)
- 4-5 star + AI (5 points)

### Drink Package Quality: 20 points
- Cocktails included in standard AI (5 points)
- Premium/branded spirits included (5 points)
- No major drink complaints in reviews (10 points)

### Waterpark Quality: 15 points
- 3+ slides baseline (5 points)
- Lazy river present (5 points)
- Additional features/variety (5 points)

### Entertainment Quality: 10 points
- Kids club + evening kids entertainment (5 points)
- Adult/family evening shows (5 points)

### UK/English Suitability: 10 points
- High volume of UK reviews (5 points)
- Strong evidence of English-speaking support (5 points)

### Value for Money: 5 points
- Price vs. quality comparison

## Data Collection Framework

For each candidate hotel, extract:

### From Operator Listing
- Hotel name, country, region
- Star rating
- Total package price
- Provider name
- Flight duration and times
- Transfer duration
- All-inclusive tier (Standard/Premium)
- Beach distance
- Waterpark description
- Entertainment program overview
- Drinks package details (if specified)

### From Guest Reviews (sample recent reviews; prioritise the last ~18 months)
Web search typically returns aggregated review *summaries*, not 10-20 individual reviews — sample what you can and weight by recency. A 3-year-old comment on drinks or food is likely stale; flag any heavily-relied-on review that is old.
- Drinks package quality and limitations
- Waterpark actual experience (photos, descriptions)
- Food quality and variety
- Entertainment quality and timing
- Sunbed availability issues
- Overcrowding or queue problems
- Hidden costs or upsells
- UK visitor presence and English language support
- Any recurring complaints

### Review Validation
Do NOT rely solely on brochure descriptions. Cross-check:
- Drinks inclusion details against multiple reviews
- Waterpark features from photos and guest descriptions
- UK visitor base from review analysis
- Common complaints across multiple reviewers

## Output Format

Return the options that genuinely qualify, ranked by score — up to 10. Do NOT pad to 10 with weak candidates that barely clear the mandatory bar; a strong 6 beats a padded 10.

For each hotel:

```markdown
## [Hotel Name]

**Country/Location**: [Country], [Region]
**Distance to beach**: [X minutes walk / onsite]
**Indicative Package Price**: £X,XXX–£X,XXX (season estimate — verify live; price confidence: Low unless quoted)
**Provider**: [Company], [Package reference]
**Star Rating**: ⭐⭐⭐⭐⭐ (X stars)
**Flight Duration**: X hours X minutes
**Transfer Duration**: X hours
**All-Inclusive Tier**: [Standard/Premium]
**Dates**: [Travel dates]

### Waterpark Features
- [List all features: pools, slides count, lazy river, splash areas]
- [Any unique features or highlights]

### Entertainment
- **Daytime**: [Kids club details, family activities]
- **Evening**: [Kids entertainment, adult shows, live music, etc.]
- **Timing**: [Hours if available]

### Drink Package
- **Included**: [Alcoholic drinks, cocktails, known brands]
- **Excluded**: [Anything not included]
- **Upgrade options**: [If applicable]

### Beach Access
- [Distance to beach, beach quality if mentioned]

### UK Suitability Score: X/10
- [Evidence: UK review count, British family comments, English staff feedback]

### Confidence: High/Medium/Low
- [Explanation of what's verified vs. assumed]

### Score: [0-100]

### Pros
- [3-5 bullet points]

### Cons
- [3-5 bullet points, validated from multiple reviews]

### Why it made the shortlist
- [1-2 sentences explaining key strengths]

---
```

## Confidence Scoring

### High (70-100 score)
All major requirements verified from multiple sources:
- Mandatory requirements confirmed from operator details
- Drink package details verified from operator + guest reviews
- Waterpark features verified from photos + reviews
- Strong UK visitor base evidenced by high UK review volume
- No major recurring complaints about core features

### Medium (50-69 score)
Most requirements confirmed with minor assumptions:
- Mandatory requirements met
- Some details inferred (e.g., exact drink brands not specified)
- UK visitor base present but review volume moderate
- Some review concerns but not deal-breakers

### Low (0-49 score)
Important elements uncertain:
- Key features inferred rather than verified
- Drink package quality unclear from available info
- Limited review data
- Significant recurring complaints about core requirements

## Budget Handling

### Budget: confirmed ceiling (default £3,500)

The ceiling covers flights, accommodation, transfers and baggage. **Because live prices can't be reliably pulled (see Pricing Reality), use the ceiling to guide ranking and the escalation note below — not as a hard filter against unverified numbers.** When a candidate's indicative range straddles the ceiling, say so and let the user confirm live.

### Escalation Protocol

If fewer than **5 strong matches** sit comfortably within the ceiling:

1. Expand search to **£4,000 maximum**
2. For matches above £3,500, explicitly explain:
   - What extra value the additional spend provides
   - Why it justifies the cost (e.g., "£400 premium for lazy river + premium branded drinks")
   - How it compares to cheaper options on quality

### Value Over Price

Rank by overall family value, not cheapest price. Examples:
- A £3,800 option with excellent drinks, lazy river, and strong UK base beats a £2,900 option with weak drinks and limited entertainment
- A £3,200 option with verified premium AI and no major complaints beats a £2,400 option with recurring food/drink issues

## Common Complaints to Flag

Cross-check reviews for recurring issues:

- Overcrowding at pools or restaurants
- Poor food quality or limited variety
- Long queues for drinks, food, or activities
- Difficult sunbed availability (early morning towel wars)
- Weak or low-quality entertainment
- Hidden costs or unexpected charges
- Poor drinks package quality
- Language barriers with staff
- Safety concerns with waterpark facilities

## Acceptance Criteria

The research is complete when:

- [ ] Search parameters confirmed with the user (Step 0) before searching
- [ ] All 3 primary providers searched
- [ ] Qualifying candidates identified and scored (up to 10, not padded)
- [ ] Mandatory requirements verified — or explicitly marked inferred — for each candidate
- [ ] Recent guest reviews cross-checked for each candidate
- [ ] Confidence level (including separate price confidence) assigned to each candidate
- [ ] Recurring complaints identified and documented
- [ ] Prices presented as indicative ranges to verify live, never fabricated as exact quotes
- [ ] Budget escalation explained if applicable
- [ ] Output formatted according to template
- [ ] Results ranked by total score, not price
- [ ] Deliverable ends with a clear "verify live price/availability" next step