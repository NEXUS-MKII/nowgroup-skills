# Risk-Specific Obligation Modules

The 15-item baseline checklist (see au.md / nz.md) audits whether the client has the right *documents*. These modules audit whether the client is meeting their *risk-specific legal obligations* — a different and deeper question, and the one a regulator or insurer probe actually lands on.

## How these modules work

**Flat and always-visible, not hidden-conditional.** Every module appears in every audit. For each, the audit makes an explicit **applicability determination**: `applicable` or `not-applicable`, *with a stated reason*. An auditor who silently omits asbestos looks identical to one who forgot it; "N/A — client confirmed they do not disturb materials in pre-2004 structures" is a visible, defensible decision that shows the obligation was considered. Considered-and-excluded beats silently-omitted, every time.

**Applicability is decided from the Phase 0/Phase 3 profile and any `activity_flags`.** When a module is `applicable`, the audit assesses against the real duty (not just "is there a document") and produces findings in the normal `findings.json` schema, setting the relevant `activity_flag` in `targeting` so the central legislative-push can later target this cohort.

**Each module is jurisdiction-aware** — AU and NZ branches, same decision-tree approach as the baseline. Always run Phase 1 live verification before asserting a duty; both countries are mid-reform.

**Module finding structure** (extends the standard finding — see gap-register.md):
- `module` — which module the finding belongs to (e.g. `hazardous_substances`)
- `applicability` — `applicable` | `not-applicable`
- `applicability_reason` — why (required for both values)
- plus the standard finding fields (requirement, legal_basis, current_state, gap, risk, priority, remediation, builder_document, etc.)

---

## Module 1 — Hazardous Substances / Chemicals  *(activity_flag: `uses_hazardous_substances`)*

**This is the pattern-setter module — the other three follow this exact structure.**

### Applicability test
`applicable` if the client stores, handles, uses, or generates hazardous chemicals above trivial domestic quantities. For trades this is almost always true — fuels, solvents, adhesives, paints, gases (oxy/LPG/acetylene), herbicides/pesticides (landscaping), cleaning chemicals, dusts. `not-applicable` only for genuinely chemical-free operations (rare). When in doubt, `applicable` — the cost of a register for a client who barely needs one is low; the cost of missing a chemical duty is high.

### What to assess (the real duties, not just "is there a doc")

| # | Obligation | Assess |
|---|-----------|--------|
| 1 | **Chemical/hazardous-substances register** | A current list of every hazardous chemical on site. Evidenced = exists + current + matches what's actually stored. |
| 2 | **Safety Data Sheets (SDS)** | An SDS for every chemical in the register, < 5 years old, accessible to workers. |
| 3 | **Risk assessment for hazardous chemicals** | Documented assessment of exposure risks + controls (ventilation, PPE, handling). |
| 4 | **Storage & segregation** | Incompatible chemicals separated; flammables stored correctly; quantities below manifest/placarding thresholds, or placarded if above. |
| 5 | **Labelling** | Containers (incl. decanted) correctly labelled per GHS. |
| 6 | **Spill response & PPE** | Spill kit + procedure; PPE matched to the SDS requirements. |
| 7 | **Health monitoring trigger** | Any chemical requiring health monitoring (e.g. isocyanates, lead, certain solvents) → cross-links to Module 4. |

### Jurisdiction branches (VERIFY current in Phase 1)
- **AU** — WHS Regulations Part 7.1 (hazardous chemicals); GHS classification/labelling; manifest & placarding quantity thresholds; the register + SDS duties. Poisons line 13 11 26. Verify the client's state hasn't varied thresholds.
- **NZ** — Health and Safety at Work (Hazardous Substances) Regulations 2017; the hazardous substances inventory; SDS; location/test certificates and approved handlers for certain substances above thresholds; signage. WorkSafe / EPA hazardous-substances controls.

### Document mapping (builder)
- **NEW** `STD:Hazardous_Substances_Register` — the chemical register + SDS index + per-chemical risk/control summary. (To be specced in standard-library.md next build phase.)
- Existing: feeds PPE Register (#6), Risk Assessment Form (#2), Emergency Response Plan (spill response).

---

## Module 2 — Plant Registration & High-Risk-Work Licensing  *(activity_flag: `operates_registrable_plant`)*

### Applicability test
`applicable` if the client operates plant that is either (a) **registrable** with the regulator, or (b) requires a **licensed/competent operator**. Triggers: cranes, hoists, pressure equipment, certain elevating work platforms (EWPs), forklifts, concrete pumps, scaffolding >4m, excavators in some uses. `not-applicable` for clients running only hand tools and non-registrable equipment — but note the baseline still requires SOPs for whatever they do run.

### What to assess
The baseline checklist asks "is there an SOP." This module asks the *legal* questions the SOP doesn't cover:

| # | Obligation | Assess |
|---|-----------|--------|
| 1 | **Plant registration** | Is registrable plant (or its design) actually registered with the regulator? Registration current? |
| 2 | **Operator licensing/competency** | Does each operator of high-risk plant hold the correct High Risk Work Licence (AU) / certificate of competence (NZ)? Current, sighted, recorded? |
| 3 | **Plant risk assessment** | Documented risk assessment per item of plant. |
| 4 | **Inspection & maintenance** | Logged pre-start + scheduled maintenance (links to SOP maintenance schedule). |
| 5 | **Guarding & isolation** | Guards fitted; lockout/tagout for maintenance. |

### Jurisdiction branches (VERIFY)
- **AU** — WHS Regulations Chapter 5 (plant); registrable plant & plant designs list; **High Risk Work Licences (HRWL)** — forklift, crane, EWP (boom ≥11m), rigging/dogging/scaffolding classes. State-issued, nationally recognised.
- **NZ** — HSWA + plant regulations; certificates of competence; WorkSafe registrable plant categories (e.g. cranes, pressure equipment). Note NZ's regime differs in detail — verify per item.

### Document mapping (builder)
- **NEW** `STD:Plant_Equipment_Register` — every item of plant with registration status, operator-licence linkage, inspection cycle.
- Existing: SOPs (`SOP:<Equipment>`), Training & Competency Register (licences).

---

## Module 3 — Asbestos  *(activity_flag: `disturbs_asbestos_risk_materials`)*

### Applicability test
`applicable` if the client works on, renovates, demolishes, or maintains **structures built before asbestos bans** (AU: pre-2004 ban on all forms; NZ: imports banned 2016 but in-situ asbestos in older buildings is the risk). Trades that trigger: builders, electricians, plumbers, roofers, demolition, renovators. `not-applicable` for new-build-only or genuinely no-disturbance work — stated with reason.

### What to assess

| # | Obligation | Assess |
|---|-----------|--------|
| 1 | **Asbestos awareness** | Do workers have asbestos-awareness training before disturbing suspect materials? |
| 2 | **Identification / presumption** | Process to identify or presume asbestos before work on pre-ban structures (incl. reviewing the building's asbestos register where one should exist). |
| 3 | **Asbestos register & management plan** | For workplaces they *control* containing asbestos — a register + management plan. (Often the client is a contractor entering others' sites — assess the duty to obtain/consult the site register.) |
| 4 | **Licensed removal** | Any removal beyond the small non-friable threshold requires a licensed removalist (Class A friable / Class B non-friable). Assess they're not doing unlicensed removal. |
| 5 | **Air monitoring & clearance** | For removal work — monitoring + clearance certificate before reoccupation. |

### Jurisdiction branches (VERIFY)
- **AU** — WHS Regulations Chapter 8 (asbestos); Class A/B removal licensing; asbestos register duty for workplaces with ACMs; pre-2004 presumption.
- **NZ** — Health and Safety at Work (Asbestos) Regulations 2016; Class A/B licensing; WorkSafe notification for licensed removal; management plan duty.

### Document mapping (builder)
- **NEW** `STD:Asbestos_Register` and **NEW** `STD:Asbestos_Management_Plan` (for clients who control ACM-containing workplaces).
- Existing: Training Register (awareness), SWMS (removal/disturbance work).

---

## Module 4 — Health Monitoring  *(activity_flag: `requires_health_monitoring`)*

### Applicability test
`applicable` if workers are exposed to hazards with a **legal health-monitoring duty**: hazardous chemicals requiring monitoring (lead, isocyanates, certain solvents), **respirable crystalline silica** (concrete/stone/excavation — high regulatory focus right now), asbestos, **hazardous noise** (the NSW audiometric change, Jan 2026), welding fume. Cross-links from Modules 1 and 3. `not-applicable` only if no monitored exposure exists.

### What to assess

| # | Obligation | Assess |
|---|-----------|--------|
| 1 | **Exposure identification** | Has the client identified which workers have monitored exposures? |
| 2 | **Health monitoring program** | Baseline + periodic monitoring by an appropriate practitioner for those workers. |
| 3 | **Records** | Health monitoring records kept, confidential, retained for the required period. |
| 4 | **Atmospheric/exposure monitoring** | Where required (silica, noise) — workplace exposure-standard monitoring, not just health monitoring. |
| 5 | **Action on results** | Process to act on adverse results (control review, medical follow-up). |

### Jurisdiction branches (VERIFY — this is the fastest-moving area)
- **AU** — WHS Regulations health-monitoring provisions; the silica reforms (engineered-stone prohibition + silica processes); NSW audiometric testing (Clause 58) commenced 1 Jan 2026 — verify which states have equivalent noise duties.
- **NZ** — HSWA health-monitoring duties; WorkSafe exposure standards (WES); silica and noise focus. Verify current.

### Document mapping (builder)
- **NEW** `STD:Health_Monitoring_Register` — monitored workers, exposure type, monitoring schedule, records pointer (results themselves stay confidential/out of the library).
- Existing: links to Hazardous Substances Register (M1) and PPE Register.

---

## Summary — new builder documents these modules require

To be specced in standard-library.md in the documents build phase:

| Module | New builder document(s) |
|--------|------------------------|
| Hazardous substances | `STD:Hazardous_Substances_Register` |
| Plant & licensing | `STD:Plant_Equipment_Register` |
| Asbestos | `STD:Asbestos_Register`, `STD:Asbestos_Management_Plan` |
| Health monitoring | `STD:Health_Monitoring_Register` |

Each becomes a `document` entry in `library.json` like any other — versioned, owned, reviewed, and regenerated on change — so the moment a module's document exists it inherits the living-library propagation and central-push wiring for free.

## Activity flags these modules set (for central-push targeting)

`uses_hazardous_substances` · `operates_registrable_plant` · `disturbs_asbestos_risk_materials` · `requires_health_monitoring`

These populate `targeting.activity_flags` in library.json, so a legislative push (e.g. a silica-rule change) can target exactly the clients it affects.
