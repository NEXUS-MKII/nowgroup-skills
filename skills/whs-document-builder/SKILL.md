---
name: whs-document-builder
description: "Use when a user wants to build, generate, or create WHS (Work Health & Safety) documents, safety policies, safe operating procedures (SOPs), or any suite of workplace documents for a trade or service business, for an Australian or New Zealand client. Triggers: 'create WHS documents', 'build a safety library', 'write SOPs for our tools/equipment', 'generate safety policies', 'make branded WHS documents', 'write a safe work method statement', 'produce the documents from the audit', or any request to produce multiple .docx workplace documents. Also use when a user uploads a logo to brand a document set, uploads an existing policy to expand or replicate, or hands you a findings.json gap register from the whs-compliance-audit skill. This is the back half of the audit-to-build system: when a findings.json is present, produce ONLY the documents it flags, tailored to its notes, so each document traces to a real gap rather than a template guess. Use proactively when someone wants a document library produced."
---

> v2026-08-04.1 · source-of-truth: `nowgroup-skills/skills/whs-document-builder/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.

# WHS Document Builder

Generates complete, branded, Word-compatible (.docx) WH&S document libraries for trade and service businesses, for **AU or NZ** clients. Produces professional multi-document suites using the `docx` npm package, validated against the OOXML standard before delivery.

This is the **back half** of a two-skill system. The front half, `whs-compliance-audit`, produces a `findings.json` gap register. When that file is present, this skill produces only the documents it flags — each traceable to a real, identified gap. Without it, the skill still works greenfield, but prefer audit-first: a document produced against a finding reflects *this* business; a document produced from a template guess is the tick-box trap.

## Quick Reference

| Task | Go to |
|------|-------|
| Consume an audit's findings.json | [Consuming the Gap Register](#consuming-the-gap-register) below |
| Maintain a living library (propagation) | [The Living Library](#the-living-library) below |
| The registry schema (source of truth) | [references/library.md](references/library.md) |
| Build a branded common module | [references/common-module.md](references/common-module.md) |
| Build standard WHS library docs | [references/standard-library.md](references/standard-library.md) |
| Build the two governing docs (Policy, Manual) | [references/standard-library.md](references/standard-library.md) → "The Two Governing Documents" |
| Build individual equipment SOPs | [references/equipment-sops.md](references/equipment-sops.md) |
| Critical technical rules (docx bugs) | [Critical Rules](#critical-rules) below |

---

## Consuming the Gap Register

When the user provides a `findings.json` from `whs-compliance-audit` (or points at one), this drives the whole build:

1. **Read `audit_meta`** — `jurisdiction`, `state_territory`, `industry`, `worker_count`, and `client_name` set the content tailoring and which jurisdiction's terminology/Act references to use throughout the documents. An AU client's docs cite the adopting WHS Act and AS standards; an NZ client's docs cite HSWA 2015, WorkSafe, "beforeUdig", and AS/NZS standards.
2. **Filter to `remediation_type: "document"`** findings. These are the only documents to build. Ignore `process`/`training`/`verify`/`external` findings — they aren't yours to produce.
3. **Resolve `builder_document` identifiers** to builders:
   - `STD:<DocName>` → a standard-library document (see [references/standard-library.md](references/standard-library.md))
   - `SOP:<Equipment>` → an equipment SOP (see [references/equipment-sops.md](references/equipment-sops.md))
   - `PERMIT:<Type>` → a permit (in the standard library)
4. **Apply `tailoring_notes`** from each finding — the real plant list, real site hazards, worker count, subcontractor use, and the client's own terminology. This is what makes the document specific to the business.
5. **Stamp traceability** — each generated document references the finding id it closes (e.g. a footer or document-info line: "Produced to remediate finding F-007"). This lets the client see the audit→document line directly and reinforces the value of the assessment.
6. **Honour the roadmap order** if the user wants phased delivery — build the "Immediate" phase documents first.

If jurisdiction is NZ, read [references/standard-library.md](references/standard-library.md) with the NZ adaptations in mind (HSWA references, worker participation emphasis, notifiable-event 5-year record-keeping); if AU, use the model-WHS adopting Act for the client's state/territory. Match the document content to the jurisdiction the audit verified — never mix AU and NZ legal references in one client's documents.

**On first build, generate `library.json` alongside the documents.** Once the initial library is produced from the audit, write the registry (see [references/library.md](references/library.md)) — it becomes the source of truth for all future maintenance. The audit's `findings.json` seeds it; from then on `library.json` is authoritative.

---

## The Living Library

After the first build, the client's library is **maintained**, not rebuilt from scratch. This is the subscription value: a change is made once at the source and propagates to every affected document. Full schema and rules in [references/library.md](references/library.md). The essentials:

**The one rule:** changes are made to `library.json`, never by hand-editing the Word files. The registry is the single source of truth; documents are generated from it. A hand-edited `.docx` silently diverges from the register — in a compliance system that's worse than no system. The WHSMS Manual states this rule to the client.

**Propagation = regeneration.** When a `data_field` (e.g. the WHS officer's name), a shared `content_block` (e.g. the incident-notification procedure), or a single document's content changes:
1. Find affected documents — field → every doc whose `consumes_fields` lists it; block → every doc in the block's `consumed_by`; document → just that one.
2. Bump versions on the changed element and affected documents.
3. Regenerate **only** the affected documents.
4. **Always** regenerate the WHSMS Manual (it reflects the register).
5. Append a `change_log` entry (date, version, summary, trigger, author, documents_affected).

Because the build is deterministic from `library.json`, propagation is safe and repeatable — nothing of value lives only in a `.docx`, so nothing is lost.

**Central legislative push (Phase 2 / multi-client)** is this same propagation fanned out across all client libraries, but **targeted** (by each client's `targeting` block — jurisdiction, state, trade, size, activity flags) and **human-gated** (AUBIT approves the legal interpretation once before fan-out). The skill layer implements single-client propagation now; the schema is built so the Phase 2 portal can drive the multi-client push without a data-model change. See [references/library.md](references/library.md) → "How central legislative push works."

---

## Process Overview

### Step 1 — Gather requirements

If a `findings.json` is present, most of this is already answered — read it (see above) and only confirm the logo and naming convention. Otherwise establish:

1. **Client name and industry** — content is tailored to the trade.
2. **Jurisdiction** — AU (+ state/territory) or NZ. This sets every legal reference in the documents.
3. **Logo** — ask the user to upload it; extract brand colours (see [references/common-module.md](references/common-module.md)).
4. **Existing policies** — if any, extract text with `extract-text file.docx` to match tone and equipment list.
5. **Document scope** — standard WHS library, tool/equipment SOPs, or both.
6. **Document codes / naming convention** — e.g. `BL-` for "Brazier's Landscaping".

Always extract brand colours before building — the common module is the foundation of every document.

### Step 2 — Build the common module

**Always do this first.** The common module (`common_[client].js`) is a shared Node.js file all builders `require()`. It contains brand colours, `makeHeader`/`makeFooter`, section banners, reusable table builders, typography helpers, numbering/styles/pageProps constants, and the base64 logo. See [references/common-module.md](references/common-module.md). Test it with a minimal document and validate before proceeding.

### Step 3 — Build documents

One Node.js builder script per document type or group, each `require`-ing the common module, writing to `/mnt/user-data/outputs/[PREFIX]_[DocumentName].docx`, logging `✓ filename.docx`. Batch related documents with `Promise.all()`.

- **Standard WHS library** (14 core docs): [references/standard-library.md](references/standard-library.md)
- **Equipment SOPs** (one per tool/machine): [references/equipment-sops.md](references/equipment-sops.md)

Only build what's needed — if driven by `findings.json`, build only the flagged documents and only SOPs for plant actually run.

### Step 4 — Validate all outputs

Run the OOXML validator on every file before presenting:

```bash
for f in /mnt/user-data/outputs/PREFIX_*.docx; do
  result=$(python /mnt/skills/public/docx/scripts/office/validate.py "$f" 2>&1)
  if echo "$result" | grep -q "PASSED"; then echo "✓ $(basename $f)"
  else echo "✗ $(basename $f)"; echo "$result" | grep "error" | head -3; fi
done
```

Fix failures before presenting. The most common is an invalid page-number element in the footer — see [Critical Rules](#critical-rules).

### Step 5 — Present files

Use `present_files` to deliver all outputs, grouped by document type. If driven by an audit, summarise which findings each batch closes.

---

## Critical Rules

Hard-won fixes for bugs that cause Word to refuse to open the file.

### 1. Page numbers — use `SimpleField`, NOT `PageNumberElement`

`PageNumberElement` generates an invalid `<w:pgNum>` tag. Always:
```javascript
const { SimpleField } = require('docx');
new SimpleField({ instruction: "PAGE", cachedValue: "1" }),
new TextRun({ text: " of " }),
new SimpleField({ instruction: "NUMPAGES", cachedValue: "1" })
```

### 2. Table shading — use `ShadingType.CLEAR`, never `ShadingType.SOLID`

`SOLID` with a hex fill produces black backgrounds. Always:
```javascript
shading: { fill: "D6EDD0", type: ShadingType.CLEAR }
```

### 3. Hex colours — 6 digits exactly

docx throws on 8-char hex (colour + opacity). Never concatenate opacity:
```javascript
// BAD: shading: { fill: GREEN + "22", ... }  → Invalid hex value
// GOOD: shading: { fill: "D6EDD0", type: ShadingType.CLEAR }
```

### 4. Tables need dual widths

Set `columnWidths` on the table AND `width` on every cell, or rendering breaks. `columnWidths` must sum to the table width.

### 5. Bullet/numbered lists — use `numbering` config, never unicode characters

```javascript
new Paragraph({ numbering: { reference: "bullets", level: 0 }, children: [new TextRun("Item")] })
```
Never `new Paragraph({ children: [new TextRun("• Item")] })`.

### 6. Always use `WidthType.DXA`, never `WidthType.PERCENTAGE`

Percentages break in Google Docs. A4 content width = `9360` DXA.

---

## Logo Embedding

Extract brand colours with Python/PIL, then embed the logo via base64 `ImageRun`. Get image dimensions before hardcoding the transformation size. Full pattern in [references/common-module.md](references/common-module.md).

---

## Document Naming Conventions

| Pattern | Example |
|---------|---------|
| `[PREFIX]_[DocumentName].docx` | `BL_SWMS_Template.docx` |
| `[PREFIX]_SOP_[EquipmentName].docx` | `BL_SOP_Chainsaw.docx` |
| `[PREFIX]_Tool_Policy_[Group].docx` | `BL_Tool_Policy_Power_Tools.docx` |

Document codes (in headers/footers): `[PREFIX]-[CATEGORY]-[SEQ]`, e.g. `BL-SOP-CS-001`.

---

## Content Customisation by Jurisdiction and Industry

Tailor substance, not just the logo. **First by jurisdiction** (this is non-negotiable — wrong-jurisdiction legal references make a document worse than useless):

- **AU** — cite the adopting WHS Act for the client's state/territory, the relevant state regulator, AS/AS/NZS standards, "Dial Before You Dig", AU licensing (HRWL, state electrical/plumbing licences, ChemCert).
- **NZ** — cite HSWA 2015, WorkSafe NZ, AS/NZS standards, "beforeUdig", NZ registration regimes (EWRB, PGDB), and emphasise worker engagement/participation and the 5-year notifiable-event record.

**Then by trade:**

- **Landscaping** — chainsaw chaps, sun/UV protection, chemical-resistant gloves, snake-bite/heat protocols, locate-service callout for ground-penetrating work, ChemCert/AHCMOM213 chainsaw competency.
- **Electrical** — state/EWRB licence, AS/NZS 3000 competency, arc flash, insulated tools, live-work permits.
- **Plumbing/Gas** — state/PGDB licence, hot-work permit for soldering/brazing, confined space for drainage/pit work.

The TMT (generic trades) library is the universal baseline; client-specific libraries adapt from it.

---

## Reference Files

- **[references/library.md](references/library.md)** — The `library.json` registry: the living source of truth, the propagation rules, and the central-legislative-push design. Read this for any maintenance or second-build work.
- **[references/common-module.md](references/common-module.md)** — Full common module: brand colour extraction, all helper functions, `makeHeader`/`makeFooter`.
- **[references/standard-library.md](references/standard-library.md)** — The 21 standard WHS documents: the working 14, the two governing documents (WHS Policy, WHSMS Manual), and the five risk-module documents (hazardous substances, plant, asbestos ×2, health monitoring).
- **[references/equipment-sops.md](references/equipment-sops.md)** — The 9-section equipment SOP pattern and the `doc()` function, with examples for 22 tools.
