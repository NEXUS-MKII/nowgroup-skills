# Quiz Architecture

How to design the questions and the band logic *before* touching ScoreApp settings.

## Table of contents
1. The 3-part question structure
2. The inversion test (is a high score actually a good lead?)
3. Designing bands around the ICP
4. Question-writing patterns
5. Lead-capture and trust rules

---

## 1. The 3-part question structure

A reliable qualification quiz separates three jobs. Don't blend them.

**Part 1 — Capture & Viability.** Name, email, and the 2–4 categorical signals that determine whether this person is even a fit (e.g. homeowner status, income bracket, household structure). These are the gating signals that later drive Audiences and Logic Jumps. Keep it to what you'll actually route on.

**Part 2 — Scored Diagnostic.** The questions that produce the score — usually yes/no or scaled. Each should map to a best-practice the business cares about, so that a "weak" answer becomes a named gap you can address on the result page. Everyone should answer these; they are what make every result page meaningful, including the lowest band's.

**Part 3 — Qualifiers / Intent.** Buying-readiness and context questions (current situation, 90-day goal, primary obstacle, preferred solution, open text). Often **unscored** — their value is in human follow-up and in Audience rules, not in the band number. The open-text final question is frequently where the most valuable sales context lives.

Keep the whole thing lean. Every added question lowers completion. A question earns its place only if it (a) produces score, (b) drives routing, or (c) yields follow-up gold. "Nice to know" questions fail this test.

---

## 2. The inversion test (do this every time)

Before equating "high score = best lead," ask: **for this business, does a high scorer actually convert and refer — or have they already got it handled?**

In many advisory/service funnels, the highest scorers are the *worst* commercial prospects: they're sophisticated, they DIY, they extract value and leave, and they don't refer. Meanwhile the *middle* band — enough resources to act, but real gaps — is the gold. And the lowest band needs nurturing into readiness, not a sales call.

If that's the shape, **a pure score-driven traffic light routes backwards.** The fix: viability (Part 1) and/or intent (Part 3) gate the band, and the score only refines *within* the viable group. A high score then becomes the one thing that lifts someone into the deprioritized "already sorted" band — not the thing that wins your best CTA.

Always state, in business terms, what each band *means* and *what you want to happen to them*, before defining percentage thresholds.

---

## 3. Designing bands around the ICP

- Name each band for the person, not the score ("Activation / Accelerator / Architect", not "Low/Med/High").
- Decide the **target band** explicitly — the one the whole funnel exists to deliver to its CTA.
- Decide what the **off-target bands** get instead (education + a takeaway tool for the not-ready; light-touch acknowledgement for the over-qualified).
- Identify the **subtypes** within a band that genuinely need different content (e.g. single-income vs dual-income changes the financial maths and therefore the advice). Subtypes are an Audiences job.

---

## 4. Question-writing patterns

- **Scored diagnostic questions**: phrase so a "yes" is unambiguously the best-practice behaviour. Put the diagnostic intent in a parenthetical for your own build notes.
- **Accessibility over precision**: use the term the user understands ("net worth") even if the business tracks something more precise ("net investment position") — then educate on the result page. A question only works if it's answered honestly and easily.
- **Route, don't disqualify**: prefer answer options that let you route someone to an alternative offer/path rather than a dead "you fail." This both converts better and reads as more generous.
- **Surface the objection**: if the business has a known #1 objection (e.g. "advice is too expensive"), include an answer option that surfaces it, so the result page can pre-empt it.
- **90-day framing for goals**: people can't achieve big outcomes in 90 days but they can take a structural first step — word goal questions to reflect realistic near-term action.

---

## 5. Lead-capture and trust rules

- **Capture name + email early** (while engagement is high) but **defer the phone number** to a later, higher-trust step (e.g. the booking step). Asking for a phone number before establishing trust measurably increases opt-outs — people don't yet know the brand or what's behind the quiz.
- **Drop fields you won't use** (e.g. region) — every field is friction.
- **Tell the user their data is private** where relevant, especially around interactive tools — it reduces drop-off and matches a privacy-conscious audience.
