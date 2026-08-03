---
name: whs-compliance-audit
description: "Use when a user wants to audit, assess, review, or gap-analyse a business's Work Health & Safety (WHS) compliance, for an Australian or New Zealand client. Triggers: 'audit our WHS', 'are we compliant', 'WHS gap analysis', 'review our safety documentation', 'WHS health check', 'where are the gaps in our safety system', or any request to evaluate existing safety policies/SOPs/registers against legal requirements. Also trigger when a user points at a Google Drive folder of safety documents and asks what's missing, uploads a batch of policies for review, or asks whether their documentation would stand up to a regulator or insurer. Produces a standalone gap register plus remediation roadmap, the assessment sold first that justifies any document rewrites afterward. Use proactively when someone wants to know the STATE of their compliance, rather than wanting documents produced (that is the coupled whs-document-builder skill). If unsure whether a request is assess vs produce, assess first."
---

> v2026-08-04.1 · source-of-truth: `nowgroup-skills/skills/whs-compliance-audit/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.

# WHS Compliance Audit

A skill for auditing a trade or service business's Work Health & Safety compliance against the correct jurisdiction's legal framework, and producing a **standalone, defensible gap register + remediation roadmap**.

This is the front half of a two-skill system. It is **sold first** as an assessment product. Its output — a structured `findings.json` plus a human-readable report — is also the direct input to the coupled `whs-document-builder` skill, which produces the remediating documents as a post-audit upsell. Build the audit honestly and the document rewrites sell themselves.

## The core principle: assessment, not tick-boxing

The failure mode this skill exists to prevent is the *confident, branded, useless deliverable* — a report that looks compliant but bears no relationship to how the business actually operates, or worse, asserts legal duties that don't apply to the client's jurisdiction. Two rules guard against this:

1. **Never assert a legal duty without verifying it currently applies to the client's specific jurisdiction.** WHS/HSWA law in both countries is mid-reform and adoption varies by state (see [Critical: the law is moving](#critical-the-law-is-moving)). The reference files tell you *what to check and where* — they are not a frozen snapshot of the answer.
2. **Never raise a finding the client's own evidence already answers.** Ingest what they have *before* you interview them. Asking a client a question their uploaded SWMS already answers is tick-boxing in miniature, and it destroys the credibility of the whole audit.

---

## Critical: the law is moving

As of mid-2026 both jurisdictions are mid-reform. You **must** run a live verification step (Phase 1) before asserting duties. Known live changes to check the current status of:

- **Australia** — Safe Work Australia published amendments to the model WHS Act (Dec 2025) expanding incident notification (psychological harm, violent incidents, 15+ day absences, more dangerous-incident categories). **These only apply once adopted by each state/territory, and adoption is uneven** — early on, only the ACT had adopted. Always confirm the status for the client's *specific* state/territory before asserting these duties.
- **New Zealand** — The Health and Safety at Work Amendment Bill (introduced Feb 2026) proposes a shift to "critical risks" and a "small PCBU" (<20 workers) category. It is a **Bill, not yet law** — assess against HSWA 2015 as enacted, and flag the proposed changes as "coming, not yet in force."

Every finding you produce carries a `verified_as_at` date and a `source` so the deliverable is honestly dated and auditable.

---

## Process Overview

### Phase 0 — Scope and jurisdiction gate

Establish before anything else:

1. **Jurisdiction** — Australia or New Zealand. If Australia, **which state/territory** (NSW, VIC, QLD, SA, WA, TAS, ACT, NT) — this determines which regulator, which adopted amendments, and which licensing regime applies. There is no "Australia-wide" answer for adoption status.
2. **Business profile** — trade/industry, number of workers, sites, whether they engage subcontractors, whether they're a "small PCBU."
3. **Plant & equipment register** — what machinery/tools they actually run (this scopes which SOPs the builder will later need).
4. **What triggered the audit** — proactive health-check, insurer requirement, regulator notice, post-incident, tender prerequisite. This sets the urgency and tone of the roadmap.

Read the matching jurisdiction reference file now:
- Australia → [references/au.md](references/au.md)
- New Zealand → [references/nz.md](references/nz.md)

### Phase 1 — Verify the current legal framework (MANDATORY)

Before assessing anything, web-search the **current** status of the duties relevant to this client. At minimum verify:
- The governing Act and its current in-force version for the jurisdiction.
- For AU: which model-WHS amendments the client's **state/territory** has adopted as at today.
- For NZ: whether the Amendment Bill has progressed to law and what is currently in force.
- The current regulator name and incident-notification contact.
- Current licensing/competency requirements for the client's trade.

Record what you verified and the date. This populates the `verified_as_at` and `source` fields on every finding. If a search is inconclusive, mark the finding `verification: unconfirmed` rather than guessing — an honest "confirm with your regulator" is far more valuable than a confident error.

### Phase 2 — Ingest existing evidence (BEFORE interviewing)

The client may have documentation already. Pull and read it *first*, so the interview only fills genuine gaps. Three ingestion modes — use whichever the client's setup supports:

**Mode A — Google Drive folder.** The client points you at one or more Drive folders. Use the Google Drive connector to list and read every document. See [references/ingestion.md](references/ingestion.md) for the search/read pattern.

**Mode B — Mass upload / batch.** The client drops a batch of files (or a zip). Read each, classify it, and triage. See [references/ingestion.md](references/ingestion.md).

**Mode C — Nothing yet (greenfield).** No existing documentation — skip to interview. Every requirement starts as `absent`.

For each document found, classify it against the requirement list (see jurisdiction reference) and mark each requirement as **evidenced / partial / absent**, capturing *which* document evidences it and any obvious deficiency (undated, unsigned, generic-template, references wrong Act, etc.). This produces a provisional findings map.

### Phase 3 — Structured interview (close the gaps only)

Now interview — but **only about what the evidence didn't establish**. Use the gap map from Phase 2 to target questions. Cover: operating reality (do they actually run the plant their docs mention?), consultation/worker participation practices, training currency, incident history, and any control that's claimed-on-paper but you need to confirm is real-in-practice. Keep it tight — a client whose documents answered 60% of the checklist should get a short interview, not the full battery. Prefer the `ask_user_input` tool for structured multiple-choice where it speeds the client up.

The interview also gathers what's needed to decide **risk-module applicability** (Phase 3.5): do they store chemicals, operate registrable/licensed plant, disturb pre-ban building materials, or have monitored exposures? A few targeted questions here unlock the deeper assessment.

### Phase 3.5 — Risk-specific obligation modules

The baseline checklist (Phases 2–3) audits whether the client has the right *documents*. This phase audits whether they're meeting their *risk-specific legal duties* — the deeper question a regulator or insurer actually probes. Read [references/risk-modules.md](references/risk-modules.md).

Run all four modules — hazardous substances, plant registration & licensing, asbestos, health monitoring — and for each make an **explicit applicability determination** (`applicable` / `not-applicable`) with a stated reason. Every module appears in the audit even when N/A: a considered-and-excluded obligation is defensible; a silently-omitted one is indistinguishable from a forgotten one. Where a module is `applicable`, assess against the real duty and produce findings in the normal schema, and set the relevant `activity_flag` in `targeting` so the future legislative-push can target this cohort. Each module is jurisdiction-aware — apply the AU or NZ branch and Phase 1 verification.

### Phase 4 — Assess and build the gap register

For each requirement, produce a finding with: requirement, legal basis (Act + section/reg, jurisdiction-specific), current state (evidenced/partial/absent), the gap, risk rating, priority, and the remediation action — including **whether the remediation is a document the `whs-document-builder` can produce** (this is the coupling field and the upsell flag). See [references/gap-register.md](references/gap-register.md) for the exact schema — this schema is the contract with the builder skill, so follow it precisely.

### Phase 5 — Remediation roadmap

Sequence the findings into a roadmap: what to fix, in what order, grouped into sensible phases (e.g. *Immediate / legal-exposure*, *Near-term / foundational*, *Ongoing / maturity*). Flag which roadmap items are document rewrites the builder produces — this is the natural, honest upsell: "here is what's missing, here is the order to fix it, and items X/Y/Z are documents we can generate for you."

### Phase 6 — Produce the deliverable

Generate the audit report as a branded `.docx` (reuse the `whs-document-builder` common module for consistent branding if a logo is available) **and** write `findings.json` to the outputs directory. The `.docx` is the client-facing product; the JSON is the machine-readable handoff to the builder. Validate the `.docx` with the OOXML validator before presenting.

`findings.json` seeds the builder's `library.json` (the living registry — see `whs-document-builder/references/library.md`). When the audit converts to a build, the findings become the initial document set, and from then on `library.json` is the source of truth for ongoing maintenance and legislative push. Two findings always map to governing documents that anchor the system: `STD:WHS_Policy` (the signed commitment) and `STD:WHSMS_Manual` (the living system map) — flag these whenever a build is in prospect, as they're the capstone of the subscription value.

---

## Coupling to whs-document-builder

The output `findings.json` is consumed by `whs-document-builder` so that every document it produces is traceable to a specific finding. Each finding that is remediable-by-document carries:
- `remediation_type: "document"`
- `builder_document` — the document-builder's identifier for that doc (e.g. `SOP:Chainsaw`, `STD:SWMS_Template`)
- `tailoring_notes` — the specifics the builder needs (real plant list, real site hazards, the client's terminology) so the produced document reflects *this* business, not a template.

This is what stops the builder from reverting to greenfield guesswork. The audit did the grounding; the builder executes against it.

---

## Reference Files

- **[references/au.md](references/au.md)** — Australian framework: model WHS Act spine, the shared duties, the moving amendments, state/territory regulators + notification lines, AS standards, licensing regimes, and the AU requirement checklist.
- **[references/nz.md](references/nz.md)** — New Zealand framework: HSWA 2015, WorkSafe NZ, the proposed Amendment Bill, AS/NZS standards, NZ licensing, and the NZ requirement checklist.
- **[references/ingestion.md](references/ingestion.md)** — How to ingest evidence from a Google Drive folder (connector pattern) or a mass upload, classify documents against requirements, and build the provisional gap map.
- **[references/risk-modules.md](references/risk-modules.md)** — The four risk-specific obligation modules (hazardous substances, plant registration & licensing, asbestos, health monitoring): applicability tests, the real duties to assess, AU/NZ branches, and the new builder documents they require. Read in Phase 3.5.
- **[references/gap-register.md](references/gap-register.md)** — The exact `findings.json` schema (the coupling contract with the builder), risk-rating and priority rubrics, and the report structure.
