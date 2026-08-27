---
name: nowgroup-whitepaper-builder
description: Build, audit, and remediate NOW Group's own white papers under NOW doctrine — the paper as withholding instrument, run pathos/logos/ethos, scored by ASSAY. Use whenever the user wants to write a white paper, turn a question or a body of material into one, score or audit a draft, fix a HOLD or REVISE verdict, or diagnose why a paper did not open conversations. Triggers on "write a white paper", "audit this paper", "score this draft", "run ASSAY", "turn this into a white paper", "is this paper ready", "why didn't this paper land", or a draft pasted for review. Also fires on "pathos logos ethos", "convergence check", or "the carrier test" in a document context. Do NOT use for client proposals (nowgroup-proposal-builder), sell-sheets, capability decks, LinkedIn content, or papers written in a client's voice — this skill covers NOW Group's own papers only.
---

# NOW Group White Paper Builder

## 0 · Runtime doctrine fetch — mandatory, before anything else

Load the spec before the stage call. Two sources, in this order:

1. **Upstream, authoritative** — `web_fetch` on
   `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/reference/assay-spec.md`
2. **Bundled fallback** — `references/assay-spec.md`, shipped with this skill

Use upstream when it resolves. Fall back to the bundle when it does not, and **say which one is in play**, with its version line, in the first line of output. A silent fallback is how a skill quietly enforces last quarter's rules.

The spec carries the doctrine layers and the rubric the paper will be scored against. `nowgroup-whitepaper-assay` loads the identical file, so a paper is never built against a standard different from the one that will judge it. If the two skills report different spec versions, stop and reconcile before building.

If either fetch fails, **halt and say so**. Do not proceed from memory of this file. The doctrine is versioned upstream and drifts; working from a remembered version is how a skill silently enforces last month's rules.

---

## 1 · Phase 0 Context Discovery — mandatory, before any intake question

Never ask for what is already available. Scan in this order:

1. **Project files and uploads** — existing drafts, transcripts, research lists, doctrine files.
2. **`conversation_search`** — prior papers, prior ASSAY runs, the question already under examination, any NET_SYNC or Nexus material touching the subject.
3. **Banked NOW frameworks** — the Four I's, the 4Cs, the Partner Pendulum, the networking maturity ladder, the Mastermind book material. A paper often already has its mechanism sitting in a framework that has been built and never argued.
4. **Voice material** — NOW register is the default; no client voice block applies, since this skill covers NOW's own papers only.

State what Phase 0 found before asking anything. Regenerating context that already exists is the known failure mode here.

---

## 2 · Stage call — mandatory, before any output

Name the stage in one line:

| Stage | Trigger |
|---|---|
| **Commission** | A question exists, no paper yet |
| **Conversion** | Material exists — transcripts, frameworks, notes — and wants to become a paper |
| **Audit** | A draft exists and needs ASSAY |
| **Remediate** | An ASSAY verdict of HOLD or REVISE needs working |

Wrong stage produces confident, wasted output. Say which one and why.

---

## 3 · Commission gate

Before a word of draft, four things are locked. Ask for what is missing in **one round**, not an interrogation.

1. **The real question**, phrased as a question the operator does not currently know the answer to. A question with a known answer produces a pitch with footnotes.
2. **The counterfactual finding.** What result would have killed this paper? Written down. This pre-answers ASSAY K7, and a commission that cannot produce one does not proceed.
3. **Who bears the cost.** Named parties, named consequence. Pre-answers K9.
4. **The candidate field.** What resolutions already exist, who originated each, and where NOW has a commercial stake in one. Stakes identified here get declared on the page later.

If the operator cannot supply 1 or 2, the honest output is a note saying the paper is not yet a paper — the enquiry has not happened. Say that rather than writing around it.

---

## 4 · Doctrine — the three layers

Full text comes from the runtime fetch. Held in working memory during a build:

**Hosting.** Candidates may be named, examined and credited, including NOW's own and especially competitors'. Arbitration is prohibited. Two candidates live at the close. Equal scrutiny. Every candidate carries a named limitation and NOW's own carries the sharpest it honestly has. Stakes declared at the point of hosting. Credit by name.

**Sequence.** Pathos, then logos, then ethos, in order of appearance.

**Convergence.** Every section traces to the core through a load-bearing sub-core. Returns to the core are permitted where they carry nameable nuance.

---

## 5 · Build sequence

### Pathos — the opening

Name who is affected and what it costs them. Position the reader as someone able to act on those effects. Attach the animating question to a pain.

The opening does not establish NOW's standing, proximity, or history with the problem. That is ethos wearing a pathos coat and it fails the same way as a credentials page.

### Logos — the body

Three beats, in order:

- **Promise** — what should be possible; what the situation holds out.
- **Threat** — what is undermining it, with the mechanism named at a level below the observable outcome. This beat is where the paper's authority is manufactured. Ask why three times below the stated finding; depth three is usually the contribution and usually is not yet on the page.
- **Potential** — why the promise stays unrealised. Potential as indictment: what value gets called before it has arrived.

The candidate survey sits inside the body, under equal scrutiny. Proof runs through all three beats. Separate measured claims from reasoned ones in plain sight, and do it by marking the difference rather than by hedging uniformly.

### Ethos — the close

Two sides, both required:

- Why this is worth holding open **as a discussion** rather than closing as a conclusion.
- Why **the reader** is equipped to hold it. The most common near-miss is an ethos close that covers the author's standing and forgets the reader's, which halves the move, since the reader's standing is what converts them from audience to participant.

**Ethos is a modulation on the carrier, and the carrier is pathos and truth.** Standing rides on the analysis; it does not get its own transmission.

### Handback

Immediately after ethos. A named question the paper cannot answer, disconfirmable, settleable only inside the reader's situation. Where candidates were hosted, the question is *which of these applies here, and what would you need to know to tell.*

No recommendation. No next step. No door held open.

---

## 6 · The carrier test

Before returning any draft, strip every explicit statement of NOW's standing, experience, or capability.

- If the reader's sense of the author's standing **survives the strip**, ethos was modulated correctly. Restore the sentences or leave them out; either works.
- If the standing **collapses**, it was being declared rather than carried. The analysis is not yet doing the work, and no edit to the ethos section fixes that — the fix is in the body.

---

## 7 · Convergence discipline during the build

Write these down before drafting sections, not after:

1. The **core idea**, one sentence.
2. Each **sub-core**, with its path to the core stated explicitly.
3. Any section whose path cannot be stated is cut before it is written.

On repetition, the burden runs both directions:

- A cut is only valid if the cutter names the nuance that is absent.
- A defence is only valid if the writer locates the present nuance on the page.

Unlocatable nuance is slack, whoever is claiming it. Earned return completes the sentence *this changes what the core means because ______.*

---

## 8 · Pre-return gate

Nothing leaves without a **structural self-check**: kill conditions K1–K9, rhetorical sequence traced, carrier test run, convergence traced. Report it in four lines above the draft. This is the builder marking its own work and is not an audit.

A **full ASSAY** — both passes, ten dimensions, human gates — runs through `nowgroup-whitepaper-assay`, not here. Hand off when the draft is final or when the user asks for a score. The separation is deliberate: the builder knows what it intended, which is the one thing an auditor is required to disregard.

A kill found at self-check returns to the failing section before the draft is shown. Do not present a draft the builder already knows is held.

---

## 9 · Voice gate

NOW register: operator-intellectual, restrained, density over volume. The paper gets quieter as its argument sharpens.

Banned outright: leverage, synergy, paradigm. The "not X, it's Y" construction. Announcement lead-ins ("here's the part where…"). Symmetrical triplets. Throat-clearing transitions. Give / giving / generosity framing — use "create value" or "an increment of value".

Note the trap: the existing NOW doctrine file uses the banned not-X-it's-Y construction in several places. Doctrine sourced verbatim still passes through this gate.

---

## 10 · Output

Markdown artefact. Length follows the argument; the three tiers are guidance, not targets.

| Tier | Range | Shape |
|---|---|---|
| Brief | 1,200–1,800 | One mechanism, one candidate pair |
| Standard | 2,500–4,000 | Full sequence, full survey |
| Deep | 6,000+ | Appendix, method notes, multi-case |

docx on request only. Every output states its rationale in one line: what was built, at which stage, and what it is for.

---

## 11 · Out of scope

Client proposals → `nowgroup-proposal-builder`. Partner enablement, sell-sheets, capability decks → different asset, different physics, does not score here. Papers in a client's voice → out of scope by decision; this skill covers NOW Group's own papers only.
