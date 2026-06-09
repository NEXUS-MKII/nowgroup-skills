NOW GROUP — NEXUS VOICE ENGINEERING SYSTEM
Voice Analysis, Parameter Extraction & Deployment System
Universal. Repeatable. Code-deployable. Load this before every content session.

Part 1
The Analysis Protocol — how to extract a voice fingerprint from any transcript + 4 written samples

Part 2
The Voice Parameter Block — the structured encoding output, with Chris White / NOW Group as the master example

Part 3
Deployment & Drift Prevention — how this loads into content systems, when it fires, and how drift is detected and corrected

This system works for any client. Part 1 is universal. Part 2 is Chris White's completed encoding — the template for all future client encodings. Part 3 is the developer and operator instruction set.

  PART 1 — THE ANALYSIS PROTOCOL  
How to Extract a Voice Fingerprint
Give this protocol a transcript snippet of at least 800 words and 4 pieces of written content (minimum 150 words each). It outputs a completed Voice Parameter Block in the format defined in Part 2.
The protocol is written to run as a Claude system prompt. It can also be run manually by a human analyst. The output format is identical either way — a structured parameter block that can be copy-pasted into any content session.

INPUTS REQUIRED
INPUT A
Transcript snippet — minimum 800 words. Must be unscripted speech: a session recording, a client call, a voice memo. NOT a prepared speech or a read document. Spoken voice only.

INPUT B
4 written samples — minimum 150 words each. Must be the author's own unedited writing. LinkedIn posts, articles, emails, notes — anything they wrote without a ghostwriter. Label them S1, S2, S3, S4.

If transcript is unavailable: use 6 written samples instead of 4. Mark the output as TRANSCRIPT-ABSENT and add a confidence penalty of -15% to all Cluster A parameters.

CLUSTER A — REGISTER AND TEMPERATURE ANALYSIS
What to find: the coldest line in the corpus, the warmest line, the speed of transition between them, and what triggers the shift.


Parameter
How to measure
Output format
A1
Temperature range
Find the single coldest line across all inputs (most confronting, most direct, least warm). Find the single warmest (most generous, most personal, most empathetic). These are the poles.
COLD: [quote] | WARM: [quote]
A2
Shift trigger
What precedes every temperature shift? Is it: a personal disclosure, a named failure, a provocation landing, a rhetorical question? Identify the trigger pattern across 3+ instances.
TRIGGER: [named pattern] e.g. 'personal disclosure → warmth'
A3
Shift speed
Is there a transition phrase before the register changes, or does it arrive without warning? Count transition phrases vs hard cuts across all shifts found.
SPEED: [transition / hard-cut / mixed] + ratio e.g. '80% hard-cut'
A4
Dominant register
Count sentences that are primarily cold/confronting vs warm/generous. The majority register is the voice's default state.
DOMINANT: [cold/warm/balanced] + ratio e.g. '60% cold, 40% warm'
A5
Warmth type
When warmth appears, what form does it take? Options: retrospective (I was there too), concurrent (I see you), instructional (here is the way), generous (this is what I give you freely).
WARMTH-TYPE: [label] + 1 example quote

CLUSTER B — STRUCTURAL SIGNATURE ANALYSIS
What to find: how the voice builds a piece. Hook type, paragraph rhythm, where key devices appear positionally.


Parameter
How to measure
Output format
B1
Hook type ratio
Classify the opening of each written sample: P=Provocation (challenge/confrontation), S=Scene (physical moment), ST=Statement (declarative truth), Q=Question. Count each type.
HOOKS: P:__ S:__ ST:__ Q:__ | DOMINANT: [type]
B2
Paragraph length pattern
In each written sample, tag paragraph lengths: L=Long (4+ sentences), M=Medium (2-3), S=Short (1 sentence). Map the sequence for each sample e.g. L-L-M-S-M-S-S-S.
PATTERN: [most common sequence] e.g. 'L-M-S-S = long build, short punch'
B3
Self-interruption position
Find every instance of parenthetical, qualification, self-correction, or mid-thought catch. Note which paragraph they appear in (1st, 2nd, 3rd, final). Find the most common position.
SELF-INTERRUPT: paragraph position __ | frequency per 200 words: __
B4
Crystallisation close presence
Does the final substantive line (before CTA) function as a stand-alone principle? Score each sample Y/N. Calculate ratio.
CLOSE-TYPE: crystallisation __/4 | summary __/4 | CTA-only __/4
B5
Sentence length variance
Within a single paragraph, measure shortest sentence word count vs longest. High variance (1 word vs 20+) = rhythmic voice. Low variance = flat voice.
VARIANCE: low/medium/high | shortest: __ words | longest: __ words
B6
Scene vs list ratio
When a multi-part idea is conveyed, is it delivered as a numbered/bulleted list or dissolved into prose/scene? Count instances of each across all samples.
SCENE: __ instances | LIST: __ instances | RATIO: [scene/list/mixed]

CLUSTER C — TRIVIUM WEIGHTING
What to find: which of the three classical modes of meaning — Grammar (being/clarity), Logic (understanding/structure), Rhetoric (expression/persuasion) — leads, which is present underneath, which is absent or weakest.

Score each sample 1 (weak), 2 (present), 3 (dominant) for each Trivium element. Average across 4 samples. The highest average is the lead. The lowest is the weakest.


Element
What to look for
Score 1–3 per sample
Avg
C1
Grammar (Being/Clarity)
Is the voice clear about what it IS — its position, its identity, its grounding? Does the author define terms, name things precisely, state their own position without hedging?
S1:__ S2:__ S3:__ S4:__
__
C2
Logic (Understanding/Structure)
Is the voice structured? Does it build an argument? Does it use cause-effect, contrast, analogy? Does the reader always know why one thought follows another?
S1:__ S2:__ S3:__ S4:__
__
C3
Rhetoric (Expression/Persuasion)
Is the voice beautiful or forceful in its expression? Does it use rhythm, repetition, contrast, imagery? Does it move the reader toward a feeling or an action beyond just understanding?
S1:__ S2:__ S3:__ S4:__
__

Output: TRIVIUM-LEAD: [highest avg] | TRIVIUM-UNDER: [second] | TRIVIUM-WEAK: [lowest] | e.g. 'Logic leads. Rhetoric underneath. Grammar present but weakest.'

CLUSTER D — EMPATHY TILT AND BROADCAST STATE
What to find: the signature empathy pattern — how the author positions themselves relative to the reader's pain — and the broadcast state (fullness vs lack).


Parameter
How to measure
Output format
D1
Empathy tilt type
Find every moment of empathy across all samples. Classify each: RETROSPECTIVE = 'I was there, I know' | CONCURRENT = 'I see you there right now' | INSTRUCTIONAL = 'here is the path out' | GENEROUS = 'I give this freely'. Which dominates?
EMPATHY-TILT: [dominant type] | ratio e.g. '70% retrospective' | 1 anchor quote
D2
Broadcast state
Does the content read as given from fullness (confident, abundant, not seeking return) or from lack (seeking validation, proving, convincing)? Signs of lack: excessive hedging, stacked social proof, over-explaining credibility.
BROADCAST: [fullness/lack/mixed] | specific signal e.g. 'leads with proof of others, not own certainty = mild lack signal'
D3
Signature empathy phrase
The 3 most characteristic empathy phrases across the corpus — the lines that most precisely capture how this voice holds the reader's experience.
EMPATHY-PHRASES: [quote 1] | [quote 2] | [quote 3]
D4
Charisma balance
Warmth-competence ratio: voices that lead with warmth feel safe. Voices that lead with competence feel credible. Which does this voice establish first in most samples?
CHARISMA-LEAD: [warmth/competence/simultaneous]

CLUSTER E — PHILOSOPHICAL REGISTER
What to find: does the voice make abstract or philosophical claims? When? Are they earned before stated? What is the concrete-to-abstract ratio?


Parameter
How to measure
Output format
E1
Philosophical frequency
Count the number of abstract/philosophical claims per 200 words across all written samples. A claim is philosophical if it makes a statement about how reality works, not just how a specific situation worked.
PHIL-FREQ: __ claims per 200 words | HIGH(3+) / MED(1-2) / LOW(0-1)
E2
Earned vs stated
For each philosophical claim, check what precedes it. Is there a concrete scene, a moment, a named experience that earns it? Or does it open before the work is done? Score: EARNED or STATED-FIRST.
EARN-RATIO: earned __/total | stated-first __/total | dominant: [earned/stated/mixed]
E3
Concrete-abstract ratio
In each sample, count concrete sentences (specific, named, observable) vs abstract sentences (general, universal, conceptual). Express as ratio.
CONCRETE:ABSTRACT = __:__ | e.g. '70% concrete, 30% abstract'
E4
Signature philosophical register
The 3 most characteristic philosophical lines — the ones that best represent this voice's intellectual signature. These become voice anchors.
PHIL-ANCHORS: [line 1] | [line 2] | [line 3]

CLUSTER F — DEVICES PRESENT
What to find: which named rhetorical devices appear across the corpus, how frequently, and whether they appear in both transcript and written content (unconscious = authentic).

Run each sample against the device list. Mark Y (present), N (absent), U (unclear). Devices that appear in transcript AND written samples are AUTHENTIC — they are the voice, not a stylistic choice.

Device
Transcript
S1
S2
S3
S4
Freq
Authentic?
Crystallisation close
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Self-interruption
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Scene over list
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Temperature shift
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Earned claim
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Spine sentence anchor
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Wave structure
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Orthogonal reframe
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Fruits test (outcome framing)
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples
Beacon close
Y/N
Y/N
Y/N
Y/N
Y/N
__/5
Y if transcript + 2+ samples

Add custom devices discovered during analysis. If a pattern appears 3+ times and has no existing name — name it and add it to the library.

ANALYSIS OUTPUT: When all 6 clusters are complete, compress into the Voice Parameter Block format defined in Part 2. The Block is the only deliverable that gets deployed. The analysis worksheets are the working documents.

  PART 2 — THE VOICE PARAMETER BLOCK  
The Structured Encoding
This is the output of the analysis protocol. It is the only document that gets deployed into content systems. It must be complete, accurate, and written so a model reading it cold can execute the voice without reconstruction.
Below is the master template followed immediately by Chris White / NOW Group as the completed example.

THE PARAMETER BLOCK — MASTER TEMPLATE
Copy this template for every new client encoding. Replace all [BRACKETED] values with the analysis outputs. Do not remove any field — if a field is unknown, mark it UNSCORED and note what input is missing.

VOICE_PARAMETER_BLOCK_v1
client_id: [CLIENT_ID]
analyst: [NAME] | date: [DATE] | confidence: [%]
corpus: transcript=[Y/N] | written_samples=[N] | total_words_analysed=[N]

# CLUSTER A — REGISTER AND TEMPERATURE
temperature_range:
  cold_pole: '[most confronting line from corpus]'
  warm_pole: '[most generous line from corpus]'
shift_trigger: [named pattern]
shift_speed: [transition/hard-cut/mixed] | ratio: [%]
dominant_register: [cold/warm/balanced] | ratio: [%]
warmth_type: [retrospective/concurrent/instructional/generous]

# CLUSTER B — STRUCTURAL SIGNATURES
hook_type_ratio: P:[n] S:[n] ST:[n] Q:[n] | dominant: [type]
paragraph_pattern: [sequence] | description: [plain english]
self_interrupt_position: paragraph [n] | frequency: [n per 200 words]
close_type: crystallisation:[n/4] summary:[n/4] cta_only:[n/4]
sentence_variance: [low/medium/high] | range: [shortest]-[longest] words
scene_vs_list: scene:[n] list:[n] | dominant: [scene/list/mixed]

# CLUSTER C — TRIVIUM WEIGHTING
trivium_lead: [Grammar/Logic/Rhetoric] | avg: [score]
trivium_under: [Grammar/Logic/Rhetoric] | avg: [score]
trivium_weak: [Grammar/Logic/Rhetoric] | avg: [score]
trivium_note: '[one sentence description of how the three operate together]'

# CLUSTER D — EMPATHY TILT AND BROADCAST STATE
empathy_tilt: [type] | ratio: [%]
broadcast_state: [fullness/lack/mixed]
empathy_anchor_1: '[quote]'
empathy_anchor_2: '[quote]'
empathy_anchor_3: '[quote]'
charisma_lead: [warmth/competence/simultaneous]

# CLUSTER E — PHILOSOPHICAL REGISTER
phil_frequency: [HIGH/MED/LOW] | claims per 200 words: [n]
earn_ratio: earned:[n] stated_first:[n] | dominant: [earned/stated/mixed]
concrete_abstract_ratio: [%]:[%]
phil_anchor_1: '[signature philosophical line]'
phil_anchor_2: '[signature philosophical line]'
phil_anchor_3: '[signature philosophical line]'

# CLUSTER F — DEVICES
authentic_devices: [comma-separated list of devices scoring Y]
absent_devices: [comma-separated list of devices scoring N across all samples]
custom_devices: [any newly named devices discovered]

# NEXUS FINGERPRINT FIELDS (from nexus_brand_discovery.py)
voice_fingerprint: '[2-3 sentence voice description]'
tone_descriptors: [comma-separated list]
banned_words: [comma-separated list]
signature_phrases: [comma-separated list — these are verbatim, do not paraphrase]
sector_focus: [industry/niche]
audience_who: '[tight one-line description]'
primary_problem: '[the core pain this voice addresses]'
primary_promise: '[the core transformation this voice offers]'
differentiation_claim: '[what makes this voice/brand distinctly different]'
voice_samples: ['quote 1' | 'quote 2' | 'quote 3']
discovered_keywords: [comma-separated list]
content_themes: [comma-separated list]

# SPINE SENTENCES (max 10 — these anchor every piece of content)
spine_1: '[sentence]'
spine_2: '[sentence]'
spine_3: '[sentence]'
# ... up to spine_10

# CONTENT GENERATION RULES (loaded with every batch)
rule_hook: [instruction for opening every piece]
rule_close: [instruction for closing every piece]
rule_register: [instruction for managing temperature shifts]
rule_structure: [instruction for paragraph architecture]
rule_philosophy: [instruction for when and how to deploy philosophical claims]
rule_empathy: [instruction for how to hold the reader's pain]
rule_cta: [instruction for CTA placement and format]

END_VOICE_PARAMETER_BLOCK


CHRIS WHITE / NOW GROUP — COMPLETED PARAMETER BLOCK
This is the master encoding. Load this at the start of every NOW Group content session. It supersedes any generic voice instructions. If there is a conflict between this block and a general instruction, this block wins.

VOICE_PARAMETER_BLOCK_v1
client_id: NOW_GROUP_CHRIS_WHITE
analyst: Campaign build session | date: 2026-04 | confidence: 88%
corpus: transcript=Y (Session 1 TSVC, ~4000 words) | written_samples=6 | total_words_analysed=~8000

# CLUSTER A — REGISTER AND TEMPERATURE
temperature_range:
  cold_pole: 'That is on me.'
  warm_pole: 'I bought the company. Same network. Changed the pre-conditions.'
shift_trigger: personal_disclosure — warmth arrives immediately after the author names their own failure or complicity. No transition.
shift_speed: hard-cut | ratio: 85% hard-cut, 15% transitional
dominant_register: cold | ratio: 60% confronting/precise, 40% warm/generous
warmth_type: retrospective — 'I was there, I named it too late, here is what that cost and produced'

# CLUSTER B — STRUCTURAL SIGNATURES
hook_type_ratio: P:3 S:2 ST:1 Q:0 | dominant: provocation
paragraph_pattern: L-L-M-S-S | description: long build, medium turn, two short punches. The S-S at the end is the signature.
self_interrupt_position: paragraph 3-4 (never opening, never final) | frequency: 1.2 per 200 words
close_type: crystallisation:5/6 summary:1/6 cta_only:0/6
sentence_variance: HIGH | range: 3-28 words within single paragraphs
scene_vs_list: scene:8 list:1 | dominant: scene — frameworks dissolved into prose in 89% of cases

# CLUSTER C — TRIVIUM WEIGHTING
trivium_lead: Logic | avg: 2.8/3
trivium_under: Rhetoric | avg: 2.4/3
trivium_weak: Grammar | avg: 1.9/3
trivium_note: 'Logic structures every piece and is always present. Rhetoric arrives in rhythm and crystallisation moments. Grammar (clarity of being/position) is present but often implicit — Chris states what things ARE through what they DO, not through direct definition.'

# CLUSTER D — EMPATHY TILT AND BROADCAST STATE
empathy_tilt: retrospective | ratio: 75% retrospective, 20% generous, 5% instructional
broadcast_state: fullness — content gives without requiring return. No stacked social proof. Credibility assumed not argued.
empathy_anchor_1: 'That is on me.'
empathy_anchor_2: 'I felt the friction early. I overrode it.'
empathy_anchor_3: 'You feel it before you can name it. I named it too late.'
charisma_lead: competence-first — establishes precision and observation before warmth. Warmth lands harder because of this sequencing.

# CLUSTER E — PHILOSOPHICAL REGISTER
phil_frequency: HIGH | claims per 200 words: 2.3
earn_ratio: earned:9 stated_first:1 | dominant: earned — philosophical claims appear at or near close of a section that has done the work
concrete_abstract_ratio: 65%:35%
phil_anchor_1: 'All true consciousness is coherence.'
phil_anchor_2: 'Truth travels at zero resistance. Deception creates drag in the channel.'
phil_anchor_3: 'The field reflects what you broadcast. Always.'

# CLUSTER F — DEVICES
authentic_devices: crystallisation_close, self_interruption, scene_over_list, temperature_shift, earned_claim, spine_sentence_anchor, retrospective_empathy, orthogonal_reframe
absent_devices: question_hook, numbered_list_framework, concurrent_empathy
custom_devices: repulsion_mechanism_check (broadcasting from lack vs fullness), stag_hunt_reference (collective reward framing), channel_metaphor (truth/deception as signal physics)

# NEXUS FINGERPRINT FIELDS
voice_fingerprint: 'Operator-intellectual. Thinks in systems, speaks in physics analogies. RAF signals engineering meets performance psychology — precision tools applied to human relationships. Savage then warm, without warning or transition. Coins his own concepts rather than borrowing borrowed frameworks.'
tone_descriptors: precise, confronting, generous, philosophical, self-aware, retrospective, warm-without-announcement
banned_words: genuinely, honestly, straightforward, synergy, leverage, paradigm, best-in-class, digital-transformation, in-todays-business-environment, I-want-to-talk-to-you-about, many-business-owners-struggle-with, as-you-can-see
signature_phrases: 'Truth travels at zero resistance', 'You will know them by their fruits', 'The field reflects what you broadcast. Always.', 'That is on me.', 'Contribution without engineering burns what it was meant to warm.', 'Not a mastermind. A resonance chamber.', 'Operators only.'
sector_focus: SME networking, partner ecosystems, business growth, relationship engineering
audience_who: 'Ambitious lifestyle business operators — trades, professional services, coaching, consulting — who are building seriously and networking strategically. Not beginners. People who already give a lot and want systems underneath the generosity.'
primary_problem: 'High-effort networking producing low-leverage outcomes. Goodwill without engineering. Partnership without pre-conditions. Contribution without a hearth.'
primary_promise: 'A referral engine that compounds. Partner relationships that produce outcomes neither party could have conceived alone. The engineering underneath the contribution.'
differentiation_claim: 'Chris has been inside a Shadow Mastermind at the partnership level, named it too late, bought the company, and rebuilt the pre-conditions from the inside. He teaches what he learned through failure, not theory.'
voice_samples: ['That is on me.' | 'Truth travels at zero resistance. Deception creates drag in the channel.' | 'Contribution without engineering burns what it was meant to warm.']
discovered_keywords: resonance, coherence, field, signal, frequency, broadcast, engineering, pre-conditions, harvest, contribution, referral-engine, tinderbox, mastermind, shadow
content_themes: partnership-engineering, referral-systems, mastermind-philosophy, coherence-physics, relationship-ladder, VMV-clarity, voice-anchoring, operator-growth

# SPINE SENTENCES
spine_1: 'You are already in a mastermind. The question is whether you chose it.'
spine_2: 'The field reflects what you broadcast. Always.'
spine_3: 'You will know them by their fruits.'
spine_4: 'All true consciousness is coherence.'
spine_5: 'Contribution without engineering burns what it was meant to warm.'
spine_6: 'A fire without a hearth does not heat the room. It burns it.'
spine_7: 'Truth travels at zero resistance. Deception creates drag in the channel.'
spine_8: 'You feel it before you can name it. Learn to listen to the channel.'
spine_9: 'The resonance chamber is what happens when you engineer coherence rather than hoping proximity produces it.'
spine_10: 'That is on me.'

# CONTENT GENERATION RULES
rule_hook: Open with provocation, scene, or declaration. NEVER with context, setup, or preamble. The hook IS the post. Test: would someone stop scrolling for this line alone?
rule_close: Final substantive line before CTA must be a crystallisation — a principle that stands alone, survives extraction, and travels without the post around it. Never a summary. Never a transition.
rule_register: Default to cold/precise. Warmth arrives without announcement after a personal disclosure or named failure. Never signal the shift. Never transition into it. Hard-cut only.
rule_structure: Long build → medium turn → two short punches. S-S at the end. Sentence variance HIGH within paragraphs. Self-interrupt appears in paragraphs 3-4. Never in opening, never in final.
rule_philosophy: Philosophical claims close — they do not open. Every abstract claim must be preceded by a concrete scene or named moment that earns it. Frequency: 2-3 per 200 words MAX. Phil anchor lines are verbatim — do not paraphrase.
rule_empathy: Retrospective. 'I was there. I named it too late. Here is what that cost and produced.' Never pitying. Never concurrent ('I see you struggling'). The author has been through it — that is the empathy. State the price paid, not the sympathy offered.
rule_cta: One CTA. Never stacked. Positioned after crystallisation line. Phase-matched: Soft wk1-3, Mid wk4-6, Hard wk7-8. DM keyword in ALL CAPS when used.

END_VOICE_PARAMETER_BLOCK


  PART 3 — DEPLOYMENT AND DRIFT PREVENTION  
How to Wire This Into Content Systems
The Voice Parameter Block is worthless if it isn't loaded. This section tells developers exactly where it loads, how it loads, and how to detect and correct voice drift before it compounds across a campaign.

DEPLOYMENT ARCHITECTURE


Layer
What happens here
1
System prompt (persistent)
The full VOICE_PARAMETER_BLOCK is injected as the first element of every system prompt for this client. It fires before any user message. It is never summarised — always loaded in full. In Claude API calls: system parameter. In Claude Projects: project instructions.
2
Batch priming (per session)
At the start of every content session, before any content is requested, a priming message fires. Format below. This confirms the parameter block is active and loads the specific batch context (which week, which CTA phase, which spine sentence to anchor on).
3
Per-asset instruction (per piece)
Every content request includes: the asset type, the week number, the CTA phase, and the specific spine sentence to build around. Never request 'a post' — always request 'a [type] post for week [n] anchored on spine_[n], CTA phase [soft/mid/hard].'
4
Encoder gate (post-generation)
Every generated piece runs through the Encoder Lite before it is accepted. If Lite score < 7/8, it is returned for regeneration with specific failure notes, not accepted and edited manually. Manual editing of AI output trains the model toward the editor's voice, not the client's.
5
Drift detection (weekly)
At the end of each campaign week, the operator runs one piece through the Full Encoder. The pattern note (which checks failed most) is recorded. If the same check fails 3 weeks running — the parameter block needs a rule update, not just a post edit.

THE BATCH PRIMING MESSAGE — COPY-PASTE FORMAT
This fires at the start of every content session. Replace bracketed values. Send this before any content requests in the session.

PRIMING: NOW GROUP / CHRIS WHITE
Voice parameter block: NOW_GROUP_CHRIS_WHITE is active.
Session context:
  Campaign: Tinderbox July 9 launch
  Week: [WEEK NUMBER]
  CTA phase: [SOFT / MID / HARD]
  Anchor spine sentence: [SPINE SENTENCE FROM BLOCK]
  Assets to produce this session: [LIST]

Confirm parameters active before producing any content.
All content in this session must pass Encoder Lite (7/8) before delivery.
Do not summarise or paraphrase signature_phrases. Use verbatim or not at all.
Philosophical claims: earned before stated. Never open with a phil anchor.
Temperature: default cold/precise. Warmth arrives after personal disclosure. Hard-cut only.
END PRIMING

THE PER-ASSET REQUEST FORMAT
Every content request in the session uses this format. Never say 'write me a post about X'. Always use the structured request. It loads the right constraints before generation starts.

ASSET REQUEST
type: [copy_post / article / video_script / carousel_spec / infographic_spec]
week: [n]
cta_phase: [soft / mid / hard]
cta_keyword: [TINDERBOX / VMV / ADD-ONE / etc]
anchor_spine: [exact spine sentence from block]
angle: [pain-contrast-framework / myth-reality-example / case-vignette / steelman / etc]
hook_type: [provocation / scene / statement]
primary_device: [crystallisation_close / scene_over_list / temperature_shift / etc]
length: [120-220 words / 1400-1800 words / 45-60 seconds]
notes: [any session-specific context]
END ASSET REQUEST

DRIFT DETECTION — THE 5 WARNING SIGNALS
Voice drift happens when the parameter block is active but the generation is subtly defaulting toward generic LinkedIn content under the surface. These are the five early signals. Catch any one of these and the batch needs to be re-run, not manually edited.

Drift signal
What it looks like
Correction
Context before hook
Post opens with a scene-setting paragraph before the provocation. 'In the world of modern networking...' or 'Many business owners face...'
Re-run. Add to priming: 'The hook is the first word. No preamble. No context. No setup.'
Symmetrical lists
Three points of equal length. Four steps all presented the same way. The rhythm is identical across items. The post reads like a template.
Re-run. Add to priming: 'Dissolve all frameworks into prose or scene. No numbered lists. No bullet points. If a list appears in a draft — it must be rewritten as a scene.'
One temperature throughout
Post is warm all the way through. Or precise all the way through. No shift. No moment where the register changes without warning.
Re-run. Add to priming: 'Find the coldest true line in this topic. Include it. Then find where the warmth should arrive. Hard-cut between them.'
Summary close
Final line before CTA recaps what was said. 'So as you can see, engineering your partnerships is the key.' Or 'That's why partnership engineering matters.'
Re-run. Add to priming: 'The final line before the CTA must be a principle that survives extraction. Read the crystallisation close examples from the parameter block. Match that register.'
Philosophical claim leads
A spine sentence or philosophical anchor appears in the first two lines before the post has done any work to earn it.
Re-run. Add to priming: 'Phil anchors close. They do not open. Move the philosophical claim to the final substantive paragraph. Build the concrete scene first.'

RE-ENCODING PROTOCOL — WHEN TO UPDATE THE PARAMETER BLOCK
The parameter block is not static. It is a living encoding that updates as the voice evolves and as more corpus data becomes available.

Trigger
Action
Who
Same Encoder check fails 3 weeks running
Add a specific rule to rule_[parameter] in the parameter block. Do not just note it — encode it as an explicit constraint.
Operator + Chris approval
New spine sentence coined in session
Add to spine list immediately. If over 10, remove the weakest existing spine (lowest frequency of use across campaign).
Operator
New custom device identified
Name it. Add to custom_devices. Add a prompt instruction. Add it to the Encoder Full device table.
Operator + Chris approval
New campaign launches (different product/audience)
Run the batch priming update only — do not re-encode the full block. Add campaign-specific context in the SESSION CONTEXT section of the priming message.
Operator
Quarterly pattern review
Run 3 existing posts through Full Encoder. Record pattern note. If any cluster shows consistent weakness — add a targeted rule to that cluster in the block.
Chris + Operator
Client voice evolves significantly
Re-run analysis protocol on new corpus. Generate new parameter block. Version-stamp it (v2, v3). Keep previous versions archived.
Analyst

FOR DEVELOPERS — NEXUS PIPELINE INTEGRATION
The Voice Parameter Block is designed to slot directly into the nexus_brandkit_ingest.py pipeline as the synthesis output. The nexus_brand_discovery.py scrape populates the NEXUS FINGERPRINT FIELDS section. The analysis protocol populates Clusters A–F. The pipeline merges both into the complete block.

The NEXUS FINGERPRINT FIELDS in the parameter block (voice_fingerprint through content_themes) map 1:1 to the 13 fields output by nexus_brand_discovery.py. The Cluster A–F fields are the additional encoding layer that nexus_brandkit_ingest.py synthesises on top of the scrape. Together they form the Brand Blueprint + Voice Encoder for every paid buyer.

Implementation sequence for new client onboarding:
1. nexus_brand_discovery.py scrapes site → outputs 13 nexus fingerprint fields
2. Analyst runs analysis protocol on transcript + 4 written samples → outputs clusters A-F
3. nexus_brandkit_ingest.py merges both outputs → generates complete VOICE_PARAMETER_BLOCK
4. Block is stored in client record and loaded as system prompt prefix for all content sessions
5. Batch priming message is generated from block fields → operator customises session context
6. Per-asset requests use structured format → Encoder Lite gates all output
7. Weekly drift detection → parameter block updated when warranted

The parameter block format is designed to be model-agnostic. It loads into Claude, GPT-4, Gemini, or any capable model as a system prompt prefix without reformatting. The CODE format (not JSON, not YAML) was chosen deliberately: it is human-readable, version-controllable, and copy-pasteable without parsing errors.

NOW Group — Voice Analysis, Parameter Extraction & Deployment System — v1.0 — nowgroup.co.nz
