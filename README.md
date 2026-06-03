# NOW Group · Shared Claude Skill Library

Cross-instance Claude skill library. Skills + source instruction docs that any Claude instance can fetch — Claude Code CLI, Claude.ai web, Claude desktop, IDE extensions, or shared with collaborators.

## Why this exists

Claude skills are markdown files with instructions. They work brilliantly *per-instance*, but each instance manages its own skills independently — drift is inevitable. This repo solves that by giving every Claude instance the same fetch URL.

The pattern:

```
1. Install a skill once in any Claude instance (paste the skill.md)
2. The skill's body instructs Claude to fetch source docs from this repo
   via raw.githubusercontent.com URLs
3. Edit the source docs here → every Claude instance picks up the new
   version on next invocation. No re-install needed.
```

Single source of truth, automatic sync, no per-instance drift.

## What's in here

| Directory | Purpose |
|---|---|
| `skills/` | Installable Claude skill markdown files. Paste these into Settings → Skills in any Claude instance. |
| `sources/methodology/` | How-to docs, workflow patterns, process specs that skills reference at runtime |
| `sources/voice/` | Voice / style / tone references (general — not buyer-specific) |
| `sources/reference/` | General reference material (frameworks, lookup tables, glossaries) |
| `_archive/` | Deprecated versions, kept for provenance |

## Scope vs `dna-x-delivered`

Two repos, clean boundary:

- **`dna-x-delivered`** — the DNA-X product suite. Client deliverables, DNA-X build specs, DNA-X skills. Product-scoped.
- **`nowgroup-skills`** (this repo) — everything else. General skills, cross-cutting methodology, reference docs. Meta-scoped.

If a `nowgroup-skills` skill needs DNA-X-specific reference material, it should fetch from `dna-x-delivered` rather than duplicate it.

## Install a skill in Claude app

1. Open the raw markdown URL for the skill, e.g.
   `https://raw.githubusercontent.com/NEXUS-MKII/nowgroup-skills/main/skills/<skill-name>.md`
2. Copy the full file contents (frontmatter + body)
3. Claude app → **Settings → Skills → Create new skill** → paste → save
4. Invoke from any conversation with `/<skill-name>`

## Add a new skill

```bash
cd "/Users/christopherwhite/Applications/Claude Projects/nowgroup-skills"
# write the skill at skills/<name>.md (frontmatter: name + description + body)
# write any source docs the skill fetches at sources/<category>/<doc>.md
git add skills/<name>.md sources/<category>/<doc>.md
git -c user.email="chris@nowgroup.co.nz" -c user.name="Chris White" commit -m "Add <name> skill + sources"
git push
```

The skill is immediately installable via the raw URL pattern above.

## Visibility + sensitive content rules

Public repo. Anything pushed here is world-readable.

**Safe to publish**:
- Skill instruction files
- Methodology specs, schemas, taxonomies
- Voice/style references that are general (not derived from any specific buyer's data)
- Workflow patterns, frameworks, glossaries

**Never publish**:
- Voice training corpora (`_voice_corpus_chris/`, `_voice_corpus_di/` etc — these are real writing samples)
- Per-buyer Voice Parameter Blocks (contractually buyer-owned)
- API keys, tokens, OAuth secrets, internal credentials
- Internal pricing not yet on public marketing pages
- Buyer PII, contracts, financials

Run a pre-push scan whenever you add new content:

```bash
grep -irE "(password|api[_-]?key|secret|token|@[a-z0-9.-]+\.(com|co\.nz|org)|\+64)" skills/ sources/
```

## Future moves (all 1-click)

- **Org transfer** — when a `nowgroup` GitHub org is created, Settings → Transfer ownership. URLs auto-redirect permanently.
- **Custom domain** — if you front this with a subdomain (e.g. `skills.nowgroup.co.nz`), add a `CNAME` file + DNS CNAME → `nexus-mkii.github.io`. Pages auto-issues HTTPS cert.
- **Repo rename** — Settings → Rename. URLs auto-redirect.

All preserve old URLs as permanent redirects — won't break installed skills.

## Skill registry

| Skill | Purpose | Sources fetched |
|---|---|---|
| [consultative-selling-thread-builder](skills/consultative-selling-thread-builder.md) | Spin up a primed sales thread for consultative LinkedIn prospecting — stage call, DM/comment/hold decision tree, quality gate, rationale on every output | `sources/methodology/consultative-selling/*` (6 doctrine docs + 3 templates) + `sources/reference/consultative-selling/*` (3 schemas + system explainer) |
