# Evidence Ingestion — Drive Folders, Mass Upload, and Gap Mapping

The audit assesses what the client *already has* before interviewing them. This file covers the three ingestion modes and how to turn raw documents into a provisional gap map. **Always ingest before you interview** — asking a question the client's own document already answers is the fastest way to lose credibility.

---

## Mode A — Google Drive folder

The client points you at one or more Drive folders containing their safety documentation. The Google Drive connector is available; the tools are deferred, so load them with `tool_search` before use (e.g. `tool_search(query="google drive search files")`), then use the exact parameter names returned — don't guess them.

Pattern:
1. **Search/list** the folder's contents. Use `Google Drive:search_files` with a query scoped to the folder, or `Google Drive:list_recent_files`. Capture each file's id, name, and type.
2. **Read** each candidate document with `Google Drive:read_file_content` (natural-language representation) rather than downloading raw bytes — you want the text to classify it, not the binary.
3. **Classify** each document against the requirement checklist (see jurisdiction reference) — see [Classification](#classification) below.

If a folder is large, prioritise documents whose names suggest WHS content (policy, SWMS, SOP, induction, register, emergency, permit, risk) before reading everything. Note what you skipped so the client knows the scan wasn't exhaustive.

---

## Mode B — Mass upload / batch

The client uploads a batch of files (or a zip). Files land under `/mnt/user-data/uploads/`. Consult the `file-reading` skill router for how to read each type (docx/pdf/xlsx/etc.) correctly rather than blindly `cat`-ing binaries. For `.docx`, the `extract-text` helper or the docx skill's read path works; for PDF use the pdf-reading skill.

Pattern is the same as Mode A from step 3: read, then classify.

---

## Mode C — Greenfield

No existing documentation. Skip ingestion; every requirement starts `absent`. Go straight to interview, but keep it efficient — a greenfield client needs the *roadmap* more than a long interrogation.

---

## Classification

For each document read, determine:

1. **Which requirement(s) it addresses** — map to the jurisdiction checklist (e.g. "this is their SWMS template" → requirement 3).
2. **State** — does it fully satisfy the requirement (`evidenced`), partially (`partial`), or not really despite its title (`absent`)?
3. **Deficiencies** — common ones that downgrade `evidenced` to `partial`:
   - Undated or no version control
   - Unsigned / no officer endorsement
   - Generic template with placeholder text never filled in ("[Company Name]", "[Insert]")
   - References the **wrong or superseded Act** (e.g. an AU template used by an NZ client, or a pre-HSWA reference)
   - References plant the client doesn't run, or omits plant they do run
   - No evidence of being implemented (a policy that exists but no induction records, no toolbox-talk records)

A document that *looks* complete but is a never-customised template is a `partial` finding with a specific deficiency — and it's often the most valuable thing the audit surfaces, because the client believed they were covered.

---

## Output of ingestion: the provisional gap map

After ingestion you have, for every requirement: a state (evidenced/partial/absent), the evidencing document (if any), and noted deficiencies. This map drives Phase 3 — you interview **only** about:
- Requirements still `absent` with no document (confirm it's truly absent, not just not-in-this-folder).
- `partial` items where you need to know if the gap is paper-only or also real-in-practice.
- Operating-reality checks: does the plant in their SOPs match what they actually run? Are claimed controls actually in use?

Carry the map straight into the `findings.json` (see gap-register.md) — `current_state` and `evidence_source` are populated directly from it.
