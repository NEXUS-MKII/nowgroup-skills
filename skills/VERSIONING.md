# Skill versioning (standing practice, all NOW build centres)

Every SKILL.md carries a version stamp as the FIRST line after the
frontmatter (never in the title):

    > v<YYYY-MM-DD>.<n> · source-of-truth: `<repo>/skills/<name>/SKILL.md` — ...

Rules:
1. ANY content change to a skill bumps the stamp (new date, or .n+1 for a
   same-day second edit) in the same commit.
2. The repo copy is the source of truth; uploaded .skill files on claude.ai
   are copies. If an upload's stamp is older than the repo's, it is stale —
   re-package (zip of `<name>/SKILL.md`, extension .skill) and re-upload.
3. Claude instances with repo access should compare stamps and flag stale
   uploads; on claude.ai without repo access, the stamp lets a human check
   currency at a glance.
4. Skills sync (including the stamp bump) is part of definition-of-done for
   any UX/behaviour change the skill describes.

Applies to every skills estate: AUBIT (aubit-tmt, aubit-template), NOW/NEXUS
(nowgroup-skills), and DNA-X from its first skill.
