# Dynamic Delivery

Choosing the right mechanism for each piece of conditional content, and turning that into a build map.

## Table of contents
1. The four mechanisms in depth
2. The decision tree
3. The per-section build map (the method)
4. Worked example
5. Combining mechanisms

---

## 1. The four mechanisms in depth

**Score Tiers + Dynamic Content.** On a text/media/CTA/category section, toggle *Enable Dynamic Content*. The section splits into one editor per tier (Low/Med/High/etc.). ScoreApp shows the matching tier's version based on the user's score. Native, reliable, no code. This is the workhorse for "content changes by score."

**Categories.** Lets tiers key off a *specific category's* percentage rather than the overall. Use when "which area they're weakest in" should drive content. Also powers `{highest_category_name}` / `{lowest_category_name}` style personalization.

**Audiences (Pro plan).** Show/hide an entire section based on **specific answers** to specific questions — and you can combine with tiers ("Medium tier AND answered No to readiness"). This is the only native answer-reactive visibility. Verify it's available on the account's plan before designing around it.

**Logic Jumps.** Branch the question flow itself: skip questions, or jump to a particular result page, based on an answer. Use for routing the *path*, not styling the result.

---

## 2. The decision tree

For each piece of conditional content, ask in order:

1. Does it change based on the **question path** the user should take? → **Logic Jump**.
2. Does it change based on **score** (overall or category)? → **Dynamic Content tier**.
3. Does it change based on a **specific answer**? → **Audience** (Pro). If no Pro plan, consider a merge-tag-driven JS show/hide (verify it works — see custom-code-and-merge-tags.md) or a shadow-question/Logic-Jump workaround.
4. Does it just need the person's **name/score/category number**? → **merge tag** inside an existing section.
5. Is it a **bespoke layout or interactive tool**? → **Custom Code block** (then apply 2/3 above to make it conditional).

---

## 3. The per-section build map (the method)

This is the single most useful build artifact. For each result page, list every section top-to-bottom and tag each with its mechanism:

```
PAGE: [band name]
- Hero ............... static + merge tag ({first_name}, {overall_score})
- Band explainer ..... tier-driven (Dynamic Content)
- Subtype block A .... Audience (answer = X)
- Subtype block B .... Audience (answer = Y)
- Shared plan ........ static
- Interactive tool ... custom code block (+ Audience if conditional)
- CTA ................ tier/Audience-driven for emphasis
```

ScoreApp hides the non-matching Audience/tier sections automatically. The map IS the build instruction — hand it to whoever assembles the page.

---

## 4. Worked example

A funnel with three bands where the middle band is the target and has gap-depth sub-tiers plus single/dual-income subtypes:

- Band routing (which result page): **Logic Jumps** on viability answers + overall/category **tiers**.
- "Most ground to make up" vs "nearly there" within the middle band: **Dynamic Content** on the Best-Practice tier (3 mid-tiers).
- Single-income vs dual-income maths block: **Audience** on the household-structure answer.
- "Name, here's your score of X%": **merge tags**.
- The embedded calculator: **custom code block**, shown to all in that band.

---

## 5. Combining mechanisms

The power comes from layering: tiers decide the band and gap-depth, Audiences overlay answer-specific subtype content, merge tags personalize the visible result, and Logic Jumps shaped the path that got them here. Keep each mechanism doing the one job it's best at — don't force JS to do an Audience's job, and don't force tiers to express an answer-level distinction they can't see.
