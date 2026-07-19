---
name: scoreapp-quiz-builder
description: >-
  Expert workflow for designing and building high-converting quiz funnels in
  ScoreApp — including scoring logic, result-page routing, dynamic/conditional
  content, custom-coded result pages, lead-magnet tools, and follow-up email
  sequences. Use this skill whenever the user mentions ScoreApp, a "scorecard",
  a quiz funnel, an assessment/diagnostic quiz, result pages that change by
  score or answer, lead-scoring or lead-routing logic, or wants to turn quiz
  responses into segmented leads and nurture sequences — even if they don't name
  ScoreApp explicitly. Also use when building or critiquing quiz questions,
  score tiers, categories, Logic Jumps, Audiences, merge tags, or quiz-driven
  result/PDF personalization. ALWAYS web-search ScoreApp's current docs first
  (see Step 0) because the platform's features and plan gating change over time.
---

# ScoreApp Quiz Builder

This skill captures a battle-tested method for building quiz funnels in ScoreApp:
the scoring engine, the routing/visibility logic, the custom-coded result pages,
the takeaway tools, and the email sequences that follow. It exists because
ScoreApp has several non-obvious constraints and capabilities that, if
misunderstood, lead to architectures that cannot be built — and because the
platform changes, so assumptions must be re-verified every time.

## Step 0 — Verify currency FIRST (non-negotiable)

ScoreApp's feature set, plan gating, merge-tag syntax, and UI change over time.
Before giving any build advice, **web-search the current ScoreApp documentation**
and confirm the specifics you are about to rely on. Treat everything in this
skill's reference files as a strong prior to be re-validated, not as gospel.

Run searches such as:
- `ScoreApp [feature] documentation` (e.g. "ScoreApp Audiences documentation")
- `ScoreApp merge tags list site:support.scoreapp.com`
- `ScoreApp [feature] plan tier` (to confirm what's gated to Pro/Business)
- `ScoreApp custom code block results page` (capabilities change)

Fetch `support.scoreapp.com` articles directly when found. If a search result
contradicts this skill, **trust the current docs** and tell the user what
changed. If you cannot verify a capability, say so plainly rather than assuming.

Be especially skeptical of secondhand claims (forum posts, AI-generated how-tos).
The thread that produced this skill encountered an AI assistant that
flip-flopped repeatedly on whether custom-code blocks could be toggled dynamic.
Confirm against first-party docs.

## The four mechanisms (the mental model)

Almost every ScoreApp build decision reduces to choosing the right tool from these four. Internalize this; it prevents the most common architecture mistakes.

1. **Score Tiers** — percentage bands (Low/Med/High or more) on the overall score OR on a category. Drive content that changes **by score**. This is the native "band" system.
2. **Categories** — group questions so you get multiple sub-scores in parallel (e.g. a "Viability" score and a "Best-Practice" score). Tiers can apply per-category.
3. **Audiences** (Pro plan) — show/hide a section based on **specific answers** to specific questions. This is the only native tool that reacts to *what someone answered*, not just their score.
4. **Logic Jumps** — branch the *question flow*: skip questions or jump to a result page based on an answer.

Plus two helpers:
- **Merge tags** (`{single_curly}` — same scheme on-page HTML and in redirect URLs; verified 2026-07-18) — inject the person's name, score, tier, or category scores into content. Text only. Flat tokens (`{first_name}`, `{overall_score}`) are portable; category/question tokens are UUID-keyed per scorecard — see references/custom-code-and-merge-tags.md.
- **Custom Code blocks** (Pro/Business) — paste raw HTML/CSS/JS as a section. Use for bespoke layouts and interactive tools.

### The decision rule (commit this to memory)
- Content changes by **SCORE** → Dynamic Content on a tier.
- Content changes by **ANSWER** → Audience.
- Content just needs their **name/number** → merge tag inside an existing section.
- The **question path** itself must change → Logic Jump.
- You need a **bespoke layout or interactive tool** → Custom Code block.

## Hard constraints that shape every build

These are the traps. Design around them from the start.

- **Result pages react to score tier and Audiences — not to raw answers inside your own code by default.** Per-answer reactivity comes from Audiences (Pro), or from feeding a merge tag into your own JS (verify it resolves in-script first — see references/custom-code-and-merge-tags.md).
- **ScoreApp has no advanced formula logic.** You can assign points and sum them; you cannot write "if answer X then add 20 to category Y." Anything conditional-on-an-answer that must affect the *score* requires either clever point-weighting or "shadow questions" (duplicated, differently-scored copies behind Logic Jumps). Shadow questions are heavy maintenance — use sparingly.
- **A skipped category stays at 0.** If Logic Jumps skip a whole category for some users, plan scoring so that 0 doesn't mis-tier them. Usually: terminate skipped branches on their own result page.
- **Logic Jumps force a fixed question order** (you lose randomised/category ordering once enabled).
- **Custom Code blocks cannot be natively toggled into score-tier tabs** the way text sections can. To make a code block conditional, either wrap it in an Audience, or use JS show/hide driven by a merge tag.
- **localStorage / sessionStorage do not work reliably inside ScoreApp-embedded code.** If a tool must *save* the user's data, build it as a standalone page hosted elsewhere and link to it.

## The build workflow

Follow these in order. Each references a deeper file when needed.

### 1. Scope the funnel and the ICP routing
Establish who the quiz is qualifying, and what the *bands* mean in business terms. Critically: **decide whether a high score is actually the best lead.** In many funnels it is NOT (high scorers already have it handled and don't convert). If so, viability/intent must gate the band — score alone will route backwards. See `references/quiz-architecture.md`.

### 2. Design the questions in three parts
The proven structure: **Part 1 — capture + viability** (the gating signals), **Part 2 — scored diagnostic** (the yes/no or scaled questions that produce the score), **Part 3 — qualifiers/intent** (often unscored, for human follow-up). Keep it lean; every question costs completion rate. See `references/quiz-architecture.md` for question-writing patterns and the capture-trust rules (e.g. why phone numbers belong at the booking step, not Part 1).

### 3. Build the scoring engine
Set Categories, assign points, and define Score Tiers as percentages. Decide what each tier *means* and tune boundaries in **Draft Mode** with test run-throughs (5 tiers clump easily — test harder than 3). See `references/scoring-engine.md`.

### 4. Map result-page delivery
For each result page, list its sections and tag each as: static / tier-driven (Dynamic Content) / answer-driven (Audience) / personalized (merge tag) / interactive (Custom Code). This map IS the build instruction. See `references/dynamic-delivery.md` and `references/custom-code-and-merge-tags.md`.

### 5. Build custom-coded result pages (if used)
Self-contained sections, brand-scoped, with merge tags for personalization and editor-safe JS (guard against unswapped tags). For takeaway tools that must persist data, build standalone + link out. See `references/custom-code-and-merge-tags.md`.

### 6. Frame the follow-up sequences
One nurture sequence per result type, routed by the value ScoreApp passes to the email/CRM platform. Education-first for low bands, conversion-focused for the target band, light-touch for over-qualified. See `references/nurture-framework.md`.

### 7. Test in Draft Mode, then publish
Run the quiz end-to-end at lowest/mixed/highest scores AND down each Audience/Logic-Jump path. Confirm the right sections show and merge tags resolve (they render literally in the editor — only resolve on live/preview render). Only then publish.

## Reference files

Read the relevant file(s) for the step you're on:

- `references/quiz-architecture.md` — the 3-part question structure, ICP/band design, the "is a high score actually good?" inversion test, question-writing and lead-capture patterns.
- `references/scoring-engine.md` — Categories, points, tiers, the viability-gates-band pattern, tuning, shadow questions, and when each is worth it.
- `references/dynamic-delivery.md` — the four-mechanism decision tree in depth, with the per-section build-map method and worked examples.
- `references/custom-code-and-merge-tags.md` — merge-tag reference, custom-code-block reality, editor-safe JS pattern, the standalone-tool-for-persistence rule.
- `references/nurture-framework.md` — the 3-sequence email framework and how routing values flow to the email platform.

## Output style
When producing build deliverables, prefer a clear spec (logic, thresholds, section maps, setup steps) over vague advice. When producing result-page code, make it self-contained and on the client's brand. Always end a build plan with the open decisions the user must make before building.
