# `library.json` — The Living Registry (Source of Truth & Propagation Contract)

This is the master data model for a client's WHS system. It is the evolution of the audit's `findings.json`: where `findings.json` captures the *assessment*, `library.json` captures the *living document library* and everything needed to maintain it. **It is the single source of truth.** Documents are generated from it and are never the source of truth themselves.

This schema is deliberately designed to be **portal-ready** (Phase 2). The hosted portal/landing page reads exactly this structure — so the skill layer and the future UI speak the same data language, and Phase 2 is a wiring job, not a redesign.

---

## The one load-bearing rule

**Changes are made to `library.json`, never to the generated Word files.** A change to a data field, a shared content block, or a document's content is made *here*, and then every document that consumes the changed element is regenerated. Hand-editing a `.docx` puts it out of sync with the registry, and in a compliance system a document that disagrees with the register is worse than no system. Every client is told this explicitly (it's stated in the WHSMS Manual, Section 2).

This rule is what makes propagation trustworthy: there is exactly one place any fact can live.

---

## Schema

```json
{
  "schema_version": "1.0",
  "audit_meta": {
    "client_name": "Acme Trades Pty Ltd",
    "client_id": "acme-trades",            // stable tenant key — the portal's primary key
    "jurisdiction": "AU",
    "state_territory": "QLD",
    "regulator": "Workplace Health and Safety Queensland",
    "industry": "electrical",
    "worker_count": 14,
    "small_pcbu": false,
    "logo_ref": "logo_b64.js",
    "doc_prefix": "ACME",
    "created": "2026-06-10",
    "verified_as_at": "2026-06-10",
    "auditor": "AUBIT / NOW Group"
  },

  "targeting": {
    // The metadata the central legislative-push uses to decide which clients a change applies to.
    // Same fields as audit_meta, surfaced here as the explicit targeting layer so push logic is unambiguous.
    "jurisdiction": "AU",
    "state_territory": "QLD",
    "trades": ["electrical"],
    "worker_count": 14,
    "activity_flags": ["uses_hazardous_substances", "operates_registrable_plant"]
    // activity_flags come from the audit's triggered-module assessment (see risk-modules.md)
    // and let a push target e.g. "all clients who use hazardous substances".
  },

  "data_fields": {
    // Shared scalar facts referenced by many documents. Change once → every consuming document regenerates.
    "whs_officer_name": "Jane Smith",
    "whs_officer_position": "Managing Director",
    "emergency_contact": "Jane Smith — 0400 000 000",
    "notification_line": "1300 362 128",     // verified per-jurisdiction; do not hard-code blindly
    "principal_act": "Work Health and Safety Act 2011 (QLD)",
    "review_cycle_default": "annual"
  },

  "content_blocks": {
    // Shared PASSAGES (not just scalars) referenced by multiple documents.
    // This is where legislative push pays off most: change the block once, every document containing it reissues.
    "legal_basis_primary": {
      "text": "This document is maintained under the Work Health and Safety Act 2011 (QLD) and supporting Regulations.",
      "version": 3,
      "verified_as_at": "2026-06-10",
      "source": "https://www.safeworkaustralia.gov.au/...",
      "consumed_by": ["WHS_Policy", "SWMS_Template", "Risk_Assessment_Form"]
    },
    "incident_notification_procedure": {
      "text": "Notify [regulator] immediately of a notifiable incident. Preserve the incident site until an inspector authorises resumption...",
      "version": 5,
      "verified_as_at": "2026-06-10",
      "source": "https://www.worksafe... / model WHS Act notification fact sheet",
      "consumed_by": ["Emergency_Response_Plan", "WHS_Worker_Induction_Checklist", "WHSMS_Manual"]
    },
    "hierarchy_of_controls": {
      "text": "Controls are applied in order: elimination, substitution, isolation, engineering, administrative, PPE.",
      "version": 1,
      "verified_as_at": "2026-06-10",
      "source": "model WHS / HSWA risk management code",
      "consumed_by": ["Risk_Assessment_Form", "Manual_Handling_Policy", "Working_at_Heights_Policy"]
    }
  },

  "documents": [
    {
      "doc_key": "WHS_Policy",               // matches builder identifier STD:WHS_Policy
      "builder_id": "STD:WHS_Policy",
      "title": "Work Health & Safety Policy",
      "code": "ACME-POL-001",
      "category": "governing",               // governing | policy | procedure | register | permit | sop
      "filename": "ACME_WHS_Policy.docx",
      "version": "1.2",
      "status": "current",                   // current | due-for-review | superseded | draft
      "owner": "Jane Smith (Managing Director)",
      "last_reviewed": "2026-06-10",
      "next_review": "2027-06-10",
      "consumes_fields": ["whs_officer_name", "whs_officer_position", "principal_act"],
      "consumes_blocks": ["legal_basis_primary"],
      "source_finding": "F-001",             // traceability back to the audit finding it closed
      "tailoring_notes": "14 workers, electrical, QLD; engages subcontractors."
    },
    {
      "doc_key": "SOP_Chainsaw",
      "builder_id": "SOP:Chainsaw",
      "title": "Chainsaw — Safe Operating Procedure",
      "code": "ACME-SOP-CS-001",
      "category": "sop",
      "filename": "ACME_SOP_Chainsaw.docx",
      "version": "1.0",
      "status": "current",
      "owner": "Site Supervisor",
      "last_reviewed": "2026-06-10",
      "next_review": "2027-06-10",
      "consumes_fields": ["principal_act"],
      "consumes_blocks": ["hierarchy_of_controls"],
      "source_finding": "F-011",
      "tailoring_notes": "Client runs Stihl MS261; tree work near power lines on some sites."
    }
  ],

  "change_log": [
    {
      "date": "2026-06-10",
      "version": "1.0",
      "summary": "Initial library generated from audit.",
      "trigger": "initial build",
      "author": "AUBIT / NOW Group",
      "documents_affected": ["ALL"]
    },
    {
      "date": "2026-07-15",
      "version": "1.1",
      "summary": "WHS officer changed from Jane Smith to Raj Patel.",
      "trigger": "client change request",
      "author": "client portal",
      "data_fields_changed": ["whs_officer_name", "whs_officer_position", "emergency_contact"],
      "documents_affected": ["WHS_Policy", "Emergency_Response_Plan", "WHS_Worker_Induction_Checklist"]
    },
    {
      "date": "2026-09-01",
      "version": "1.2",
      "summary": "Updated incident notification procedure to reflect QLD adoption of expanded notifiable-incident categories.",
      "trigger": "legislative — AUBIT central update",
      "author": "AUBIT (approved: [name])",
      "content_blocks_changed": ["incident_notification_procedure"],
      "documents_affected": ["Emergency_Response_Plan", "WHS_Worker_Induction_Checklist", "WHSMS_Manual"]
    }
  ]
}
```

---

## How propagation works (single client)

Propagation = **regeneration**, not in-place editing. The algorithm:

1. A change is made to a `data_field`, a `content_block`, or a `document`'s own content/tailoring.
2. Determine the affected document set:
   - field changed → every document whose `consumes_fields` includes it
   - block changed → every document in that block's `consumed_by` (and whose `consumes_blocks` lists it)
   - document changed → just that document
3. Bump the changed element's version and the affected documents' versions.
4. Regenerate **only** the affected documents (the builder reads `library.json`, rebuilds those `doc_key`s).
5. Always regenerate the **WHSMS Manual** — it reflects the register, so any change touches it.
6. Append a `change_log` entry: date, new version, summary, trigger, author, and `documents_affected`.

Because the build is deterministic from the registry, this is safe and repeatable. Nothing is ever silently lost, because nothing of value lives only in the `.docx`.

---

## How central legislative push works (Phase 2, multi-client)

The single-client propagation above, fanned out across all client libraries — but **targeted and human-gated**. The fan-out is easy; the targeting and the gate are what make it safe. A legislative change is rarely globally identical (we confirmed this: AU amendments apply per-state, NZ reform is mid-flight), so a push must not blindly overwrite every client.

The staged sequence:

1. **Author the change centrally** — update the relevant `content_block` text + version + source, as a *proposed* update.
2. **Target the cohort** — using each client's `targeting` block: jurisdiction, state/territory, trade, worker_count, activity_flags. e.g. "QLD clients who operate registrable plant." Clients outside the cohort are untouched; borderline clients are marked `pending-confirmation` rather than auto-applied.
3. **Human approval gate (MANDATORY)** — AUBIT (you/Aaron) approves the legal interpretation **once**. The automation is in propagation and targeting; the legal judgement is never auto-applied across tenants. Pushing a wrong interpretation to every client simultaneously is the one failure that would damage the trust the subscription sells.
4. **Fan out** — for each client in the approved cohort, run the single-client propagation: update the block, regenerate affected documents + their WHSMS Manual, append a `change_log` entry stamped `trigger: "legislative — AUBIT central update"` with the approver's name.
5. **Notify** — each affected client gets the "your library was updated" message. From the client's side it "just happened over the weekend" — they never see the gate. (This is the perceived-value moment: change logged Friday / legislation pushed centrally → live in their documents by Saturday.)

The targeting metadata that makes this safe is the *same* metadata that grounds the audit. Being honest about jurisdiction up front is what later makes mass-push correct.

---

## Why this schema is portal-ready

The Phase 2 portal/landing page reads this file directly:
- **Live posture** ← `documents[].status` (counts of current / due-for-review).
- **Document map** ← `documents[]` with `category` and hyperlinks to `filename`.
- **Change log** ← `change_log[]`.
- **Review alerts** ← `documents[].next_review`.
- **Push targeting** ← `targeting`.

Design the data here correctly now, and Phase 2 is wiring a UI onto an existing model. That is the entire point of doing the schema first.
