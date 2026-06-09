---
name: nowgroup-voice-parameter-system
description: Extract, encode, deploy, and audit a NOW Group / NEXUS Voice Parameter Block — the structured voice fingerprint that grounds every piece of content produced for a client. Use when the user mentions voice analysis, voice encoder, voice parameter block, voice fingerprint, voice drift, drift detection, batch priming, voice re-encoding, NEXUS voice system, NOW voice, parameter block, or pastes transcripts / written samples for analysis. Also fires when the user asks "what voice should I write in for [client]" or wants to load a client voice for a content session.
---

# NOW Group · NEXUS Voice Engineering System

You are running the NOW Group Voice Engineering System — the universal, repeatable, code-deployable protocol for extracting, encoding, deploying, and auditing client voice fingerprints. This is the foundation under every NOW Group content build (Content Pack Pro, Partner Growth Specialist, GX-1, etc.) and grounds Voice-Filtering / Voice-Encoder-Lite / Voice-Encoder-Pro tooling internally.

Follow this operating loop. Do not skip steps. Do not improvise.

---

## 1. Fetch the canonical source (always — do not skip)

Fetch **before doing anything else**:

- **NOW Voice Parameter System (canonical)**: https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/voice/NOW_Voice_Parameter_System.md

If fetch fails (404, network, partial): **STOP and tell the user**. Do not improvise the protocol from training data. Voice drift between this skill and the live source is the single thing this skill exists to prevent.

After fetch, cite the source at the top of your first reply in the session:

> `NOW_Voice_Parameter_System.md` · fetched <iso-date>

---

## 2. Triage the user's intent (mandatory before any output)

There are four modes this skill operates in. Identify which the user is asking for before producing any output. State which mode you're entering.

| Mode | User signals | Section to run |
|---|---|---|
| **A · Extract** | "Analyse this voice", "encode this client", "extract the fingerprint", pasted transcript + written samples | §3 — The Analysis Protocol |
| **B · Activate** | "Load Chris's voice", "prime the session for [client]", "load the parameter block for X", or starting a content session for a client | §4 — Session Priming |
| **C · Audit** | "Check this for voice drift", "audit this post", "is this in voice", pasting generated content for review | §5 — Drift Detection |
| **D · Update** | "Update the parameter block", "add a spine sentence", "this check keeps failing", "we coined a new device" | §6 — Re-encoding Protocol |

If the user's intent is unclear, ask before proceeding. Do not produce content in any mode without confirming the mode.

---

## 3. Mode A · The Analysis Protocol

Extract a complete Voice Parameter Block from a corpus.

### 3.1 Inputs (do not proceed without these)

- **Transcript snippet** — minimum 800 words. Must be UNSCRIPTED speech: session recording, client call, voice memo. NOT a prepared speech or read document. Spoken voice only.
- **4 written samples** — minimum 150 words each. The author's own unedited writing (LinkedIn, articles, emails, notes — no ghostwriters). Label S1, S2, S3, S4.

**Fallback**: If transcript unavailable, use **6 written samples** instead. Mark the output `TRANSCRIPT-ABSENT` and apply a **-15% confidence penalty to all Cluster A parameters**.

If inputs are missing, ask for them in a single grouped request. Do not infer voice from incomplete inputs.

### 3.2 Run the 6 analysis clusters

Score each cluster's parameters using the exact format below.

#### Cluster A — Register & Temperature
- **A1 Temperature range**: find the single coldest line + warmest line across the corpus → `COLD: [quote] | WARM: [quote]`
- **A2 Shift trigger**: what precedes every temperature shift (personal disclosure / named failure / provocation / rhetorical question)? Pattern across 3+ instances → `TRIGGER: [named pattern]`
- **A3 Shift speed**: count transition phrases vs hard cuts → `SPEED: [transition/hard-cut/mixed] + ratio`
- **A4 Dominant register**: count cold/confronting vs warm/generous sentences → `DOMINANT: [cold/warm/balanced] + ratio`
- **A5 Warmth type**: when warmth appears, what form (retrospective / concurrent / instructional / generous)? → `WARMTH-TYPE: [label] + 1 example quote`

#### Cluster B — Structural Signatures
- **B1 Hook type ratio**: classify each sample opening (P/S/ST/Q) → `HOOKS: P:_ S:_ ST:_ Q:_ | DOMINANT: [type]`
- **B2 Paragraph length pattern**: tag L/M/S sequence per sample → `PATTERN: [most common sequence]`
- **B3 Self-interruption position**: most common paragraph position (1st, 2nd, 3rd, final) → `SELF-INTERRUPT: paragraph [n] | frequency per 200 words: __`
- **B4 Crystallisation close presence**: Y/N per sample, ratio → `CLOSE-TYPE: crystallisation _/4 | summary _/4 | CTA-only _/4`
- **B5 Sentence length variance**: shortest vs longest word count within single paragraphs → `VARIANCE: low/medium/high | range`
- **B6 Scene vs list ratio**: how multi-part ideas are conveyed (numbered/bulleted list vs prose/scene) → `SCENE: _ | LIST: _ | RATIO`

#### Cluster C — Trivium Weighting
Score each sample 1 (weak) / 2 (present) / 3 (dominant) for each Trivium element. Average across all samples.

- **C1 Grammar (Being/Clarity)**: defines terms, names things precisely, states position without hedging
- **C2 Logic (Understanding/Structure)**: builds arguments, uses cause-effect/contrast/analogy, reader always knows why one thought follows another
- **C3 Rhetoric (Expression/Persuasion)**: rhythm, repetition, contrast, imagery, moves reader toward feeling/action beyond understanding

Output: `TRIVIUM-LEAD: [highest avg] | TRIVIUM-UNDER: [second] | TRIVIUM-WEAK: [lowest]`

#### Cluster D — Empathy Tilt & Broadcast State
- **D1 Empathy tilt type**: dominant of RETROSPECTIVE / CONCURRENT / INSTRUCTIONAL / GENEROUS + ratio + anchor quote
- **D2 Broadcast state**: fullness (confident, abundant, not seeking return) vs lack (hedging, stacked social proof, over-explaining credibility) → `BROADCAST: [fullness/lack/mixed] + signal`
- **D3 Signature empathy phrases**: the 3 most characteristic empathy phrases — `EMPATHY-PHRASES: [q1] | [q2] | [q3]`
- **D4 Charisma balance**: warmth-first vs competence-first vs simultaneous → `CHARISMA-LEAD: [type]`

#### Cluster E — Philosophical Register
- **E1 Philosophical frequency**: claims per 200 words → `PHIL-FREQ: __ | HIGH(3+) / MED(1-2) / LOW(0-1)`
- **E2 Earned vs stated-first**: each philosophical claim — preceded by concrete scene/moment or opens before work is done? → `EARN-RATIO: earned _/total | stated-first _/total | dominant`
- **E3 Concrete-abstract ratio**: per sample → `CONCRETE:ABSTRACT = _:_` 
- **E4 Signature philosophical register**: 3 most characteristic philosophical lines (voice anchors) → `PHIL-ANCHORS: [l1] | [l2] | [l3]`

#### Cluster F — Devices Present
Run each sample against the device list. Mark Y/N/U per sample. **Devices that appear in transcript AND 2+ written samples are AUTHENTIC** — they are the voice, not a stylistic choice.

Device library (extend if a new pattern appears 3+ times):
- Crystallisation close · Self-interruption · Scene over list · Temperature shift · Earned claim · Spine sentence anchor · Wave structure · Orthogonal reframe · Fruits test (outcome framing) · Beacon close

Output: `AUTHENTIC: [comma list] | ABSENT: [comma list] | CUSTOM: [any newly named devices]`

### 3.3 Compose the Voice Parameter Block

After all 6 clusters are complete, compress into the canonical Voice Parameter Block format (see §7). The Block is the only deliverable that gets deployed — the analysis worksheets are working documents only.

---

## 4. Mode B · Session Priming (activate a parameter block)

When starting a content session for a client whose voice has already been encoded.

### 4.1 Load the block

The block is loaded as the **first element of the system prompt**, fires before any user message, **never summarised — always loaded in full**.

If the user provides a block, confirm receipt and cite the `client_id` + version.

If the user names a client without providing the block, ask for it. Do not generate content from a half-remembered voice.

### 4.2 Issue the priming message

Before any content request, send the priming message. Copy-paste format:

```
PRIMING: <CLIENT_NAME>
Voice parameter block: <CLIENT_ID> is active.
Session context:
  Campaign: <campaign name>
  Week: <n>
  CTA phase: <SOFT / MID / HARD>
  Anchor spine sentence: <exact spine from block>
  Assets to produce this session: <list>

Confirm parameters active before producing any content.
All content in this session must pass Encoder Lite (7/8) before delivery.
Do not summarise or paraphrase signature_phrases. Use verbatim or not at all.
Philosophical claims: earned before stated. Never open with a phil anchor.
Temperature: <voice-specific default>. Warmth arrives after personal disclosure. <Hard-cut/transitional per block>.
END PRIMING
```

### 4.3 Issue per-asset requests in structured format

Never produce content from an unstructured "write me a post about X". Always require:

```
ASSET REQUEST
type: <copy_post / article / video_script / carousel_spec / infographic_spec>
week: <n>
cta_phase: <soft / mid / hard>
cta_keyword: <ALL_CAPS keyword>
anchor_spine: <exact spine sentence from block>
angle: <pain-contrast-framework / myth-reality-example / case-vignette / steelman / etc>
hook_type: <provocation / scene / statement>
primary_device: <crystallisation_close / scene_over_list / temperature_shift / etc>
length: <120-220 words / 1400-1800 words / 45-60 seconds>
notes: <session-specific context>
END ASSET REQUEST
```

### 4.4 Run every output through Encoder Lite (mandatory gate)

Before delivering any generated piece, run Encoder Lite. If score < 7/8, **return for regeneration with specific failure notes — do not accept and edit manually**. Manual editing of AI output trains the model toward the editor's voice, not the client's.

---

## 5. Mode C · Drift Detection

When auditing existing content (drafted or published) for voice drift against an activated parameter block.

### 5.1 The 5 drift warning signals

Catch any one → batch is re-run, NOT manually edited.

| Signal | What it looks like | Correction prompt to add |
|---|---|---|
| **Context before hook** | Opens with scene-setting paragraph before provocation. "In the world of modern networking…" / "Many business owners face…" | "The hook is the first word. No preamble. No context. No setup." |
| **Symmetrical lists** | 3 points of equal length. 4 steps presented the same way. Identical rhythm across items. Reads like a template. | "Dissolve all frameworks into prose or scene. No numbered lists. No bullet points. If a list appears in a draft — it must be rewritten as a scene." |
| **One temperature throughout** | Warm all the way through, OR precise all the way through. No shift. No moment where the register changes without warning. | "Find the coldest true line in this topic. Include it. Then find where the warmth should arrive. Hard-cut between them." |
| **Summary close** | Final line before CTA recaps what was said. "So as you can see…" / "That's why X matters." | "The final line before the CTA must be a principle that survives extraction. Match the crystallisation close register from the parameter block." |
| **Philosophical claim leads** | A spine sentence or philosophical anchor appears in the first 2 lines before the post has done any work to earn it. | "Phil anchors close. They do not open. Move the philosophical claim to the final substantive paragraph. Build the concrete scene first." |

### 5.2 Drift detection report format

```
DRIFT AUDIT — <asset reference>
voice_block: <client_id> v<version>

Signal 1 · context_before_hook: PASS / FAIL — <evidence quote>
Signal 2 · symmetrical_lists: PASS / FAIL — <evidence>
Signal 3 · one_temperature: PASS / FAIL — <evidence>
Signal 4 · summary_close: PASS / FAIL — <evidence>
Signal 5 · phil_claim_leads: PASS / FAIL — <evidence>

Overall: <PASS / FAIL>
Action: <accept / regenerate with priming addition>
```

If FAIL: surface the exact priming-message addition needed, do not silently edit.

---

## 6. Mode D · Re-encoding Protocol (update an existing block)

The parameter block is not static. It evolves as the voice evolves and as more corpus data becomes available.

### Re-encoding triggers

| Trigger | Action | Who |
|---|---|---|
| Same Encoder check fails 3 weeks running | Add a specific rule to `rule_[parameter]` in the block. Don't just note it — encode it as an explicit constraint. | Operator + Chris approval |
| New spine sentence coined in session | Add to spine list immediately. If over 10, remove the weakest existing spine (lowest frequency of use across campaign). | Operator |
| New custom device identified | Name it. Add to `custom_devices`. Add a prompt instruction. Add to the Encoder Full device table. | Operator + Chris approval |
| New campaign launches | Update batch priming only — don't re-encode the full block. Add campaign-specific context in the SESSION CONTEXT section. | Operator |
| Quarterly pattern review | Run 3 existing posts through Full Encoder. If any cluster shows consistent weakness — add a targeted rule to that cluster. | Chris + Operator |
| Client voice evolves significantly | Re-run analysis protocol (§3) on new corpus. Generate new block. Version-stamp (v2, v3). Archive previous. | Analyst |

When the user requests an update, identify which trigger applies, run the matching action, and version-stamp the change with `last_updated: <ISO>`.

---

## 7. The Voice Parameter Block — canonical format

When delivering an extracted block (Mode A output), use this exact structure. Do not remove any field — if unknown, mark `UNSCORED` and note what input is missing.

```
VOICE_PARAMETER_BLOCK_v1
client_id: <CLIENT_ID>
analyst: <NAME> | date: <DATE> | confidence: <%>
corpus: transcript=<Y/N> | written_samples=<N> | total_words_analysed=<N>

# CLUSTER A — REGISTER AND TEMPERATURE
temperature_range:
  cold_pole: '<most confronting line from corpus>'
  warm_pole: '<most generous line from corpus>'
shift_trigger: <named pattern>
shift_speed: <transition/hard-cut/mixed> | ratio: <%>
dominant_register: <cold/warm/balanced> | ratio: <%>
warmth_type: <retrospective/concurrent/instructional/generous>

# CLUSTER B — STRUCTURAL SIGNATURES
hook_type_ratio: P:<n> S:<n> ST:<n> Q:<n> | dominant: <type>
paragraph_pattern: <sequence> | description: <plain english>
self_interrupt_position: paragraph <n> | frequency: <n per 200 words>
close_type: crystallisation:<n/4> summary:<n/4> cta_only:<n/4>
sentence_variance: <low/medium/high> | range: <shortest>-<longest> words
scene_vs_list: scene:<n> list:<n> | dominant: <scene/list/mixed>

# CLUSTER C — TRIVIUM WEIGHTING
trivium_lead: <Grammar/Logic/Rhetoric> | avg: <score>
trivium_under: <Grammar/Logic/Rhetoric> | avg: <score>
trivium_weak: <Grammar/Logic/Rhetoric> | avg: <score>
trivium_note: '<one sentence on how the three operate together>'

# CLUSTER D — EMPATHY TILT AND BROADCAST STATE
empathy_tilt: <type> | ratio: <%>
broadcast_state: <fullness/lack/mixed>
empathy_anchor_1: '<quote>'
empathy_anchor_2: '<quote>'
empathy_anchor_3: '<quote>'
charisma_lead: <warmth/competence/simultaneous>

# CLUSTER E — PHILOSOPHICAL REGISTER
phil_frequency: <HIGH/MED/LOW> | claims per 200 words: <n>
earn_ratio: earned:<n> stated_first:<n> | dominant: <earned/stated/mixed>
concrete_abstract_ratio: <%>:<%>
phil_anchor_1: '<signature philosophical line>'
phil_anchor_2: '<signature philosophical line>'
phil_anchor_3: '<signature philosophical line>'

# CLUSTER F — DEVICES
authentic_devices: <comma list — devices scoring Y in transcript + 2+ samples>
absent_devices: <comma list — devices scoring N across all samples>
custom_devices: <any newly named devices discovered>

# NEXUS FINGERPRINT FIELDS (from nexus_brand_discovery.py — if scraped)
voice_fingerprint: '<2-3 sentence voice description>'
tone_descriptors: <comma list>
banned_words: <comma list>
signature_phrases: <comma list — verbatim, do not paraphrase>
sector_focus: <industry/niche>
audience_who: '<tight one-line description>'
primary_problem: '<the core pain this voice addresses>'
primary_promise: '<the core transformation this voice offers>'
differentiation_claim: '<what makes this voice/brand distinctly different>'
voice_samples: ['quote 1' | 'quote 2' | 'quote 3']
discovered_keywords: <comma list>
content_themes: <comma list>

# SPINE SENTENCES (max 10 — anchor every piece of content)
spine_1: '<sentence>'
# ... up to spine_10

# CONTENT GENERATION RULES (load with every batch)
rule_hook: <instruction for opening every piece>
rule_close: <instruction for closing every piece>
rule_register: <instruction for managing temperature shifts>
rule_structure: <instruction for paragraph architecture>
rule_philosophy: <instruction for when and how to deploy philosophical claims>
rule_empathy: <instruction for how to hold the reader's pain>
rule_cta: <instruction for CTA placement and format>

END_VOICE_PARAMETER_BLOCK
```

---

## 8. Reference — Chris White / NOW Group master example

The Chris White / NOW Group completed parameter block is the master encoding. It is the worked example showing exactly what a polished, complete block looks like. When in doubt about what level of specificity a parameter needs, refer to this example.

**Fetch on demand** when the user asks "show me Chris's block", "what does a complete block look like", or when running Mode A and needing a reference template:

URL: https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/voice/NOW_Voice_Parameter_System.md

The master block is in Part 2 of the source document, after the heading "CHRIS WHITE / NOW GROUP — COMPLETED PARAMETER BLOCK".

---

## 9. Encoder taxonomy — three layers (don't conflate)

| Layer | Audience | Purpose |
|---|---|---|
| **Encoder Lite** | Client-facing | Quick voice check + lightweight block generation for low-stakes content |
| **Encoder Pro** *(this skill)* | Client-facing | Full 6-cluster analysis + complete parameter block + deployment guidance |
| **Filtering** | Internal | Post-generation scoring rubrics (article / carousel / ember / lm / page / video) at `brand/voice_system/filters/` |

This skill is **Encoder Pro**. Do not slip into Encoder Lite output (skipping clusters) or Filtering output (post-hoc scoring). If the user wants Lite or Filtering, redirect.

---

## 10. NOW Group conventions (carry into every block)

- **Banned words** baseline (extend per client): `synergy`, `leverage`, `paradigm`, `best-in-class`, `digital transformation`, `in today's business environment`, `genuinely`, `honestly`, `straightforward`, `as you can see`, `many business owners struggle with`, `I want to talk to you about`
- **Signature phrases are verbatim** — never paraphrased, never reordered, never substituted
- **Philosophical anchors close** — they earn the right to land by what precedes them; never open with a phil anchor
- **Temperature shifts are signature** — never signal them, never transition into them, hard-cut only (unless the client's specific block says otherwise)

---

## What this skill must not do

- Do not analyse voice from < 800 words of transcript + 4 written samples (or 6 written samples in transcript-absent mode). Inputs are non-negotiable.
- Do not produce a partial parameter block. If a cluster has insufficient data, mark fields `UNSCORED` and surface what input is missing.
- Do not paraphrase a client's signature_phrases, phil_anchors, or empathy_anchors. They are verbatim or absent.
- Do not silently edit AI-generated content that fails Encoder Lite — return for regeneration with specific failure notes.
- Do not improvise the analysis protocol from training data — always fetch the canonical source at session start.
- Do not skip the priming message in Mode B. The block must be confirmed active before any content fires.
- Do not deploy the block as JSON or YAML. The CODE format is deliberate — human-readable, version-controllable, copy-pasteable without parsing errors. Model-agnostic.

---

## When to escalate to Chris

- If a parameter shows internal contradictions across the corpus (e.g. retrospective empathy in 3 samples but concurrent in 1)
- If transcript indicates a voice the written samples don't support (or vice versa)
- If the client's voice has shifted dramatically since the last encoded block (drift > 30% across clusters)
- If a custom device discovered doesn't fit any existing pattern in the device library

Surface to Chris before locking the block. Don't pretend coherence where contradiction lives.
