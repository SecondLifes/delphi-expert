# Changelog

All notable changes to the Delphi AI Spec-Kit are documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) ·
Versioning: [SemVer](https://semver.org/) via annotated git tags.

## [Unreleased]

### Fixed

- **Claude Code could not discover any of this kit's skills.** It looks only
  under `.claude/skills/`; `.agents/skills/` is not one of its discovery
  locations, so every skill here was unreachable by trigger matching and only
  worked if the user typed the generated `/<skill-name>` wrapper by hand.
  `tools/generate-ai-configs.ps1` now creates one junction (Windows, no
  elevation needed) / symlink per skill under `.claude/skills/`, pointing back
  at `.agents/skills/`. The links are gitignored and regenerated after a clone,
  so the "symlink degrades on clone" hazard that keeps rules as copies does not
  apply. Verified against `code.claude.com/docs/en/skills`.
- **Cursor ignored every rule in this kit.** `.cursor/rules/` held `.md` files;
  Cursor recognizes only `.mdc` there and silently skips anything else. The
  frontmatter was already correct — only the extension was wrong. The generator
  now writes `.mdc` and sweeps the old `.md` copies instead of leaving both.
  Verified against `cursor.com/docs/rules`.
- **Gemini CLI read nothing.** The AI-tool table pointed it at
  `.gemini/rules/project-rules.md`, but Gemini CLI builds context from the
  `GEMINI.md` hierarchy. Added a root `GEMINI.md` that imports that file rather
  than duplicating it. Verified against `geminicli.com/docs/cli/gemini-md`.
- **`.claude/settings.json` used invented keys.** `allowCommands`/`denyPaths`
  are not Claude Code settings, so the advertised `.env`/`.key` protection and
  the pre-approved generator command never existed. Rewritten to the real
  `permissions.allow`/`permissions.deny` schema.
- Corrected the false claim, repeated across this kit's rules, `AGENTS.md`,
  `docs/ai-ignore-strategy.md`, the READMEs and the generator itself, that
  `.agents/skills/` "is read as a fallback location natively by every supported
  tool." It is not, and that assumption is what left the skills unreachable.
- Fixed the mutually broken links between `prompt-engineer-analyst.md` and
  `design/prompt-patterns.md` in the bundled `rad-prompt-studio`.

### Added

- `.agents/rules/analysis-output.md` — the input-resolution and output-naming
  rule the three bundled `rad-prompt-studio` master prompts reference but which
  was not present in this kit, leaving them unable to resolve a report path.
- `tools/verify-kit.ps1` and `.github/workflows/verify.yml` — a mechanical
  consistency gate (generator drift, Cursor extension, skill-link presence,
  `SKILL.md` frontmatter, `[FILL IN` residue, README image links, `LICENSE`),
  runnable locally as `pwsh tools/verify-kit.ps1` and in CI from one script.


### Added

- `delphi-http-client` skill — consuming HTTP/REST APIs
  (THTTPClient/TNetHTTPClient/TRESTClient decision table, timeout/memory/
  status discipline, JSON, async, retry patterns). Closes the verified
  server-only gap found by the system analysis (#18)

## [1.1.0] - 2026-07-21

Result of a three-way independent system analysis (Claude, Codex, Gemini)
run through the kit's own five-lens discipline — every fix below links to
its GitHub issue.

### Fixed

- Memory leak in `delphi-patterns` `CreateCustomer` example — `LExisting`
  freed before raising, success-path ownership contract stated (#1)
- Memory leak in `dmvc-framework` Create/Update examples — `BodyAs`
  result now freed on every path via `try..finally` (#2)
- `dunitx-testing` fake repository now owns inserted objects
  (`Create(True)`); TearDown releases the interface explicitly (#3)
- Incomplete bulk-`Update` examples in `dext-framework` skill and
  `dext-patterns.md` — added the missing `.Set(...)` step (#4)
- "bank" mistranslations ("banco de dados" leftovers) in
  `horse-patterns.md` and `intraweb-patterns.md` (#5)
- Double negative inverting the SRP rule in `intraweb-patterns.md` (#6)
- Broken `frameworks.md` reference in `devexpress-components` (#7)
- `devexpress-components` filter advice scoped to server-limited
  datasets (#8)
- Generic-catch prohibition scoped to business/domain code with
  explicit top-level-boundary allowance (#9)
- `clean-code` dependent-edit rule scoped to actually-broken
  dependents (#10)
- `intraweb-framework` async events: exception-surfacing guidance
  added (#11)
- `threading`: captured-object lifetime warning for `TThread.Queue` (#12)
- Boss package manager added to `AGENTS.md` stack list (#13)
- Intended `AGENTS.md` vs `.claude/CLAUDE.md` delta documented (#14)
- `rad-repo-scaffold` Python 3 prerequisite declared with manual
  fallback (#15)

### Changed

- `acbr-components` skill merged into `.agents/rules/acbr-patterns.md`
  (SSL-lib abstraction, TEF handler, NFCe/SAT prefixes preserved) —
  one source per topic (#16)

### Removed

- `flexcel-net` skill — C#/.NET content out of a Delphi-only kit;
  `flexcel-vcl` covers the Delphi side (#17)

## [1.0.0] - 2026-07-20

Initial public release as an independent repository.
