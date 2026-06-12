# Standard WHS Library — 21 Core Documents

These documents form the foundation WHS library for any trade or service business. Each adapts to the client's industry by changing the content (not just the branding).

The library has three layers:
- **The working 14** (#1–14) — the everyday WHS documents.
- **The two governing documents** (#15–16) — WHS Policy (signed commitment) and WHSMS Manual (living system map). See [The Two Governing Documents](#the-two-governing-documents).
- **The five risk-module documents** (#17–21) — produced only when the audit's risk-specific modules fire `applicable`. See [Risk-Module Documents](#risk-module-documents-1721).

All documents follow the same pattern:
```javascript
const C = require('./common_client');

const doc = new C.Document({
  numbering: C.numbering, styles: C.styles,
  sections: [{
    properties: C.pageProps,
    headers: { default: C.makeHeader("Document Title", "PREFIX-CODE-001") },
    footers: { default: C.makeFooter("1.0 | [Month Year]") },
    children: [ /* content */ ]
  }]
});

C.Packer.toBuffer(doc).then(buf => {
  C.fs.writeFileSync('/mnt/user-data/outputs/PREFIX_DocumentName.docx', buf);
  console.log('✓ DocumentName.docx');
});
```

---

## The 14 Documents

### 1. SWMS Template (`PREFIX_SWMS_Template.docx`)

Six-part structure:
- **Part A** — Job Details (2-row 4-column info table: job name, number, site, date, contractor, supervisor, licence, emergency contact)
- **Part B** — High Risk Work Activities (2-column tick-box table: 10–12 standard high-risk categories)
- **Part C** — Plant, Equipment & PPE (3-column table: plant/equipment | PPE required | certification status, 4 empty rows)
- **Part D** — Step-by-Step Risk Assessment (5-column table: work activity | hazard | controls | initial risk | residual risk, 5 empty rows; include risk rating key above table)
- **Part E** — Emergency Procedures (bullet list: 000, notify supervisor, nearest hospital, first aid kit location, assembly point)
- **Part F** — Worker Acknowledgement (4-column sign-off table: name | company | signature | date, 8 rows)

### 2. Risk Assessment Form (`PREFIX_Risk_Assessment_Form.docx`)

Four sections:
- **Section 1** — Assessment details (2×4 info grid)
- **Section 2** — Risk Matrix (5×7 colour-coded table: Consequence rows × Likelihood columns; colours: Extreme=red, High=orange, Medium=yellow, Low=dark green, Very Low=mid-green)
- **Section 3** — Hazard table (8-column: # | hazard | who at risk | initial risk | controls | residual risk | responsible | due date, 8 rows)
- **Section 4** — Hierarchy of controls (numbered list: elimination → substitution → isolation → engineering → admin → PPE)
- **Section 5** — Sign-off (3 signatories: assessor, supervisor, WHS rep)

### 3. Emergency Response Plan (`PREFIX_Emergency_Response_Plan.docx`)

Sections:
- Site info (blueBox rows: business name, address, coordinator, deputy, date)
- Emergency contacts table (service | number | notes: 000, Poisons 13 11 26, state regulator, first aider, nearest hospital)
- Sections 1–7: Purpose; RACE procedure (numbered); Fire procedure (numbered, 8 steps); Medical procedure (numbered, 7 steps); Chemical spill (numbered, 6 steps); Evacuation map placeholder; Post-emergency actions (bullets)
- Sign-off (3: emergency coordinator, PCBU, WHS rep)

### 4. First Aid Needs Assessment (`PREFIX_First_Aid_Needs_Assessment.docx`)

Sections:
- Workplace details (blueBox rows: business name, address, worker count, work type, assessor, date)
- **Risk profile table** (4 columns: risk factor | yes | no | comments; 8–10 rows tailored to the trade)
- **Resources required table** (4 columns: requirement | minimum | actual | location; rows for first aider, kit, AED, eyewash, snake kit if outdoor work)
- First aider details (4-column table: name | qualification | expiry | contact, 4 rows)
- Sign-off

### 5. WHS Worker Induction Checklist (`PREFIX_WHS_Worker_Induction_Checklist.docx`)

- Worker details (2×4 grid)
- Checklist table (4 columns: item | covered by | done ✓ | initials)
- **Section headers embedded as full-width coloured rows** within the checklist table (use `columnSpan: 4`):
  - A. WHS Policies & Legal Obligations (~5 items)
  - B. Site Rules & General Safety (~6 items)
  - C. Emergency Procedures (~5 items)
  - D. Hazard & Incident Reporting (~3 items)
  - E. PPE Requirements (~4 items)
  - F. Plant, Equipment & Tools (~3 items)
  - G. Chemical & Environmental Safety (~3 items)
  - H. Health & Wellbeing (~3–4 items, tailored to trade — outdoor/UV for landscaping)
- Worker declaration paragraph + 2-row sign-off table
- Licences sighted table (4 columns: licence | issuing body | expiry | sighted by, 5 rows)

### 6. PPE Register (`PREFIX_PPE_Register.docx`)

Two sections:
- **Issue Register** (8-column table: # | worker name | item | size/spec | qty | date issued | issued by | worker sign, 20 rows)
- **Mandatory PPE by Work Type** (2-column table: work type | minimum PPE required; 6–8 rows tailored to the trade's specific tasks)

### 7. Training & Competency Register (`PREFIX_Training_Competency_Register.docx`)

Three sections:
- **Requirements by Role** (3-column: role | minimum training required | renewal frequency; rows tailored to trade licences — e.g. electrical licence, plumbing licence, ChemCert for landscaping)
- **Worker Training Record** (7-column: # | name | training | certificate no. | date completed | expiry | sighted by, 20 rows)
- **Upcoming Expiries** (5-column: name | training | expiry | action required | actioned by/date, 6 rows)

### 8. Toolbox Talk Record (`PREFIX_Toolbox_Talk_Record.docx`)

- Meeting details (blueBox rows: site, date, time, location, facilitator, topic)
- Discussion summary (lined writing area — 6 blank paragraphs with bottom border)
- Safety issues raised (3-column: issue | action | assigned/due, 5 rows)
- Worker attendance table (5-column: # | name | company | signature | date, 20 rows)

### 9. Subcontractor Management Policy (`PREFIX_Subcontractor_Management_Policy.docx`)

Policy structure (metaTable + sections):
1. Purpose
2. Pre-Engagement Requirements (bullets: insurance, workers comp, licences, WHS system, SWMS, signed rules)
3. Site Induction
4. Ongoing Monitoring
5. Non-Compliance
6. Pre-Engagement Checklist (tick-box table: requirement | yes | N/A | comments, 6–8 rows)
7. Sign-off

### 10. Fatigue Management Policy (`PREFIX_Fatigue_Management_Policy.docx`)

Policy structure:
1. Purpose
2. Scope
3. Responsibilities (heading2 for employer, heading2 for workers)
4. Maximum Hours Guidelines (2-column table: guideline | max/requirement; 5–6 rows)
5. Signs of Fatigue (bullets)
6. Reporting Fatigue
7. Sign-off

**Industry adaptation:** For outdoor trades, add a heat/physical fatigue section with mandatory shade breaks, hydration requirements, and extreme heat day protocols.

### 11. Manual Handling Policy (`PREFIX_Manual_Handling_Policy.docx`)

Policy structure:
1. Purpose
2. Scope
3. Hazardous Manual Task identification (bullet list of indicators)
4. **Industry-specific common tasks** — e.g. for landscaping: lifting pavers, rolling turf, moving soil bags; for electrical: cable drums, switchboard components
5. Hierarchy of Controls (numbered list)
6. Safe Lifting Technique (numbered list, 8 steps)
7. Team Lifting
8. Reporting
9. Sign-off

### 12. Working at Heights Policy (`PREFIX_Working_at_Heights_Policy.docx`)

Policy structure:
1. Purpose
2. Scope
3. SWMS Requirement (all work >2m requires SWMS)
4. Hierarchy of Controls (numbered: elimination → passive → positioning → fall arrest → admin → PPE)
5. **Specific requirements** (heading2 subsections): Ladders | Scaffolding | Fall Arrest | Roofwork (or trade-specific variant — e.g. tree work for landscaping)
6. Exclusion Zones
7. Emergency Procedures for Falls
8. Sign-off

### 13. Confined Space Entry Permit (`PREFIX_Confined_Space_Entry_Permit.docx`)

Red warning banner at top, then:
- **Section 1** — Permit details (blueBox rows: number, site, location, description, date, valid from/until, supervisor)
- **Section 2** — Personnel (5-column: name | role | licence | contact | signature, 5 rows)
- **Section 3** — Atmospheric Testing (6-column: parameter | acceptable range | pre-entry | during | post | pass/fail; rows for O2, CO, H2S, LEL, CO2)
- **Section 4** — Hazard identification & controls (2-column, 6–8 rows)
- **Section 5** — Equipment & PPE checklist (3-column: item | ready ✓ | notes, 8–9 rows)
- **Section 6** — Rescue plan (blank lined area + rescue person field)
- **Section 7** — Permit authorisation sign-off (3: issuer, entrant, standby)
- Permit cancellation section (blueBox: cancelled by, time, reason, all accounted for)

### 14. Hot Work Permit (`PREFIX_Hot_Work_Permit.docx`)

Orange warning banner at top, then:
- **Section 1** — Permit & job details (blueBox rows: number, site, location, date, valid from/until (max 24h), work type, description)
- **Section 2** — Applicant details (blueBox rows: name, trade, company, contact, WHS officer)
- **Section 3** — Fire risk assessment (4-column checklist: condition | yes | no | action; 12–14 rows covering flammables cleared, fire extinguisher present, fire watch assigned, drains sealed, etc.)
- **Section 4** — PPE & equipment checklist (3-column: item | ready ✓ | notes, 8–9 rows)
- **Section 5** — Additional controls (blank lined area)
- **Section 6** — Permit authorisation sign-off (3: operator, issuer, fire watch)
- **Section 7** — Work completion (blueBox: completed at, fire watch completed, inspected by, final status)

---

## Batch Generation Pattern

For efficiency, group related documents in a single builder script:

```javascript
// e.g. whs_registers.js — generates docs 6, 7, 8 together
const docs = [
  [ppeDoc, "PREFIX_PPE_Register.docx"],
  [trainingDoc, "PREFIX_Training_Competency_Register.docx"],
  [toolboxDoc, "PREFIX_Toolbox_Talk_Record.docx"],
];

Promise.all(docs.map(([doc, name]) =>
  C.Packer.toBuffer(doc).then(buf => {
    C.fs.writeFileSync(`/mnt/user-data/outputs/${name}`, buf);
    console.log(`✓ ${name}`);
  })
));
```

Group all 4 policy documents (9–12) in one script, and the 2 permits (13–14) in another.

---

## The Two Governing Documents

These two sit above the working 14. Do not merge them — they do different jobs, and merging weakens both. The WHS Policy is a short signed declaration; the WHSMS Manual is the living system map.

### 15. WHS Policy (`PREFIX_WHS_Policy.docx`) — `STD:WHS_Policy`

The foundational *commitment* document. Short (1–2 pages), endorsed by the most senior officer. This is what a regulator looks for first and what evidences officer due-diligence intent. It is **not** a navigation document — keep it clean and declarative.

Structure:
- **Header banner** — "Work Health & Safety Policy"
- **Commitment statement** — 1 short paragraph: the business's commitment to providing a safe and healthy workplace for workers, contractors, and others, so far as is reasonably practicable.
- **Our principles** (bullet list, 5–7 items) — e.g. hazards identified and controlled via the hierarchy of controls; workers consulted and encouraged to participate; compliance with the [jurisdiction Act] and supporting regulations; adequate resources, training, and supervision; continual improvement.
- **Responsibilities** (heading2 blocks) — Officers/PCBU (due diligence), Managers/Supervisors, Workers. 2–4 bullets each.
- **Legal basis line** — cites the jurisdiction-correct Act (shared block `legal_basis_primary` — see library.md). AU: the adopting WHS Act for the client's state. NZ: HSWA 2015.
- **Review statement** — "This policy will be reviewed at least [annually] or when legislation changes."
- **Signature block** — name, position (most senior officer), signature line, date. Single signatory — this is a leadership commitment, not a worker sign-off sheet.

Keep it to one page where possible. The power of this document is its brevity and the seniority of the signature.

### 16. WHSMS Manual (`PREFIX_WHSMS_Manual.docx`) — `STD:WHSMS_Manual`

The **living** umbrella document — the navigation, oversight, and system-map layer that converts "a folder of policies" into "a safety management system." This is the capstone deliverable and the printable snapshot of the portal landing page (Phase 2). It is **assembled from `library.json`**, not hand-written — see [library.md](library.md) for the registry schema it reads.

This document is regenerated whenever the library changes, so it is always current. It must never be hand-edited (see the source-of-truth rule in library.md).

Structure (assembled from the registry):
- **Header banner** — "WHS Management System Manual" + client name + "Version X.X — generated [date]"
- **Section 1 — Purpose & Scope** — what this system covers, who it applies to, the jurisdiction it's built for (from `audit_meta`).
- **Section 2 — How This System Works** — short explainer: the manual is the map; every document has an owner, a version, and a review cycle; how to request a change (route through the system, never edit the Word files directly).
- **Section 3 — Document Register** (the core table, generated from `library.json` → `documents[]`):
  | Document | Code | Version | Status | Owner/Approver | Last Reviewed | Next Review Due |
  Each row links (hyperlink when delivered digitally) to the document. `Status` uses RAG: current / due-for-review / superseded.
- **Section 4 — How the Documents Relate** — a short narrative or simple map: the WHS Policy sits at the top; risk-specific policies and SOPs sit under it; registers and permits are operational. Generated from document `category` fields.
- **Section 5 — Change Log** (generated from `library.json` → `change_log[]`):
  | Date | Version | What Changed | Reason / Trigger | Author | Documents Affected |
  Legislative-push changes are stamped with the legal trigger and "AUBIT central update."
- **Section 6 — Review Cycle & Responsibilities** — the schedule of upcoming reviews (generated from each document's `next_review`), and who owns the system.
- **Section 7 — Legislative Currency Statement** — the jurisdiction, the Act/version assessed against, the "as at" date, and the standing note that AUBIT maintains legislative currency via central updates (the subscription value, stated plainly).

Because Sections 3, 5, and 6 are generated directly from the registry, the manual is deterministic: rebuild it and it reflects the exact current state of the library. This is what makes it a living document rather than a snapshot that goes stale.

#### Building the WHSMS Manual from the registry

```javascript
const C = require('./common_client');
const lib = JSON.parse(C.fs.readFileSync('/mnt/user-data/outputs/library.json', 'utf8'));

// Section 3 — Document Register table rows
const registerRows = lib.documents.map(d => [
  d.title, d.code, d.version, d.status, d.owner, d.last_reviewed, d.next_review
]);

// Section 5 — Change Log table rows (most recent first)
const changeRows = lib.change_log
  .slice().sort((a,b) => b.date.localeCompare(a.date))
  .map(c => [c.date, c.version, c.summary, c.trigger, c.author, c.documents_affected.join(', ')]);

// ...assemble document with C.sectionBanner / table helpers, then Packer.toBuffer as usual.
```

---

## Risk-Module Documents (17–21)

These five documents are produced when the audit's risk-specific modules (see `whs-compliance-audit/references/risk-modules.md`) fire as `applicable`. Each is a register-style document following the same table conventions as the 14 working docs, becomes a normal `document` entry in `library.json` (versioned, owned, reviewed, regenerated on change), and inherits living-library propagation and central-push targeting via its `activity_flag`.

Each carries a jurisdiction-correct legal-basis line from the shared `legal_basis_primary` content block (AU adopting WHS Regs / NZ HSWA regs) — never hard-code the Act.

### 17. Hazardous Substances Register (`PREFIX_Hazardous_Substances_Register.docx`) — `STD:Hazardous_Substances_Register`

The chemical register + SDS index + per-chemical control summary. Triggered by `uses_hazardous_substances`.

Structure:
- **Header banner** — "Hazardous Substances Register" + legal-basis line (AU: WHS Regs Part 7.1 + GHS; NZ: HSW (Hazardous Substances) Regs 2017).
- **Section 1 — Register details** (blueBox: business, site/location, person responsible, date compiled, review due).
- **Section 2 — Chemical Register** (main table, 8-column): # | Product name | Manufacturer/Supplier | Hazard class (GHS) | Location stored | Max quantity | SDS held (Y/N) | SDS date. 15–20 rows.
- **Section 3 — SDS Index & currency** (4-column: product | SDS version/date | <5yrs (Y/N) | location/link). Flags SDS over 5 years for renewal.
- **Section 4 — Storage & segregation summary** (3-column: incompatibility group | chemicals | separation control). Note manifest/placarding threshold status.
- **Section 5 — Risk controls & PPE** (3-column: chemical/group | exposure risk | controls + PPE per SDS). Cross-reference PPE Register.
- **Section 6 — Spill response** — short procedure + spill-kit location (links Emergency Response Plan).
- **Section 7 — Health-monitoring trigger flag** — list any chemical requiring health monitoring → cross-links Health Monitoring Register (#21).
- **Sign-off** — compiled by + reviewed by.

### 18. Plant & Equipment Register (`PREFIX_Plant_Equipment_Register.docx`) — `STD:Plant_Equipment_Register`

Every item of plant with registration status and operator-licence linkage. Triggered by `operates_registrable_plant`. Distinct from SOPs: SOPs say *how to operate safely*; this register tracks the *legal* registration/licensing duties.

Structure:
- **Header banner** — "Plant & Equipment Register" + legal-basis line (AU: WHS Regs Ch.5 + HRWL; NZ: HSWA plant regs + certificates of competence).
- **Section 1 — Register details** (blueBox: business, person responsible, date, review due).
- **Section 2 — Plant Register** (main table, 8-column): # | Item/plant | Make/model/serial | Registrable? (Y/N) | Registration no. + expiry | Licensed operator required? | SOP ref | Last inspection. 15 rows.
- **Section 3 — Operator licensing** (5-column: operator name | plant/class | licence type (HRWL/CoC) | licence no. | expiry). Cross-references Training & Competency Register.
- **Section 4 — Inspection & maintenance schedule** (3-column: item | frequency | responsible) — or pointer to each SOP's maintenance schedule.
- **Section 5 — Guarding & isolation** — confirmation that guards fitted and lockout/tagout applies to maintenance.
- **Sign-off**.

### 19. Asbestos Register (`PREFIX_Asbestos_Register.docx`) — `STD:Asbestos_Register`

For workplaces the client *controls* that contain (or are presumed to contain) asbestos. Triggered by `disturbs_asbestos_risk_materials`. Red warning banner at top.

Structure:
- **Warning banner** (red) — asbestos disturbance is a serious health hazard; do not disturb suspected ACM without assessment.
- **Header banner** — "Asbestos Register" + legal-basis line (AU: WHS Regs Ch.8; NZ: HSW (Asbestos) Regs 2016).
- **Section 1 — Workplace details** (blueBox: site, building age/era, controlling PCBU, competent person who identified, date, review due).
- **Section 2 — Identified / presumed ACM** (main table, 7-column): # | Location | Material/description | Asbestos type (or "presumed") | Condition | Risk (accessible/friable?) | Control/label in place. 12 rows.
- **Section 3 — Sampling/assessment** (4-column: sample ref | location | result | assessor) — or note where presumption applied in lieu of sampling.
- **Section 4 — Management actions** (3-column: item | action required | by whom/when).
- **Section 5 — Worker awareness** — confirm asbestos-awareness training (links Training Register); instruction not to disturb.
- **Sign-off** — competent person + PCBU.

### 20. Asbestos Management Plan (`PREFIX_Asbestos_Management_Plan.docx`) — `STD:Asbestos_Management_Plan`

The management-and-control plan that accompanies the register for controlled workplaces with ACM. Triggered by `disturbs_asbestos_risk_materials` where the client controls an ACM-containing workplace.

Structure (policy-style: metaTable + numbered sections):
1. Purpose & scope (which workplace/structure this covers; references the Asbestos Register #19).
2. Roles & responsibilities (PCBU, competent person, workers).
3. Identification & presumption process.
4. Control measures (labelling, condition monitoring, no-disturbance rules, access control).
5. Safe work requirements for any disturbance (SWMS trigger; licensed-removal threshold — Class A friable / Class B non-friable).
6. Air monitoring & clearance (for removal work; clearance certificate before reoccupation).
7. Incident/uncontrolled-disturbance response.
8. Review (and the legal trigger to review — new work, condition change, removal).
9. Sign-off.

### 21. Health Monitoring Register (`PREFIX_Health_Monitoring_Register.docx`) — `STD:Health_Monitoring_Register`

Tracks which workers have monitored exposures and the monitoring schedule. Triggered by `requires_health_monitoring`. **Results themselves are confidential medical records and stay OUT of this register** — the register points to where they are held, it does not contain them.

Structure:
- **Header banner** — "Health Monitoring Register" + legal-basis line (AU: WHS Regs health-monitoring provisions; NZ: HSWA + WorkSafe exposure standards).
- **Privacy notice** (blueBox) — monitoring results are confidential health records held separately; this register records *that* monitoring occurs, not the results.
- **Section 1 — Register details** (blueBox: business, person responsible, date, review due).
- **Section 2 — Monitored exposures** (4-column: hazard (e.g. silica, lead, isocyanates, noise, asbestos) | workers/roles exposed | monitoring type required | frequency).
- **Section 3 — Worker monitoring schedule** (6-column): # | worker name | exposure type | baseline date | next due | records held by/where. 15 rows.
- **Section 4 — Atmospheric/exposure monitoring** (where required, e.g. silica/noise) (4-column: hazard | area/task | last monitored | result vs exposure standard).
- **Section 5 — Action on adverse results** — process: control review + medical follow-up + notification where required.
- **Sign-off**.

---

## Risk-Module Document Summary

| # | Document | Identifier | Activity flag |
|---|----------|-----------|---------------|
| 17 | Hazardous Substances Register | `STD:Hazardous_Substances_Register` | `uses_hazardous_substances` |
| 18 | Plant & Equipment Register | `STD:Plant_Equipment_Register` | `operates_registrable_plant` |
| 19 | Asbestos Register | `STD:Asbestos_Register` | `disturbs_asbestos_risk_materials` |
| 20 | Asbestos Management Plan | `STD:Asbestos_Management_Plan` | `disturbs_asbestos_risk_materials` |
| 21 | Health Monitoring Register | `STD:Health_Monitoring_Register` | `requires_health_monitoring` |

All five follow the established table-helper conventions, carry the jurisdiction-correct legal-basis block, and only build when their module fires `applicable`.
