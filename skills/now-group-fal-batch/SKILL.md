---
name: now-group-fal-batch
description: "Generate a batch of bonus NOW Group images on demand via Fal.AI, as a fallback when a Content Pack Pro build's baseline imagery falls short or a client needs extra visual options. Produces a batch (default 6) of images from supplied prompts and drops them, named, into the client's image-bank Drive folder. Use when the user asks to 'generate bonus images', 'make 6 more images for [client]', 'top up the image bank', 'we need more visual options for [client]', or the content pack's baseline images aren't enough. This does NOT post anything and does NOT run the main content build — it is the standalone image top-up. The baseline 12 images are generated inside dna-x-build-a; this skill is only the on-demand extra batch."
---

> v2026-08-04.1 · source-of-truth: `nowgroup-skills/skills/now-group-fal-batch/SKILL.md` — if the repo copy shows a newer version than this line, this upload is stale: re-package and re-upload it.

# NOW Group — Fal Batch (bonus imagery)

You generate a **batch of bonus images** on demand when a client's Content Pack
Pro imagery needs topping up. This is the fallback lever: the pack ships with 12
baseline images from `dna-x-build-a`; when those fall short — a post's image
misses, a client wants alternatives, a seasonal angle appears — you run a batch
of extras.

You do **not** post anything. You do **not** run the content build. You produce
images and drop them, named, into the client's image-bank folder for the VA to
draw from during the batch attach-pass.

## The engine you call

All Fal generation runs through the repo script **`generate_FAL_images.py`** —
the single Fal driver. You never describe the Fal API call yourself; you invoke
the script. If the Fal endpoint/model/auth ever changes, it changes in the
script, not here. (See `generate_FAL_images_SPEC.md` for the script contract.)

This skill's job is to gather the right inputs and invoke the script correctly —
it is a thin, correct wrapper around a deterministic image step. Image
generation wants to be deterministic; keep the judgement in the prompts, not in
the plumbing.

## Operating loop

### 1. Confirm the batch inputs

- **Client / image-bank folder:** which client, and the Drive folder their image
  bank lives in. If you don't know it, ask — do not guess a path.
- **Prompts:** the batch needs prompts. Three valid sources, in order of
  preference:
  1. The user supplies them.
  2. Draw from the client's existing pack manifest `image_prompt` column (reuse
     of already-authored, on-voice prompts) — good when generating *alternatives*
     to existing posts.
  3. You author them fresh **only** from the client's Voice/brand context and the
     stated need — same anchor discipline as the pack (trace to a hook, framework,
     or brand register; no generic stock concepts, no text-in-image).
- **Count:** default **6**. Confirm if the user wants a different number — every
  image is a Fal spend, so the count is explicit, never silent.

### 2. Show the plan before spending

Print: client · folder · model · count · the prompts. This is the cost gate —
`6 images × <model>` is a real spend and the user sees it before it fires. Wait
for go.

### 3. Invoke the script

```
python generate_FAL_images.py \
  --prompts <supplied or manifest-sourced> \
  --count 6 \
  --out <client image-bank Drive folder> \
  --name-prefix bonus
```

- **No `--manifest`** — bonus images are not tied to specific posts, so there is
  no write-back. They land in the bank as `bonus_01…bonus_06` for the VA to pick
  from. (If the user explicitly wants a batch tied to specific posts, pass the
  post_ids as the prefix and, only then, `--manifest` for write-back.)
- Default model `fal-ai/flux/schnell`; pass `--model` only if the user wants a
  different look (e.g. `fal-ai/nano-banana-2`).

### 4. Report

```
Batch complete.
Client: <name>
Folder: <drive path/URL>
Model: <model>
Generated: <n> (named bonus_01 … bonus_0n)
Failed prompts: <any, listed>
```

Point the user/VA at the folder. Done.

## What this skill must not do

- **Don't post to LinkedIn.** This makes images only.
- **Don't run the content build** — that's `dna-x-build-a`. If the request is
  really "build the pack", route there.
- **Don't re-describe the Fal API** — invoke `generate_FAL_images.py`; the script
  owns the contract.
- **Don't spend silently** — show count × model and wait for go.
- **Don't author off-voice prompts** — trace to the client's brand/voice, no
  generic stock imagery, no text-in-image.
- **Don't write to the manifest** unless the user explicitly wants a
  post-tied batch (then use post_id prefix + `--manifest`).

## Relationship to the rest of the system

- **`dna-x-build-a`** makes the baseline 12 images at build-time (same script,
  manifest write-back on). This skill is the *extra* batch when 12 isn't enough.
- **`generate_FAL_images.py`** is the shared engine both use.
- **`now-group-linkedin-scheduler`** consumes the images (baseline + bonus) at
  posting time — it does not generate them.
