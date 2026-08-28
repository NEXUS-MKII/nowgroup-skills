---
name: nowgroup-whitepaper-assay
description: Run ASSAY — the NOW Group white paper audit — against a draft, with blocking human gates at every point the model cannot know the answer. Scores ten dimensions across two passes, applies nine kill conditions, and returns SHIP, REVISE or HOLD. Use whenever a white paper draft needs auditing, scoring, or a verdict checked — "run ASSAY", "audit this paper", "score this draft", "is this paper ready to publish", "why did this paper fail", "re-audit after my edits", or a paper pasted with any request to assess it. Also fires on "kill check", "convergence check", "carrier test", "survey symmetry". Do NOT use to write or fix a paper — that is nowgroup-whitepaper-builder. Do NOT use for proposals, sell-sheets, or client-voice papers.
---

> v2026-08-18.1 · source-of-truth: `nowgroup-skills/skills/nowgroup-whitepaper-assay/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.

# ASSAY — white paper audit runner

## 0 · Runtime spec fetch — mandatory

Two sources, in this order:

1. **Upstream, authoritative** — `web_fetch` on
   `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/reference/assay-spec.md`
2. **Bundled fallback** — `references/assay-spec.md`, shipped with this skill

Use upstream when it resolves. Fall back to the bundle when it does not, and **report which is in play, with its version line, at the head of the audit output**. An audit that does not name its standard is not an audit.

The spec carries the doctrine layers, the nine kill conditions, the ten dimensions with their weights and band anchors, the anti-gaming caps and the output format. This file is the **procedure**; the spec is the **standard**.

Never audit from memory of the rubric, even when both sources fail — halt instead. A remembered rubric drifts toward whatever the model finds reasonable, and what the model finds reasonable is the conventional B2B template, which is the exact thing ASSAY exists to catch.

---

## Why this runs with gates

Four inputs to the audit are unknowable from the page:

- what commercial outcome the paper serves
- what finding the author would have accepted as disconfirming
- where NOW holds a stake among the hosted candidates
- whether a passage the model reads as repetition is carrying nuance the model failed to hold

A model that supplies these itself produces a fluent audit resting on four guesses, and the guesses will be flattering because they are reconstructions from a text written to be persuasive. Each is a **blocking gate**. No verdict is issued until they are answered by a person.

---

## Gate 1 · Who is auditing

Ask, before reading:

> Are you the author of this paper, a second reader inside NOW, or outside it?

**If author:** the self-audit condition applies. Dimensions 5 (Restraint Integrity) and 6 (Survey Symmetry) are scored, reported, and **flagged as provisional** — an author reads their own intent as ambient rather than as text and is the reader least able to see the leak. SHIP is not available on a self-audit; the ceiling is REVISE-pending-second-reader.

**If second reader or outside:** proceed normally.

---

## Gate 2 · Intent declaration

The person writes, in their own words, before the model reads for score:

> *The commercial outcome this paper is built to make possible is ______.*

**The model does not draft this sentence, offer options for it, or infer it from the paper.** Inferring it converts the control into a reconstruction and the audit tests the paper against itself.

If the person declines, or says there is no commercial outcome, record that verbatim and proceed. A paper with genuinely no commercial intent is auditable; the declaration simply records that dimension 5 has nothing to leak.

---

## Gate 3 · Stake map

Ask:

> Which of the candidates in this paper does NOW have a commercial interest in — sells, resells, partners on, or would be the obvious provider of?

Needed for K3 (undeclared stake) and for the dimension 6 ceiling, which requires that NOW's own candidate carry the sharpest stated limitation on the page. Without the map, the model cannot tell which candidate is NOW's, and every asymmetry looks like ordinary variation.

If no candidates are hosted, record it. Dimension 6 is not applicable and its 13 points redistribute proportionally across dimensions 1–5, noted in the output.

---

## Pass 1 · Structural

Model runs, unassisted, and reports in this order:

1. **Kill check K1–K9.** Pass or fail each, one line of evidence per fail.
2. **Sequence trace.** Where pathos ends, where the three logos beats sit, where ethos begins. Flag ethos material appearing before the analysis regardless of where the credentials sit.
3. **Carrier test.** Strip the explicit standing statements. Report whether the reader's sense of standing survives.
4. **Convergence trace.** Core idea in one sentence. Each sub-core with its stated path to the core. Sections whose path cannot be stated, listed.

### Gate 4 · The counterfactual

Before K7 can be marked pass, ask:

> What finding would have killed this paper?

The model may say what it believes the paper's own logic implies, but the answer that counts is the person's, and it is recorded verbatim. An answer that cannot be produced **is** the audit result: K7 fails, verdict is HOLD, and the finding is that the enquiry did not happen.

### Kill exit

Any kill returns **HOLD with no score**. Do not compute or mention a number — a scored HOLD gets negotiated over the number instead of rewritten.

### Gate 5 · Override

A HOLD may be overridden by the person, and the override is recorded in the audit output with the reason given. The verdict does not change; the record shows a HOLD published over. Silent overrides are what make an audit decorative.

Pass 1 failure forfeits pass 2. Do not score a paper that has already been held.

---

## Pass 2 · Substantive

Score all ten dimensions per the spec. Rules that bind:

- **No score without a quoted anchor** from the draft. An uncited score is void and the audit is incomplete.
- **Score the page, not the intent.** The author's account of what they meant is inadmissible; the reader will not have it.
- **Absence is admissible** on dimensions 2, 4, 6 and 8. What the paper declined to include is a finding about the paper.
- Apply the anti-gaming caps from the spec before totalling.

### Gate 6 · Repetition, right of reply

Where dimension 8 flags a passage as slack repetition, the model states which nuance is absent — a flag with no such statement is void and the passage stands.

The person then has right of reply: locate the present nuance on the page. A defence that asserts nuance without locating it is void and the passage goes.

The same procedure runs in reverse. Where the person believes a passage the model kept is bloat, the model must name what the passage adds or drop it.

Unlocatable nuance is slack, whoever is claiming it.

---

## Output

```
AUDITOR:        author (self-audit, 5 & 6 provisional) / second reader / outside
INTENT:         [verbatim, from Gate 2]
STAKE MAP:      [verbatim, from Gate 3] — or none hosted, 13 pts redistributed

PASS 1
  KILLS:        K1–K9, pass/fail, evidence on each fail
  COUNTERFACTUAL: [verbatim, from Gate 4]
  SEQUENCE:     pathos / promise / threat / potential / ethos both sides
  CARRIER TEST: survives / collapses
  CONVERGENCE:  core, sub-cores with paths, untraceable sections

PASS 2                          (omit entirely on kill)
  DIMENSIONS:   score, weight, quoted anchor, one-line finding
  CAPS APPLIED: any anti-gaming cap and what triggered it
  SCORE:        nn / 100

VERDICT:        SHIP / REVISE / HOLD
OVERRIDE:       [reason, if Gate 5 used]
REMEDIATION:    three instructions, ranked by points recoverable
STRENGTH:       one line on what this paper does that most cannot
```

---

## Re-audit

Scoped to the named dimensions, always including 5, 6 and 8 — restraint and symmetry break on any edit, and convergence breaks on any structural one.

Gates 2 and 3 carry forward within a session. Gate 4 is re-run if the argument changed. Gate 1 is re-run if the auditor changed.

---

## Handoff

A REVISE or HOLD hands to `nowgroup-whitepaper-builder` at stage **Remediate**, carrying the dimension scores, the quoted anchors and the ranked remediation. The builder does not re-derive the diagnosis.
