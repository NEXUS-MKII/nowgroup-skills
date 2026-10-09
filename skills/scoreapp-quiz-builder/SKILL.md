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
  result/PDF personalization. Establish ground truth FIRST (see Step 0): if the
  ScoreApp MCP is connected, read the live scorecard through it; otherwise
  web-search the current docs, because features and plan gating change over time.
---

> v2026-10-10.1 · source-of-truth: `nowgroup-skills/skills/scoreapp-quiz-builder/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.
>
> **2026-10 refresh:** reworked around the **ScoreApp MCP connector**, which can now read a scorecard's live config AND build it (questions, categories, tiers, audiences). Web-search is now the fallback, not the first move. Added the verified resume-link format and the "started ≠ abandoned" finding.

# ScoreApp Quiz Builder

This skill captures a battle-tested method for building quiz funnels in ScoreApp:
the scoring engine, the routing/visibility logic, the custom-coded result pages,
the takeaway tools, and the email sequences that follow. It exists because
ScoreApp has several non-obvious constraints and capabilities that, if
misunderstood, lead to architectures that cannot be built — and because the
platform changes, so assumptions must be re-verified every time.

## Step 0 — Establish ground truth FIRST (non-negotiable)

ScoreApp's feature set, plan gating, merge-tag syntax, UI — and a given
scorecard's live config — all change over time. Before giving any build advice,
establish what is actually true right now. Two routes, in order of preference:

**A. If the ScoreApp MCP is connected (preferred) — read the live account.**
A ScoreApp MCP connector exposes the real scorecard, not a secondhand doc. Use
its READ tools to ground every decision:
- `list-scorecards` → find the scorecard and its id.
- `get-scorecard-settings` → status, lead-form fields, **score tiers** (names +
  bands), tracking, sub-domain.
- `list-scorecard-questions` / `list-scorecard-categories` → the real questions
  and their **UUIDs** (needed for merge tags and answer filtering).
- `list-results` (`status: started | finished`), `list-result-answers`,
  `list-result-scores`, `get-result` (`include: answers, scores, source,
  activity`) → real responses, to validate scoring and segmentation against.
  (`additional_data` is plan-gated.)

This is ground truth; prefer it over any doc or memory. See the MCP section below
for the full tool list and the build tools.

**B. Otherwise — web-search the current docs.** Run searches such as:
- `ScoreApp [feature] documentation` (e.g. "ScoreApp Audiences documentation")
- `ScoreApp merge tags list site:support.scoreapp.com`
- `ScoreApp [feature] plan tier` (to confirm what's gated to Pro/Business)
- `ScoreApp custom code block results page` (capabilities change)

Fetch `support.scoreapp.com` articles directly when found.

Treat this skill's reference files as a strong prior to be re-validated, not as
gospel. Be especially skeptical of secondhand claims (forum posts, AI how-tos) —
the thread that produced this skill hit an AI assistant that flip-flopped on
whether custom-code blocks could be toggled dynamic. If a live read or current
doc contradicts this skill, **trust the live source** and tell the user what
changed. If you cannot verify a capability, say so plainly rather than assuming.

## The ScoreApp MCP — it can read AND build

When the connector is present, the skill is no longer limited to writing a spec
for someone to click together by hand. It can **execute** much of the build.

- **Read tools** (ground truth — Step 0): `list-scorecards`,
  `get-scorecard-settings`, `list-scorecard-questions`,
  `list-scorecard-categories`, `list-results`, `list-result-answers`,
  `list-result-scores`, `get-result`, `get-result-source`,
  `get-scorecard-statistics`, `list-audiences`, `list-result-activity`, and the
  answer/score/page/question statistics tools.
- **Write/build tools:** `create-scorecard`, `create/update/delete-scorecard-question`,
  `create/update/delete-scorecard-category`, `update-scorecard-score-tiers`,
  `create/update/delete-audience`, `create/update/delete-scorecard-lead-form-field`,
  `update-scorecard-settings`, `update-scorecard`, `preview-audience-matches`.

**Safe build pattern:**
1. **Read first** (Step 0) — never write blind against assumptions.
2. Build in a **draft** scorecard and tune tiers with real/test responses.
3. After each write, **read the object back** to confirm it landed as intended.
4. Treat `delete-*` with care — confirm before removing questions/categories/tiers.
5. Write tools change the **client's live asset** — confirm with the operator
   before structural changes to a live scorecard.

The MCP does NOT replace architecture judgement (the four mechanisms below), and
it does not cover everything — custom-code result pages, nurture emails, and some
plan-gated features still need the UI and current docs.

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
- **"Started" is not "abandoned".** `list-results status=started` returns everyone who did not reach their result — but most have answered nearly every question and simply did not hit the final step. Distinguish *near-complete* (worth a "finish the scorecard" nudge) from *truly abandoned* (gave little). Treating them as one bucket wastes the near-complete ones, who are the warmest leads on the list.
- **Resume/continuation links are not in the API — build them from the key.** A started/abandoned lead's on-page `result_url` is blank until completion, and the API/MCP does not expose a resume URL. The format is deterministic: `https://<subdomain>.scoreapp.com/continue/<result key>` (verified on the MyWealth Quiz, 2026-10). Generate it from each result's `key`; never invent other params. This is what lets a follow-up drop someone back exactly where they stopped.

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

## Worked example — the MyWealth Quiz (MyFuture)

A live reference build, fully readable through the MCP (scorecard id
`cf4c791c-8583-4d49-a5f7-dcfdd36440b5`, subdomain `myfuturewealth`):

- **19 quiz questions + phone capture**, in the 3-part structure: *Part 1 capture*
  (household income, home ownership), *Part 2 scored diagnostic* (yes/no habits —
  budget review, emergency fund, KiwiSaver strategy, net worth tracking,
  insurance, written plan), *Part 3 intent/qualifiers* (current situation, 90-day
  goal, primary obstacle, support preference, wealth-app interest, and a free-text
  "one thing to know").
- **5 score tiers:** Activation (0-29) → Accelerator One (30-46) → Accelerator
  Two (47-62) → Accelerator Three (63-78) → Freedom Architect (79-100).
- **Segmentation runs on what they asked for (Part 3), not the score** — a low
  scorer who asked for an adviser outranks a high scorer who wants to self-serve.
  The full routing, segments, and CTA ladder live in
  `MyFuture Quiz/MyFuture_Lead_Segmentation_Method.md`.
- Follow-ups use resume links (`/continue/{key}`) and the Lara-voice Message
  Engine; abandoned-cart emails fire natively (visible in `get-result` activity).

## Output style
When producing build deliverables, prefer a clear spec (logic, thresholds, section maps, setup steps) over vague advice. When producing result-page code, make it self-contained and on the client's brand. Always end a build plan with the open decisions the user must make before building.
