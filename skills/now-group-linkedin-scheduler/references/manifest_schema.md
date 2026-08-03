# Content Manifest Schema (`content_manifest.xlsx`)

The manifest is the **contract** between `dna-x-build-a` (which writes it) and
`now-group-linkedin-scheduler` (which reads and updates it). Get the columns
wrong and both skills break at the seam. This file is the single source of truth
for that schema. Both skills reference it.

## Grain

**One row per post.** Not one row per slide. A carousel is a single row with
`asset_type = carousel`; its slides live inside the Gamma export the row points
at. This keeps "one post = one row" intact and lets the scheduler filter/route
without unpacking multi-slide structures.

## Columns

| Column | Written by | Type | Notes |
|---|---|---|---|
| `post_id` | build | string | Stable unique ID, e.g. `A1`, `B3`, `carousel_A`, `infog_B`, `video_C2`. |
| `week` | build | int | Which week of the pack. Written for human reference / override only — the scheduler derives "due" from `scheduled_date`, not this. |
| `day` | build | string | Optional intra-week slot label, e.g. `Mon`, `Thu`. Human reference. |
| `asset_type` | build | enum | One of: `article`, `copy_post`, `copy_post_image`, `carousel`, `infographic`, `video`. Drives routing. |
| `post_copy` | build | text | The approved LinkedIn copy. Posted exactly, never rewritten. |
| `image_prompt` | build | text | Fal.AI prompt. **Required for `copy_post_image`.** Also present (as a fallback) on `video` rows so a missing video can degrade to an image post. Empty for other types. |
| `asset_path` | build / operator | string | Location of a pre-rendered asset for HUNT types. Drive path/URL for infographic & video; may hold the Gamma URL for carousel. Empty for TEXT/GENERATE. |
| `gamma_url` | build | string | Gamma export URL for `carousel` (and `infographic` if Gamma-built). Preferred over `asset_path` for Gamma assets. |
| `scheduled_date` | build | date (YYYY-MM-DD) | **The field the scheduler trusts.** Determines what's due. |
| `scheduled_time` | build | time (HH:MM) | Local post time. |
| `timezone` | build | string | IANA tz, e.g. `Pacific/Auckland`. Never guessed. |
| `account` | build | string | LinkedIn account/page identifier. |
| `status` | build → scheduler | enum | `pending` (build default) → `scheduled` / `posted` (scheduler) → `skipped` / `failed` (scheduler). The scheduler filters out `scheduled`/`posted` when deriving "due". |
| `fal_url` | build (`generate_FAL_images.py`) | string | Fal.AI image URL for `copy_post_image` rows, generated and written at build-time. The scheduler reads it to link the image for the media-attach pass; it does not generate images. |
| `linkedin_id` | scheduler | string | Returned post/share ID from Make, if available. |

## Routing map (must match the scheduler's §3)

| `asset_type` | Route |
|---|---|
| `article`, `copy_post` | TEXT |
| `copy_post_image` | GENERATE (Fal.AI via Make) |
| `carousel`, `infographic`, `video` | HUNT (pre-rendered) |

## Status lifecycle

```
pending ──(scheduler schedules)──> scheduled ──(posts)──> posted
   │
   ├─(user skips)──> skipped
   └─(error/missing asset)──> failed
```

The next run derives "due" as: `scheduled_date` in window AND `status` ∈ {pending}.
This is why write-back matters — a row left at `pending` after posting will
double-post next run.

## Minimal example (column header + two rows, shown as CSV for readability — delivered as .xlsx)

```csv
post_id,week,day,asset_type,post_copy,image_prompt,asset_path,gamma_url,scheduled_date,scheduled_time,timezone,account,status,fal_url,linkedin_id
B2,2,Wed,copy_post_image,"Most partnerships don't break at the start...","Editorial photo, two gears meshing at a right angle, warm industrial light, no text",,, 2026-07-15,08:00,Pacific/Auckland,now-group-page,pending,,
carousel_A,1,Thu,carousel,"Swipe → the four seams every partnership hides.",,,"https://gamma.app/docs/xxxx",2026-07-10,08:00,Pacific/Auckland,now-group-page,pending,,
```
