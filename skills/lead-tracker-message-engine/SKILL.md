---
name: lead-tracker-message-engine
description: >-
  Walks a team member (Earl, or any rep) through working leads end-to-end using
  the Lead Tracker and the Message Engine together: pick a lead off the tracker,
  read their context, write the touch in the Message Engine (right channel, CTA
  and variant), send it, then log it back on the tracker. Use whenever someone
  asks how to work the leads, "who do I message next", "how do I use the message
  engine", "which variant do I send", "how do I write this lead a follow-up",
  "where do I log what I did", or is onboarding to the MyFuture lead follow-up
  workflow. Covers the MyFuture / Lara + ScoreApp pair; the TMT pair follows the
  same loop.
---

> v2026-10-12.1 · source-of-truth: `nowgroup-skills/skills/lead-tracker-message-engine/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.

# Working leads: the Lead Tracker + Message Engine, in unison

Two tools, one loop. The **Lead Tracker** tells you *who* to work and holds what
we know about them. The **Message Engine** writes the *touch* in the right voice.
You move between them: pick a lead on the tracker, write their message in the
engine, send it, then log it back on the tracker. Never one without the other.

## The two tools (MyFuture)

- **Lead Tracker** — `https://claude.ai/artifact/BU7a1iyQgCrmx3yLSDZFhP`
  Every lead, segmented by what they asked for, with their status, next step,
  notes and their ScoreApp **continue link**. This is the source of truth for
  who to work next and where each lead is up to.
- **Message Engine (Lara)** — `https://claude.ai/artifact/G46wVFngsYDZ9ZinKwKdgi`
  Writes the email or DM in Lara's voice from a lead's ScoreApp context. First
  time you hit **Write** it asks you to allow Claude for the page — say yes.

Open both in tabs. Keep the lead data private.

## The loop (do it in this order, every lead)

1. **Pick a lead on the tracker.** Work the hottest segment first (Ready for
   advice, then Blocked on cost, Wants a person, Self-directed; Abandoned = the
   finish-nudge pile). Click the lead to open their row.
2. **Read them before you write.** Their position, 90-day goal, obstacle, support
   preference, their own words (the free-text note), and whether they have a
   **continue link** (incomplete scorecard). Thirty seconds here makes the
   message land.
3. **Open the Message Engine and load the context.** Paste what the tracker shows
   for that lead into **ScoreApp context**. Fill **First name**, **Completion %**,
   and, for a finish-nudge, paste their **continue link** (it must come from
   ScoreApp — never make one up).
4. **Set the controls:**
   - **Channel** — Email (Lara) or LinkedIn DM (Rob).
   - **The ask (CTA)** — Finish the scorecard (for incompletes) · Quick chat with
     Lara · Early access to the app · Value-only nudge. One ask, matched to where
     they are.
   - **Segment** — leave on Auto-read, or set it if you know it.
   - **Rapport** — warm-light / balanced / high.
5. **Write, then choose the variant.** The engine gives three:
   - **A — good news first.** The default. Warm, leads on what they did well.
   - **B — mirror first.** When the thing stopping them is a *belief* ("advice is
     for people with more than me", "should have this sorted"). Opens by naming
     it, then turns it over.
   - **C — pain-led (now / later).** When they read keen or urgent: opens on what
     is honestly at stake — the cost now, where they'll be later if nothing
     changes, or both. Use it for people who are ready to move, not for cool or
     early leads (send A or B there).
   Read all three. Pick the one that fits this person. Check the **quality gate**
   is green.
6. **Read it like the recipient would, then send/copy.** If anything reads
   generic or off, regenerate or tweak the context and rewrite. Copy the variant
   you chose. Email goes out with the proper branded **signature** (never a bare,
   signature-less email — that reads as spam). DMs go to Rob to send.
7. **Log it back on the tracker.** On the lead's row: write what you sent in
   **notes**, set the **next step** and a **date**, and move the **status**.
   A touch that isn't logged didn't happen.

## Picking the variant — quick rule

| The lead is… | Send |
|---|---|
| Warm, doing some things right | **A** (good news) |
| Held back by a belief / assumption | **B** (mirror) |
| Keen, urgent, ready to move | **C** (pain-led) |
| Cool, early, no real stake yet | **A**, or a value-only nudge — never C |

C names a *real* cost honestly. It never fear-mongers, pressures, or invents
stakes — that breaks Lara's voice and the lead's trust.

## Rules that keep this working

- **One ask per message.** The engine enforces it; don't add a second.
- **Never invent a continue link, a number, or a fact about the lead.** Thin
  context = a shorter message, not a made-up one.
- **Every email carries a proper signature.** No exceptions.
- **Always log back.** The tracker is only useful if it's current — it's how we
  know who to work next and what's already been said.

## The TMT version (same loop)

For Aaron's Tradies Mentoring Tradies leads the loop is identical, with the TMT
tools and Aaron's voice (tradie-to-tradie, pain-first, "Stay Solid"). Once the
engine + tracker are folded into AUBIT, you'll run the same loop from inside the
AUBIT **Contacts** workspace at `crm.tmtquiz.com.au` instead of separate pages —
see `aubit-tmt/docs/plans/fold_message_engine_into_aubit.md`.
