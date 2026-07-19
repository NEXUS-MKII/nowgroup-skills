# Custom Code & Merge Tags

Building custom-coded result-page sections, personalizing with merge tags, and the persistence rule for takeaway tools.

> **Syntax verified against the live builder on 2026-07-18** (result-page HTML section, merge-field inserter). ScoreApp still changes over time — re-confirm in Step 0 — but the `{curly}` scheme and the UUID-keyed families below are what the inserter actually writes today. (Earlier versions of this skill claimed `[[double_brackets]]` on-page; that was wrong — see §2.)

## Table of contents
1. Custom Code blocks — what's real
2. Merge-tag reference (VERIFIED)
3. The editor-safe JS pattern
4. The persistence rule (standalone tools)
5. Brand-scoping custom sections

---

## 1. Custom Code blocks — what's real

- Custom Code blocks (raw HTML/CSS/JS as a section) are a Pro/Business-tier feature. Confirm the account has them.
- **They cannot be natively toggled into score-tier tabs** the way text sections can — the Dynamic Content toggle is hidden on code blocks. To make a code block conditional: wrap it in an **Audience**, or use **JS show/hide driven by a merge tag** inside the block.
- Merge tags are swapped **server-side before render**. In the builder/editor they show as literal text (e.g. `{overall_score}`), and only resolve to real values on the live or preview render. This means you cannot fully test merge-tag-driven JS from inside the editor — use Preview/Draft run-throughs.
- Whether a merge tag resolves *inside a `<script>`* (vs only in visible HTML) and whether the block runs in a sandboxed iframe are the two things to **test on one block before betting the build on it.** If sandboxed, in-script merge tags and cross-section DOM access may not work.

## 2. Merge-tag reference (VERIFIED)

**On-page custom HTML/script uses single curly braces `{...}`.** External Redirect URLs use the same `{...}` tokens. (There is no `[[double_bracket]]` scheme — disregard any older reference to it.)

The inserter (the `{ }` button in an HTML section) groups tags into six menus. **Two shapes exist:**

**A) Flat, portable tokens** — safe to hardcode in any build:

| Purpose | Token |
|---|---|
| First name | `{first_name}` |
| Last name | `{last_name}` |
| Email | `{email}` |
| Completed date | `{scorecard_taken_date}` |
| Completed timestamp | `{scorecard_taken_timestamp}` |
| Lead ID | `{result_key}` |
| UTM source / medium / campaign / content / term | `{utm_source}` `{utm_medium}` `{utm_campaign}` `{utm_content}` `{utm_term}` |
| Scorecard name | `{scorecard_name}` |
| Report (results) URL | `{report_url}` |
| Highest / lowest category name | `{highest_category_name}` `{lowest_category_name}` |
| All scorecard answers (block) | `{scorecard_answers}` |
| All lead-form answers (block) | `{lead_form_answers}` |
| Question count / response count | `{question_count}` `{response_count}` |
| **Overall** score — percent | `{overall_score}` |
| Overall — tier label | `{overall_score_tier}` |
| Overall — actual | `{overall_score_actual}` |
| Overall — out of 10 / rounded | `{overall_score_out_of_10}` `{overall_score_out_of_10_rounded}` |

**B) UUID-keyed tokens** — the UUID belongs to a specific category/question in *this* scorecard. **Never hardcode the UUID across builds** — always pull the exact token from the inserter in the target scorecard.

| Purpose | Token shape |
|---|---|
| A named category's score | `{score.<CATEGORY_UUID>.percent}` · `.tier` · `.relative_percent` · `.actual` · `.out_of_10` · `.out_of_10_rounded` |
| A quiz question | `{quiz_question.<QUESTION_UUID>.question}` · `.answer` · `.answer_value` · `.score` |
| A lead-form field | `{signup_question.<SIGNUP_UUID>.question}` · `.answer` |

Notes that trip people up:
- **Overall vs named category differ in shape.** Overall is flat (`{overall_score...}`); a named category is `{score.<UUID>.*}` and *additionally* exposes `relative_percent` (its share of 100% across all categories). Overall has no `relative_percent`.
- **Phone / company / any custom lead field** are *signup questions* → `{signup_question.<UUID>.answer}`, not flat tokens. Only First name, Last name and Email are flat.
- Use quotes around string tags in JS (`var n = "{first_name}";`), none around numeric tags used in math (`var s = {overall_score};`).

## 3. The editor-safe JS pattern

Because tags render literally in the editor, guard your script so the block never looks broken there and degrades to a sensible default:

```html
<script>
  var raw = "{overall_score}";         // literal in editor, real number on live render
  var score = parseInt(raw, 10);
  if (isNaN(score)) { score = -1; }    // editor / unswapped fallback
  // ... logic, with a branch that handles score === -1 gracefully
</script>
```

Never let an unswapped tag throw an error or display a raw `{overall_score}` to a user.

## 4. The persistence rule (standalone tools)

**localStorage/sessionStorage do not work reliably inside ScoreApp-embedded code.** So if a takeaway tool must *save* the user's inputs (a budget planner, a tracker they return to), build it as a **standalone page hosted outside ScoreApp** and link to it from the result page (and/or email it). Embedded-in-ScoreApp = display/compute only, no persistence. Standalone = full persistence, printable, genuinely "theirs."

A common pattern: embed a lightweight version on the result page for instant value, AND send the standalone, data-saving version in the first follow-up email. Same tool, two touchpoints.

## 5. Brand-scoping custom sections

Make each custom section self-contained so it drops in cleanly regardless of ScoreApp's surrounding chrome: scope all CSS under a wrapping class, set its own background/padding/fonts, import the brand font, and use CSS variables for the palette. Use spaced hyphens rather than em-dashes in copy to avoid encoding artifacts (em-dashes sometimes render as `â€"` depending on how the platform serves the block). Avoid relying on the parent page's styles.
