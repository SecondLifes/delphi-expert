# Five-Lens Self-Audit — delphi-expert

> Combines the Prompt Engineer & Analyst, Repo Auditor, DevOps/Config
> Engineer, Systems Forensics Analyst, and Context Engineer lenses
> (`rad-prompt-engineer`). Performed 2026-07-19 as part of migrating this
> kit — a fork of [delphicleancode/delphi-spec-kit](https://github.com/delphicleancode/delphi-spec-kit)
> that predates this workspace's own template standard — up to the
> structural parity already established by `batch-script-expert` and
> `prompt-analyzer-expert`, **without disturbing the kit's own 20+
> Delphi/framework-specific skills or its original MIT license/copyright**
> (both explicit, user-confirmed constraints for this migration — see
> `CONTRIBUTING.md`'s "Provenance" section).

## Scope of this migration

What changed:

- Added `## Identity`, `## Skill Check (Mandatory)`, `## Working
  Directory`, `## Proactive Quality Suggestions (Mandatory Closing Step)`
  to all four primary files (`AGENTS.md`, `.claude/CLAUDE.md`,
  `.gemini/rules/project-rules.md`, `.github/copilot-instructions.md`).
- Renamed `.gemini/rules/delphi-rules.md` → `.gemini/rules/project-rules.md`
  for cross-kit naming consistency, updating every internal reference
  (`AGENTS.md`, `.agents/rules/sync-workflow.md`, `README.md`,
  `README.tr-TR.md`, `docs/proje-haritasi.md`, `docs/ai-ignore-strategy.md`).
- Replaced `.agents/skills/rad-prompter/` (an early, unmaintained ancestor
  of this workspace's `rad-prompt-engineer`) with the current
  `rad-prompt-engineer` bundle, plus `rad-skill-finder`, `rad-python`, and
  `rad-web-scraping` — the same four-skill bundle every `spec-kits/*`
  template built from `blank-scaffold` now ships. `rad-repo-scaffold` and
  all 20 Delphi/framework-specific skills were left untouched, per explicit
  user instruction.
- Added the missing root docs (`CODE_OF_CONDUCT.md`, `CONTRIBUTING.md` /
  `.tr-TR`, `SECURITY.md` / `.tr-TR`, `README.tr-TR.md`) and `src/` as the
  default AI-generated-output root.
- Rewrote `docs/proje-haritasi.md` for the new structure; updated
  `docs/ai-ignore-strategy.md`'s two stale path references.
- Replaced `tools/generate-ai-configs.ps1` with the version fixed earlier
  this session in `batch-script-expert` (copy-before-delete ordering,
  empty-source-yields-mass-deletion guard) — the file has no kit-specific
  content, so this was a straight file copy, then re-run and verified clean
  (`docs/proje-haritasi.md coverage: OK`).
- Removed three IDE/build-artifact files (`I18nApp.dproj.local`,
  `I18nApp.identcache`, `I18nApp.res`) from `examples/i18n-app/` — user
  confirmed after being told these files aren't under any git tracking
  (no `.git` exists at or above this kit) and directly contradict the
  kit's own `AGENTS.md`/`.gitignore`/`.cursorignore` "never use as
  context" list.
- **Deliberately not touched:** `LICENSE` (MIT, `Delphi Clean Code`
  copyright), the README's Sponsorship/Pix-donation section, and all
  content under the 20 Delphi/framework-specific skill folders.

## Lens 1 — Prompt Engineer & Analyst

- All four primary files now state the same Identity/Skill
  Check/Working Directory/Proactive Quality Suggestions rules, reworded
  (not copy-pasted verbatim) to fit each file's format — verified by
  reading each file in full after editing, not just grepping for section
  titles.
- No contradiction introduced: the pre-existing "Memory Management
  (Critical)" and "Anti-Patterns to Avoid" sections in `AGENTS.md` already
  said the same thing the new Identity paragraph now states up front
  (`try..finally` immediately after `.Create`) — the new section
  reinforces rather than duplicates conflicting guidance.
- `.kiro/steering/*.md` (4 files) were intentionally **not** given the
  four sections — this matches `sync-workflow.md`'s own statement that
  Kiro steering docs are "a different concept... intentionally out of this
  sync scheme," and matches the precedent set in `batch-script-expert` and
  `prompt-analyzer-expert`, where only the four primary files got this
  treatment.

## Lens 2 — Repo Auditor

- Verified every skill-folder claim in `docs/proje-haritasi.md` against
  the actual `.agents/skills/` listing (`ls` — 27 folders) — count and
  names match exactly, including the four newly bundled skills and the
  removal of `rad-prompter`.
- Verified `.claude/rules/` and `.cursor/rules/` are byte-identical to
  `.agents/rules/` via `diff -rq` after running the generator — zero
  differences.
- Verified `LICENSE` was not modified (`head -5` still reads `Copyright
  (c) 2026 Delphi Clean Code`, MIT) — the one file this migration was
  explicitly instructed to leave alone.
- Verified no stale `.gemini/rules/delphi-rules.md` or `rad-prompter`
  references remain anywhere except the two files where they're
  intentional historical notes (`docs/proje-haritasi.md`'s "yerini aldı"
  entries, written as history, not as a live pointer).

## Lens 3 — DevOps/Config Engineer

- `tools/generate-ai-configs.ps1` ran clean end-to-end: synced 16 rule
  files to both `.claude/rules/` and `.cursor/rules/`, synced `review.md`
  to `.claude/commands/`, generated 27 skill-command wrappers (one per
  `.agents/skills/*` folder, correctly excluding the removed
  `rad-prompter`), and reported `docs/proje-haritasi.md coverage: OK`.
- `.claude/settings.json`'s `allowCommands` (`boss *`, `msbuild *`, the
  generator script) is unchanged and still correct for this stack — unlike
  `batch-script-expert`'s original copy, this kit's settings.json was
  never Delphi-*inappropriate*, so nothing needed removing here.
- Confirmed none of the four newly bundled skills (`rad-prompt-engineer`,
  `rad-skill-finder`, `rad-python`, `rad-web-scraping`) carry a hardcoded
  `E:\system\dev\AI` or `E:/system/dev/AI` absolute path — grepped all
  four folders, zero matches, consistent with them being the already-
  verified-portable variants from `blank-scaffold`.

## Lens 4 — Systems Forensics Analyst

- Root-caused the fork-provenance signal (MIT license naming `Delphi
  Clean Code`, a Brazilian sponsorship link, Portuguese-language content
  and ACBr/Brazilian-tax-specific framework coverage) *before* making any
  LICENSE change — asked the user rather than assuming this kit followed
  the same "originated in this workspace" pattern as `batch-script-expert`
  and `prompt-analyzer-expert`. This changed the actual migration scope
  (license/attribution preserved) rather than being a cosmetic footnote.
- The three removed IDE-artifact files in `examples/i18n-app/` were
  traced to their cause: this kit (unlike the other two, which were built
  fresh from `blank-scaffold`) was cloned from an external repository that
  had already accumulated local IDE build residue before this workspace
  ever touched it — not a new mistake introduced during this migration.
- No other Delphi-origin/foreign-stack residue found in the newly added
  files (`CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`/`.tr-TR`,
  `SECURITY.md`/`.tr-TR`, `README.tr-TR.md`, `src/README.md`) — these were
  authored fresh against this kit's actual stack, not copy-pasted from a
  different-language sibling without adaptation.

## Lens 5 — Context Engineer

- Checked the 4 newly added skills' frontmatter (`name`, `description`)
  against the 23 pre-existing skill names for trigger overlap — no
  collision: `code-review` (Delphi code review checklist) and
  `rad-prompt-engineer` (prompt/rule/skill review) cover genuinely
  different domains despite both involving "review."
- `AGENTS.md`'s header note already instructed every reader that
  referencing `rad-prompt-engineer` by name/folder/matching-request loads
  all five lens files at once — this note was carried over unchanged from
  the `batch-script-expert`/`prompt-analyzer-expert` pattern, so the
  trigger mechanism is consistent workspace-wide.
- Skill discovery for the 20 Delphi-specific skills is unaffected by this
  migration — their own `SKILL.md` frontmatter was not touched, so
  whatever trigger precision they already had is preserved as-is.

## Addendum — rad-powershell-master added (same day, follow-up request)

The user asked to also bundle `rad-powershell-master` (source:
`josiahsiegel/claude-plugin-marketplace`, upstream name
`powershell-master`) — not part of `blank-scaffold`'s default 4-skill
bundle, added specifically for this kit. Copied as-is from the
workspace's own `.agents/skills/rad-powershell-master/` (grepped for
hardcoded `E:\system\dev\AI` paths first — none found, unlike some
earlier workspace-tooling files that needed cleanup before being
portable). Skill count is now 28 (22 Delphi-specific + 6 general-purpose:
`rad-repo-scaffold`, `rad-prompt-engineer`, `rad-skill-finder`,
`rad-python`, `rad-web-scraping`, `rad-powershell-master`) —
`docs/proje-haritasi.md`, `README.md`, `README.tr-TR.md`, and
`.gemini/rules/project-rules.md` all updated to match. Re-ran
`tools/generate-ai-configs.ps1` twice: once produced a coverage
`WARNING` for the un-documented skill (caught before this turn ended,
not silently ignored), the second run after fixing
`docs/proje-haritasi.md` reported clean.

## Verdict

**Migration complete for its stated, user-confirmed scope**: structural
parity (Identity/Skill Check/Working Directory/Proactive Quality
Suggestions, bilingual root docs, `src/`, the standard 5-skill bundle
including `rad-powershell-master`, fixed generator script) reached
without touching the 20 Delphi-specific skills, `rad-repo-scaffold`, or
the original MIT license/copyright — all verified by direct inspection
(`diff`, `grep`, actually re-running the generator script) rather than
assumed correct from having written it.

**Not in scope for this pass** (flagged, not fixed, since they fall
outside what was asked): the 20 Delphi-specific skills and their
`references/` files were not independently re-audited for internal
accuracy — this migration's mandate was structural parity, not a content
review of framework-specific guidance the user explicitly asked not to be
touched. A future, separately-scoped audit could apply the five lenses to
that content on its own terms.
