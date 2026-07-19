# Scoring Engine

How ScoreApp turns answers into scores, tiers, and bands — and the patterns that make routing work.

## Table of contents
1. How ScoreApp scoring works
2. Categories — running parallel scores
3. Score Tiers — defining bands
4. The viability-gates-band pattern
5. Shadow questions (and when they're worth it)
6. Tuning and testing

---

## 1. How ScoreApp scoring works

After a user finishes, ScoreApp: sums points earned across relevant questions/categories, divides by total possible, converts to a percentage (0–100), and determines which tier that percentage falls into. Points are assigned per answer. **There is no conditional formula logic** — you cannot natively say "if they answered X, add to category Y." You can only assign fixed points to answers and sum them.

---

## 2. Categories — running parallel scores

Group questions into Categories to get multiple sub-scores at once. This is the key to sophisticated routing without formula logic. Example: a "Viability" category (from the gating questions) and a "Best-Practice" category (from the diagnostic) produce two independent percentages. Tiers can apply to each category, not just the overall score — so you can show different content based on a *specific* category's tier.

Category scores are surfaced via UUID-keyed merge tags, e.g. `{score.<CATEGORY_UUID>.percent}` (also `.tier`, `.relative_percent`, `.out_of_10`). The UUID is scorecard-specific — pull it from the merge-field inserter, never hardcode it. The overall score is flat: `{overall_score}`. See custom-code-and-merge-tags.md §2.

---

## 3. Score Tiers — defining bands

- Default is three tiers (Low/Med/High), weighted toward the lower end on purpose — lower/medium outcomes motivate action, so the default ranges aren't even thirds.
- Rename, recolour, and re-range freely. Add tiers with **Add Tier** (a new tier appears mid-range labelled "new" — you'll need to rename and re-range several to make it coherent).
- 2–5 tiers is the practical range. More than that and users clump confusingly.
- Tiers can express *sub-bands*: e.g. three "Mid" tiers inside one named band, each varying content by gap-depth. This is how you get within-band granularity without Audiences.

---

## 4. The viability-gates-band pattern

When a high score is NOT the best lead (see the inversion test in quiz-architecture.md), don't let the diagnostic score alone set the band. Instead:

- **Logic Jumps** on the Part-1 viability answers route non-viable people straight to the "not ready" result page, *before* the diagnostic score matters for their band.
- For everyone viable, the **Best-Practice category tier** refines them across the target band's sub-tiers and into the over-qualified top band.
- The honest limitation: ScoreApp can't combine two answers into one rule, so compound disqualifiers ("couple under threshold AND has kids") can't be auto-detected. Hard-jump only the clear off-ramps; let borderline cases continue and be placed by their diagnostic tier. This also happens to honour a "route, don't disqualify" philosophy.

---

## 5. Shadow questions (and when they're worth it)

If you genuinely need an *answer* to affect the *score* differently depending on routing — the only native way is **shadow questions**: duplicate a question, score the copies differently, and use Logic Jumps to send different users to different copies.

This is real but heavy: every shadow question doubles a build item and adds a jump to maintain. **Default to NOT using them.** Reserve for the rare case where the scoring genuinely must diverge by route. For "vary the CTA emphasis by intent," prefer Audiences (Pro) or simply keeping intent unscored and reading it for human follow-up — far cheaper for a soft benefit.

---

## 6. Tuning and testing

- Set boundaries as a starting hypothesis, then **test in Draft Mode** (Build → Settings → General → Draft Mode) with full run-throughs at lowest, mixed, and highest answers.
- If everyone lands in one tier, rebalance points so typical answers yield mid-range.
- With 5 tiers, clumping is more likely than with 3 — test the spread harder and nudge boundaries until real ICP profiles land where the business expects.
- Draft Mode doesn't consume your response allocation — use it freely.
