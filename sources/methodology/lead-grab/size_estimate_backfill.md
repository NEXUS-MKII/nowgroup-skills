# Size-Estimate Backfill — Manual via Claude.ai

**Use case**: Adding `size_estimate` + `size_signals` columns to lead lists that were generated BEFORE the engine size-estimate commit shipped (NOW Group `baa-booking-test` commit `bea2f0b`, 2026-06-06). Runs against existing sheet data, no engine re-run needed, zero Apify spend.

**Tool**: Claude.ai web app (Sonnet 4.6 or Opus 4.7 recommended — better reasoning than Haiku on edge cases).

**Cost**: Free (uses Claude.ai subscription).

**Workflow**: Export → paste into Claude.ai → paste classified output back into sheet. ~5 min per 100-200 rows.

**Cross-references**:
- `feedback_digital_presence_as_maturity_spine.md` (memory) — why size signals matter per ICP
- `feedback_discovery_typology_framework.md` (memory) — how size fits the broader typology framework
- `project_enrichment_engine.md` (memory) — the engine that auto-computes this column for NEW runs

---

## Step 1 — Extract the columns Claude needs from your sheet

From any lead-grab sheet tab (`master` / `_hot` / `_warm` / `_cold`), select and copy these columns for the rows you want classified:

| Column | Why it matters |
|---|---|
| `business_name` | Primary signal — names like "WSP" vs "Hadley Consultants" vs "Bob's Plumbing" are unmistakable size signals |
| `website` | URL pattern is huge — `company.com/en-NZ/offices/...` = multinational branch; `.co.nz` / `.com.au` = local |
| `category` | Context for what "normal" size looks like (a plumber at 100 staff is enterprise; an engineering firm at 100 staff is mid) |
| `city` / `region` / `country` | Location size signal |
| `general_phone` / `general_email` | Pattern check — generic `info@` or named exec @ small domain |
| `fb_followers` | Scale: <1K micro/small, 10K mid, 100K+ enterprise |
| `ig_followers` | Same |
| `review_count` | <50 micro/small, 500+ mid/enterprise |

Copy as tab-separated values (Google Sheets: select range → Cmd-C copies as TSV).

**Batch size**: 100-200 rows per Claude.ai prompt (keeps response under output token limits).

---

## Step 2 — Paste this prompt into Claude.ai, followed by your TSV

````
You are a sales-intelligence analyst. For each business row below, infer COMPANY SIZE from the signals provided.

Output ONE of these size buckets per row:
- micro      = 1-5 staff (solo, partnership, small local operator)
- small      = 5-20 staff (classic SME, single location)
- mid        = 20-100 staff (multi-location, established mid-market)
- enterprise = 100+ staff OR multinational subsidiary / corporate branch
- unknown    = insufficient signal

Read these signals in order of strength:
1. Business name — "WSP Queenstown" or "BDO Auckland" signals multinational branch; "Hadley Consultants Limited" signals local SME; "Bob's Plumbing" signals micro
2. URL pattern — corporate subdomain like `company.com/en-NZ/offices/...` signals multinational; local TLD `.co.nz` or `.com.au` signals local/SME
3. Review count — <50 = micro/small, 50-500 = small/mid, 500+ = mid/enterprise
4. Social follower scale — <1K = micro, 1-10K = small, 10-100K = mid, 100K+ = enterprise marketing
5. Multi-location mentions in name (e.g. "Auckland | Wellington | Christchurch") = mid+
6. Category context — adjust thresholds by vertical (an accounting firm with 50 staff is mid; a plumber with 50 staff is mid+)

For each row also write a ≤12-word "size_signals" evidence trail explaining your call.

INPUT FORMAT: tab-separated, first row is header.
OUTPUT FORMAT: tab-separated, exactly these two columns, no header, one row per input (preserve input order):
size_estimate<TAB>size_signals

If a row has insufficient signal, output "unknown" with reason ("no website, low review count" etc.).

Here's the data:

<paste your TSV here>
````

---

## Step 3 — Paste Claude's output back into your sheet

Claude returns TSV: `size_estimate<TAB>size_signals` per row, in input order.

In Google Sheets:
1. Add two new columns to the right of your data: `size_estimate` and `size_signals`
2. Click the first cell of the new `size_estimate` column at the correct row
3. Paste — Sheets auto-splits the TSV across both columns

If you batched (e.g. rows 1-200, 201-400), paste each batch at the matching starting row.

---

## Step 4 — Use the new columns

| Client | Filter |
|---|---|
| **Marksmen** (inverted ICP — wants high digital maturity SMEs) | `_cold` tab × `size_estimate IN ('small','mid')` = primary outreach (drops multinationals + micro operators) |
| **Aaron / AUBIT-TMT** | `size_estimate='small'` = Demo A (profit wedge); `size_estimate='mid'` = Demo B (compliance wedge) |
| **NOW Group** (Chris's own consultative networking) | `size_estimate IN ('small','mid')` = ideal builder profile |
| **Farhad / Dream Websites** | Optional flag `size_estimate='enterprise'` = "too big, partner referral candidate" |
| **Any future default-ICP client** | `size_estimate IN ('small','mid')` = standard primary band |

---

## Limits + caveats

- **Claude.ai paste limit**: roughly handles 100-200 rows comfortably per prompt. Beyond that, output may truncate — batch smaller.
- **Response cleanup**: Claude.ai response sometimes adds prose preamble — strip everything except the TSV lines before pasting back to Sheets.
- **Sparse-signal rows**: where the website wasn't successfully scraped (no website column or scraper failed), Claude may return `unknown` more often — that's correct behaviour, not error.
- **Model choice**: Sonnet 4.6 is the recommended default. Opus 4.7 is more accurate but slower; use for high-stakes batches (e.g. NOW Group accountants where mis-classification costs a partner referral).

---

## When you'd skip this and just restart the runner instead

If the campaign has many sub-runs remaining (e.g. Marksmen's 14 with most still unrun), restarting with new engine code is cleaner — every NEW sub-run gets `size_estimate` automatically. Only backfill the runs that ALREADY completed before the engine fix shipped.

Decision tree:
- Sub-run already complete + low-friction to backfill → use this methodology
- Sub-run not yet started → wait for runner restart (engine adds the column natively)
- Sub-run currently in flight → let it complete, then backfill via this methodology
- Future cycle, fresh start → engine handles automatically, this methodology not needed

---

## How to invoke this methodology from any Claude.ai conversation

Fetch this file's raw URL into a Claude.ai conversation:

```
https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/sources/methodology/lead-grab/size_estimate_backfill.md
```

Then paste the prompt from Step 2 followed by your TSV data. Claude reads this methodology + your data and returns classified rows in one pass.

---

*Last updated 2026-06-06 · Companion to `baa-booking-test` commit `bea2f0b` (Claude synthesis: add size_estimate + size_signals columns)*
