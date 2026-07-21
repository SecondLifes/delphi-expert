# Changelog

All notable changes to the Delphi AI Spec-Kit are documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) ·
Versioning: [SemVer](https://semver.org/) via annotated git tags.

## [Unreleased]

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
