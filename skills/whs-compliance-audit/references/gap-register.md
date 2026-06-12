# Gap Register — `findings.json` Schema and Report Structure

This file defines the **coupling contract** between `whs-compliance-audit` and `whs-document-builder`. The audit writes `findings.json`; the builder reads it. Follow the schema precisely — a malformed field breaks the handoff.

---

## The `findings.json` schema

```json
{
  "audit_meta": {
    "client_name": "Acme Trades Pty Ltd",
    "jurisdiction": "AU",
    "state_territory": "QLD",
    "regulator": "Workplace Health and Safety Queensland",
    "industry": "generic trades",
    "worker_count": 14,
    "small_pcbu": false,
    "audit_trigger": "insurer requirement",
    "audit_date": "2026-06-10",
    "verified_as_at": "2026-06-10",
    "ingestion_modes_used": ["drive_folder", "interview"],
    "auditor": "AUBIT / NOW Group"
  },
  "findings": [
    {
      "id": "F-001",
      "requirement": "Documented WHS policy signed by an officer",
      "legal_basis": "WHS Act s.27 (officer due diligence)",
      "jurisdiction_note": "Model WHS Act as adopted in QLD; verify current version.",
      "current_state": "absent",            // evidenced | partial | absent
      "evidence_source": null,               // filename/Drive doc that evidences it, or null
      "deficiency": "No WHS policy located in supplied documentation.",
      "gap": "No documented, officer-endorsed WHS policy establishing the safety management system.",
      "risk_rating": "HIGH",                 // EXTREME | HIGH | MEDIUM | LOW
      "risk_rationale": "Foundational duty; absence undermines every downstream control and officer due-diligence defence.",
      "priority": 1,                          // 1 = immediate, ascending
      "remediation_action": "Produce a WHS policy tailored to the business and have an officer endorse it.",
      "remediation_type": "document",         // document | process | training | verify | external
      "builder_document": "STD:WHS_Policy",   // builder identifier, or null
      "tailoring_notes": "14 workers, generic trades, QLD; engages subcontractors.",
      "verification": "confirmed",            // confirmed | unconfirmed
      "source": "https://www.safeworkaustralia.gov.au/... (model WHS Act)",
      "module": "baseline",                   // baseline | hazardous_substances | plant_licensing | asbestos | health_monitoring
      "applicability": "applicable",          // applicable | not-applicable (modules only; baseline is always applicable)
      "applicability_reason": "N/A — only required when the triggering activity is present."
    }
  ],
  "roadmap": [
    {
      "phase": "Immediate — legal exposure",
      "rationale": "Items where absence is a current, direct legal or insurer exposure.",
      "finding_ids": ["F-001", "F-004"]
    },
    {
      "phase": "Near-term — foundational",
      "rationale": "Core system documents and registers that everything else depends on.",
      "finding_ids": ["F-002", "F-007"]
    },
    {
      "phase": "Ongoing — maturity",
      "rationale": "Practices that lift the system from compliant to defensible over time.",
      "finding_ids": ["F-011"]
    }
  ]
}
```

### Field rules

- **`current_state`** drives the report's RAG colouring: `absent` = red, `partial` = amber, `evidenced` = green. A finding for an `evidenced` requirement is still recorded (it's proof of what's working) but carries no remediation.
- **`remediation_type`**: `document` = the builder can produce it (upsell flag); `process` = a workflow the client must adopt; `training` = a competency to obtain; `verify` = confirm with regulator/lawyer (used when `verification: unconfirmed`); `external` = needs a specialist (e.g. an occupational hygienist).
- **`builder_document`**: populate ONLY when `remediation_type` is `document`. Use the builder's identifiers: `STD:<DocName>` for standard-library docs, `SOP:<Equipment>` for equipment SOPs, `PERMIT:<Type>` for permits. Null otherwise.
- **`verification`**: if Phase 1 couldn't confirm a duty currently applies in the client's jurisdiction, set `unconfirmed` and make `remediation_type: "verify"`. Never silently assert an unverified duty.
- **`verified_as_at`** and **`source`**: every finding is honestly dated and sourced. This is what makes the deliverable defensible.

---

## Risk-rating rubric

Rate on consequence × how foundational the gap is, not just severity of the hazard:

| Rating | Meaning |
|--------|---------|
| **EXTREME** | Absence creates immediate, serious legal exposure or a credible path to a fatality/serious-harm event with no compensating control (e.g. no SWMS for high-risk construction work; untrained operation of extreme-risk plant). |
| **HIGH** | Foundational duty unmet, or a significant control gap on commonly-used high-risk plant. Undermines the officer due-diligence defence. |
| **MEDIUM** | Real gap but with partial compensating controls, or a document that exists but is deficient (undated/unsigned/generic). |
| **LOW** | Maturity/best-practice gap; not a current legal exposure. |

## Priority rubric

`priority` is an integer ordering for the roadmap, distinct from risk. A MEDIUM-risk item can be priority 1 if it's a quick foundational fix that unblocks others. Order by: (1) current legal/insurer exposure, (2) foundational dependency, (3) effort-to-close.

---

## Report structure (the client-facing `.docx`)

ALWAYS use this structure:

```
# WHS Compliance Audit — [Client Name]
## Executive Summary
   - Overall posture (one paragraph, honest), headline counts (X absent / Y partial / Z evidenced),
     the single most urgent exposure, and the "as at" date with jurisdiction verified.
## Scope & Method
   - Jurisdiction + state/territory, regulator, what was assessed against (Act + version),
     ingestion modes used, interview coverage, and the verification statement
     (what was confirmed live vs. flagged for regulator confirmation).
## Compliance Snapshot
   - RAG table: requirement | legal basis | current state | risk | priority.
## Detailed Findings
   - One block per finding: the gap, why it matters (in plain language), the legal basis, the risk, and the remediation.
## Remediation Roadmap
   - The phased sequence. Clearly mark which items are documents AUBIT can produce (the upsell),
     which are processes/training the client must own, and which need regulator confirmation.
## Important Notices
   - The "law is moving" caveat for the client's jurisdiction; this is an assessment as at the date shown,
     not legal advice; confirm adopted amendments with the named regulator.
```

The honest "Important Notices" section is not boilerplate to bury — it's what separates a credible assessment from a tick-box. It tells the client exactly what was verified and what they should confirm, which builds the trust that earns the rewrite work.
