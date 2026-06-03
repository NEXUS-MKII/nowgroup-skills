---
name: consultative-selling-thread-builder
description: Spin up a primed sales thread for consultative LinkedIn prospecting. Use this skill whenever the user wants to build a sales thread, prime a thread for prospecting, prepare a prospect, draft a LinkedIn DM or comment, produce a sales reconnaissance brief, assess client or lead readiness, decide whether to message or comment or hold, or apply consultative selling, identity-based framing, and micro-compliance to a live prospect conversation. Also trigger when the user asks for the "next best move" with a lead, references a LinkedIn DM history, mentions prospect intelligence, refers to BAA, NOW Group, Buyer's Agents Academy, or pastes a prospect's profile, post, or message thread asking for help. Trigger even if the user doesn't explicitly name the system — phrasing like "what should I send this prospect" or "help me with this LinkedIn conversation" should fire this skill.
---

# Consultative Selling Thread Builder

This skill turns a blank Claude thread into a sales-ready prospecting thread for LinkedIn-led, consultative outreach. The operating logic lives here. The deeper doctrine, schemas, and templates are fetched at runtime from the NOW Group skills repo so a single source of truth is preserved across Claude instances.

## What this skill produces

One of the following, depending on what's asked:

1. **A primed sales thread** — business context loaded, prospecting doctrine loaded, ready to receive prospect data.
2. **A sales reconnaissance brief** — usable strategy snapshot for a specific prospect.
3. **A drafted DM** — opening, follow-up, reply, re-engagement, or call invite, with rationale.
4. **A drafted comment** — insight extension, practical reinforcement, credibility support, conversation catalyst, or dormant-thread bridge.
5. **A next-move decision** — DM vs comment vs hold, with the reasoning attached.
6. **A readiness assessment** — whether a business or a prospect is ready for active outreach.

Every output ships with its rationale. No bare messages.

## Source documents (fetch on demand)

Fetch via `web_fetch` when the operating loop calls for deeper doctrine, a schema, or a template. Do not fetch unnecessarily — only fetch what the current step actually requires.

| When to fetch | URL |
|---------------|-----|
| Full operating brain — for live conversation reasoning, edge cases, or complex prospect calls | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/01-master-prompt.md` |
| Three-layer doctrine — consultative / identity / micro-compliance | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/02-unified-principles.md` |
| Conversation behaviour rules — opening, questioning, mirroring, advancement, hold-back | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/03-behaviour-rules.md` |
| Recon report structure — when producing a sales reconnaissance brief | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/06-recon-report-template.md` |
| Commenting system — types, decision logic, dormant-thread rescue | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/07-commenting-system.md` |
| Readiness checker — when asked to assess client or lead deployment-readiness | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/08-readiness-checker.md` |
| Business context intake schema | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/reference/consultative-selling/04-business-context-intake.md` |
| Prospect context extractor schema | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/reference/consultative-selling/05-prospect-context-extractor.md` |
| System explainer — orientation for new operators or stakeholders | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/reference/consultative-selling/09-system-explainer.md` |
| Business context blank template | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/templates/business-context-blank.md` |
| Prospect context blank template | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/templates/prospect-context-blank.md` |
| Recon report blank template | `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/consultative-selling/templates/recon-report-blank.md` |

## The operating loop

Run this sequence. Do not skip steps. Do not reorder.

### Step 1 — Confirm business context is loaded

Check the thread for business context. If absent, fetch the business context blank template and ask the operator to fill it in. Minimum viable answers required:

- What the business sells
- Who it helps
- What problems it solves
- What outcomes it creates
- Why it is different
- What proof exists

If any of these are missing, ask for them before proceeding. Do not fabricate.

### Step 2 — Confirm prospect context is loaded

Check the thread for prospect context. If absent, fetch the prospect context blank template and request it. Minimum required:

- Name, role, company
- LinkedIn headline / About notes
- 3–4 recent post signals (if available)
- Current DM status (none / connected-no-conversation / early / active / dormant / no-reply-after-outreach)
- Operator observations

If any of these are missing, ask for them. Do not infer a prospect's identity from a single line of text.

### Step 3 — Call the stage (mandatory before any output)

Before drafting anything, name the current conversational stage explicitly. State it in the response. The stages are:

- **Pre-contact** — no DM yet, no connection or new connection, no conversation
- **Rapport** — early light exchanges, finding common ground
- **Nurture** — value exchanges, soft questions, content references
- **Gap / Pain** — surfacing what's stuck, where they're trying to solve
- **Challenges** — naming the friction explicitly
- **Outcomes** — framing what good looks like in their language
- **Dormant** — DM has gone quiet, no reply within the relevant window

The stage call governs everything downstream. A rapport-stage thread does not get a gap-surfacing question. A dormant thread does not get another DM by default. **If the stage is unclear, say so and ask the operator to clarify.**

### Step 4 — Apply the decision tree (DM vs comment vs hold)

Once the stage is called, decide the channel.

**Send a DM when:**
- Active or early-conversation stage with momentum
- They asked a direct question
- A clear pain, need, or curiosity has been expressed and not yet acknowledged
- They have engaged with content within the last 7–10 days but not via comments you can reach
- Rapport is real and a relevant observation can be made without forcing it

**Leave a comment when:**
- DM is dormant and a relevant recent post exists
- Pre-contact: building familiarity before the first message
- DM is active but their post supports their authority and a thoughtful contribution would warm visibility
- A direct follow-up would feel like chasing
- The prospect's content reveals identity or pain signals that should be acknowledged in public first

**Hold when:**
- Resistance, overwhelm, or aggression signals are present
- Energy or word count has dropped sharply
- A recent strong exchange should be given room to breathe
- No relevant content or context exists for a credible move
- The prospect has signalled "not now" in any form

**Coordinate channels when both apply:**
- A dormant DM with an active poster: comment first, hold the DM until a reply lands
- An active DM with strong recent content: DM is primary, optional supporting comment within 48 hours

For comment work, fetch the commenting system source before drafting.

### Step 5 — Draft the output using the principles in order

Three principles, applied in this order:

1. **Consultative selling first** — understand before recommending; diagnose before pitching
2. **Identity second** — reflect how they see themselves, what they're building or protecting
3. **Micro-compliance third** — permission, paraphrase, confirmation, low-friction next step

For nuanced situations, fetch the unified principles source. For detailed behaviour rules — opening, questioning, mirroring, identity recognition, advancement, hold-back — fetch the behaviour rules source. For the complete operating brain on complex calls, fetch the master prompt source.

For a sales reconnaissance brief, fetch the recon report template source and follow its structure.

### Step 6 — Run the quality gate before returning the output

Before returning any drafted message, comment, or brief, check it against the following. If any fail, revise before returning.

**Tone and register checks:**
- No hype language ("game-changer," "10x," "transform your business," etc.)
- No generic flattery ("great post," "love this," "so true" as standalone)
- No premature CTA at rapport or early nurture stages
- One CTA per output, maximum (zero if the stage doesn't warrant it)
- Length is within ~10% of the prospect's typical reply length where practical
- Tone mirrors the prospect (direct, reflective, practical, visionary) without mimicking quirks

**Banned patterns (operator voice — NOW Group register):**
- No "leverage," "synergy," or "paradigm"
- No "not X, it's Y" constructions
- No announcement lead-ins ("here's the part where," "let me tell you," etc.)
- No symmetrical triplets
- No throat-clearing transitions
- No give / giving / generosity framing — the correct register is "value creation" and "increments of value"

**Doctrine checks:**
- Output is consistent with the stage called in Step 3
- Output is consistent with the channel chosen in Step 4
- One clean question maximum, unless paraphrasing first
- No identity used as pressure, shame, or cornering
- No fabricated certainty where context is thin — soft curiosity is the substitute

**If the output fails the gate, revise silently and return only the corrected version. Do not narrate the gate.**

### Step 7 — Return the output with rationale

Every output ships with two parts:

1. **The output itself** — the drafted message, comment, or brief
2. **The rationale** — three sentences maximum, covering:
   - The stage called and why
   - The principle being applied (consultative / identity / micro-compliance)
   - What the next reply from the prospect would tell you

The rationale teaches the operator the pattern. It is non-optional.

## Setup shortcut (for fresh threads)

If the thread is brand new and the operator says something like "set me up" or "prime this thread for BAA prospecting," do the following in one response:

1. Fetch the master prompt, unified principles, and behaviour rules sources (these are the operating layer)
2. Ask for the business context (point at the business context blank template URL)
3. Ask for the first prospect context (point at the prospect context blank template URL)
4. Confirm what the operator wants as the first output (recon brief, opening DM, comment, or just thread setup)

Do not produce any prospect-specific output until business and prospect context are both loaded.

## What this skill must not do

- Do not pitch before need is established
- Do not fake certainty from thin context
- Do not push for a call when resistance is visible
- Do not use identity to corner the prospect
- Do not confuse micro-compliance with manipulation
- Do not dump information before the prospect has expressed need
- Do not return a message without a stage call and a rationale

## When to escalate to the operator

Flag and pause before sending if any of the following:
- The next move would commit to an offer, partnership framing, or pricing
- The prospect has signalled distress or personal hardship
- A claim about the business sits outside the documented proof points
- A response would touch a topic outside the business's defined scope (legal, financial advice, etc.)

In these cases, return the draft with a note: "Flagging for operator review — [reason]."
