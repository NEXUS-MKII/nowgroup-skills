# `generate_FAL_images.py` — build spec for Claude Code

**Role:** the single Fal.AI driver for the NOW Group content system. It is the
one place that knows *how* to call Fal. Two different callers invoke it with
different arguments — they never re-describe the Fal call themselves:

- **dna-x-build-a (build-time):** generates the pack's baseline images and writes
  their URLs back into the Excel manifest.
- **now-group-fal-batch skill (on-demand fallback):** generates a batch of bonus
  images into a client's image-bank folder, no manifest write-back.

Because both share this script, **the script owns the Fal contract.** If the Fal
endpoint, auth, model, or response shape ever changes, it changes here once and
both callers inherit it. Do not let either caller hardcode Fal details.

Repo home: same repo the rest of the NOW build system lives in (`NEXUS-MKII/…`).

---

## Environment

- `FAL_KEY` — read from environment, never hardcoded, never logged. (Fal reads
  this variable name by convention; keep it.)
- Google Drive credentials — reuse whatever the repo already uses for Drive
  writes in the build system. Do not invent a second Drive auth path.

## The Fal call (verified against Fal docs)

```
POST https://fal.run/<model_id>
Headers:
  Authorization: Key <FAL_KEY>
  Content-Type: application/json
Body: { "prompt": "<prompt text>", ...optional model params }
Response: image URL nested at  images[0].url
```

- Default `model_id`: `fal-ai/flux/schnell` (fast, low-cost, good for volume).
  Make it a CLI/config arg with that default so the model can be swapped
  (e.g. `fal-ai/nano-banana-2`) without touching code.
- Fal supports a synchronous call (above) and an async queue call. Start
  synchronous — simpler, fine for batches of 6–12. Only move to the queue
  pattern if you hit timeouts at higher volume.
- One prompt → one image by default. Do not silently generate multiples per
  prompt.

## CLI interface

```
python generate_FAL_images.py \
  --prompts <path-or-inline>     # source of prompts (see below)
  --count <int>                  # how many images to generate
  --out <drive_folder>           # where images land in Drive
  --manifest <path>              # OPTIONAL — if given, write fal_url back per row
  --model <model_id>             # OPTIONAL — defaults to fal-ai/flux/schnell
  --name-prefix <str>            # OPTIONAL — filename prefix, e.g. post_id or "bonus"
```

**Prompt source (`--prompts`):**
- When called by dna-x-build-a: point at the Excel manifest; read the
  `image_prompt` column for rows whose `asset_type` is `copy_post_image` (and the
  fallback prompt on `video` rows if you want video-fallback images pre-made).
- When called by the fallback skill: accept a small list of prompts (inline or a
  simple file) — no manifest required.

## Output contract

For each generated image:
1. Save to the `--out` Drive folder.
2. **Filename:** `<name-prefix>_<index>.<ext>` — e.g. `B2_01.png` when the prefix
   is the post_id (build-time), or `bonus_01.png` … `bonus_06.png` (fallback).
   The scheduler and the VA rely on the filename tying an image to its post, so
   the prefix must be the `post_id` for build-time runs.
3. If `--manifest` was given, write the resulting image URL (or Drive path) into
   that row's `fal_url` column. Match rows by `post_id`. Never overwrite a row
   that already has a non-empty `fal_url` unless `--force` is passed — reruns
   shouldn't silently clobber a curated image.

## Behaviour rules

- **Idempotent-ish:** a rerun without `--force` skips rows/prompts already
  satisfied, so re-invoking after a partial failure doesn't double-spend Fal.
- **Cost visibility:** before generating, print the count and the model so the
  caller (and cost) is explicit. `N images × model` — no silent spends.
- **Fail loud, continue where safe:** if one prompt fails (Fal error, timeout),
  log which prompt/post_id failed, keep going, and report the failed set at the
  end. Never write a broken/empty URL into the manifest.
- **Never log the key.** Never echo `FAL_KEY` in output or error traces.

## What this script does NOT do

- Does not post anything to LinkedIn. It only makes images and writes files/URLs.
- Does not decide *which* posts get images — that's the manifest's `asset_type`
  (build-time) or the caller's prompt list (fallback).
- Does not write image prompts — those come from dna-x-build-a's authoring step.
- Does not hold LinkedIn or Make credentials. Fal + Drive only.
