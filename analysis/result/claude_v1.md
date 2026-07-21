# Analysis — `delphi-expert` v1
**Reviewer:** claude (Sonnet 5) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `spec-kits/delphi-expert` — system content only: `.agents/skills/` (28 skill folders, ~95 files), `.agents/rules/` (16 files), `.agents/commands/review.md`, `AGENTS.md` (823 lines), `.claude/CLAUDE.md` (104 lines) — verified, all files opened this session. Explicitly out of scope per the request: `examples/`, `docs/`, `src/`, `tools/`, `README*`.
**Lenses applied:** all five
**Mode:** Analysis ("sistem içeriklerini... analiz etmek" → Analysis per prompt-engineer-analyst.md's WORK MODES: "review"/"analyze" alone → Analysis, no rewrite requested)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

`delphi-expert` kitinin sistem katmanı (skill/rule/identity, örnek kod veya çıktı dosyaları hariç) baştan sona okundu: 28 skill klasörü, 16 rule dosyası, `AGENTS.md` ve `.claude/CLAUDE.md`. Genel kalite yüksek — Delphi'ye özgü içerik (SOLID, tasarım desenleri, 3 veritabanı motoru, threading, memory/exception yönetimi) tutarlı ve derin. En önemli bulgu: bu kite gömülü 5 genel-amaçlı "rad-*" skill'in (`rad-prompt-studio`, `rad-python`, `rad-web-scraping`, `rad-skill-finder`, `rad-powershell-master`) hepsi workspace'in kendi güncel sürümlerinden **geride** — özellikle `rad-prompt-studio` %62 daha kısa ve tamamen eski mimariyi kullanıyor. Ayrıca `aurelius-mapping`/`aurelius-objects` skill'lerinin TMS'in kendi resmi skill deposundan geldiği doğrulandı (web araması ile), ama bu kaynak hiçbir yerde not edilmemiş. Eksik skill adayı olarak en güçlü bulgu: dış REST API'lere Delphi'den istek atma (HTTP client tüketimi) konusunda hiçbir skill yok — sadece sunucu tarafı (Horse/DMVC/Dext) kapsanmış.

---

## OVERALL — Genel Değerlendirme

- **Category:** OVERALL
- **Lens(es):** all five
- **Verification verdict:** VERIFIED
- **Evidence type:** executed/observed-this-session
- **Location:** `spec-kits/delphi-expert/.agents/skills/*`, `.agents/rules/*`, `AGENTS.md`, `.claude/CLAUDE.md`
- **Finding:** The system layer is large (28 skills, 16 rules) but coherent. Domain skills (SOLID/design-patterns/refactoring/clean-code/memory-exceptions, the three DB engines, Horse/DMVC/Dext/Intraweb/ACBr/DevExpress, the two FlexCel editions, the two Aurelius skills, threading, DUnitX/TDD) are internally consistent with each other and with `AGENTS.md`'s embedded summaries (spot-checked SOLID DIP example, Firebird/PostgreSQL/MySQL golden rules, memory-management golden standard — all match). The five bundled "rad-*" utility skills, by contrast, are stale copies of workspace-root skills with no update mechanism.
- **Evidence:** Full read of every file listed in the Target line; `diff -rq` against `.claude/skills/{rad-python,rad-web-scraping,rad-skill-finder,rad-prompt-studio,rad-powershell-master}` this session.
- **Impact:** A user working in this kit gets excellent Delphi-specific guidance, but invoking `rad-prompt-studio` from inside this kit (as `AGENTS.md`'s own instruction tells any AI to do) silently runs an outdated version of that skill.
- **Recommendation:** See MISSING-01 below — establish or run a propagation step for the bundled rad-* skills; this is the single highest-value fix available for this kit.

---

## CRITICAL — Kritik Bulgular

*No critical (correctness-breaking, unsafe, or data-loss-causing) findings in the domain content itself.*

---

## BUG — Yazılım Hataları

*None found — this is documentation/instruction content, not executable code; no runtime defects apply.*

---

## ERROR — İçerik/Mantık Hataları

*No factually wrong Delphi/framework content found. Spot-checked claims that are easy to get wrong and easy to verify: Firebird `RETURNING` requires `Open` not `ExecSQL` (correct, consistent across `AGENTS.md`, `firebird-database` skill, `delphi-conventions.md`); MySQL has no `RETURNING`, needs `LAST_INSERT_ID()` (correct, consistent); PostgreSQL `IDENTITY` over `SERIAL` for new projects (correct); the three DBs' default transaction isolation levels (Firebird/PostgreSQL = Read Committed, MySQL/InnoDB = Repeatable Read) are stated correctly and consistently in both `mysql-database`'s and `postgresql-database`'s own `transactions-and-ops.md` comparison tables.*

---

## MISSING — Eksik Unsurlar

### [MISSING-01] Bundled `rad-*` skills are stale copies with no sync mechanism

- Category: MISSING
- Lens(es): DevOps/Config Engineer, Systems Forensics Analyst, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: executed/observed-this-session
- Location: `spec-kits/delphi-expert/.agents/skills/{rad-prompt-studio,rad-python,rad-web-scraping,rad-skill-finder,rad-powershell-master}/`
- Finding: This kit bundles static copies of five workspace-root skills. All five have drifted from their canonical `.claude/skills/` counterparts, with no mechanism (script, hardlink, or documented manual step) to detect or fix the drift. `rad-prompt-studio` is the worst case: 97 lines vs. the canonical 258 — missing Evaluation Mode entirely, missing the Golden Rules section, and still structured around a single monolithic `references/five-lenses.md` file instead of the current five separate lens files (`context-engineer.md`, `devops-config-engineer.md`, `prompt-engineer-analyst.md`, `repo-auditor.md`, `systems-forensics-analyst.md` — none of which exist in this kit's copy).
- Evidence: `diff -rq ".claude/skills/rad-prompt-studio" "spec-kits/delphi-expert/.agents/skills/rad-prompt-studio"` reports 5 files present only in the canonical version and the rest byte-different; `wc -l` on both `SKILL.md`s: 258 vs 97 lines. `rad-python`, `rad-web-scraping`, `rad-skill-finder`, `rad-powershell-master` each reported every file as differing via `diff -rq`. For `rad-powershell-master` specifically, the diff shows the bundled copy is missing the entire "Usage" table section (11 lines) that was added workspace-wide to all 6 base skills earlier this session.
- Impact: `AGENTS.md`'s own instruction says referencing `rad-prompt-studio` "is the complete instruction to load every file under `.agents/skills/rad-prompt-studio/references/*.md` and adopt all five specialist lenses" — but this kit's bundled copy doesn't have five lens files to load, it has one. Any AI following `AGENTS.md` literally from inside this kit gets the pre-consolidation architecture, not the current one.
- Recommendation: Either (a) add these five skills to `rad-template-builder`'s Maintenance Mode propagation scope so they get refreshed whenever the workspace-root versions change, or (b) if hardlinking across the `spec-kits/*` submodule boundary is undesirable (it would tie the kit's own git history to the parent workspace's), define an explicit periodic "refresh bundled rad-* skills" step and run it now as a first pass.

### [MISSING-02] No provenance record for the two TMS-sourced skills

- Category: MISSING
- Lens(es): Repo Auditor, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: executed/observed-this-session (WebSearch)
- Location: `spec-kits/delphi-expert/.agents/skills/aurelius-mapping/SKILL.md`, `aurelius-objects/SKILL.md`
- Finding: `aurelius-mapping`'s and `aurelius-objects`' descriptions match, almost word for word, the official skill descriptions published by TMS Software (the vendor of Aurelius) at `github.com/tmssoftware/skills`. This is good content provenance (vendor-authored, not reverse-engineered), but nothing in this kit records that origin — unlike the workspace root's own `skills-lock.json` convention, which tracks source repo/path/hash for every `npx skills`-installed skill.
- Evidence: WebSearch confirmed `github.com/tmssoftware/skills` ships exactly two skills, named `aurelius-mapping` and `aurelius-objects`, with descriptions matching this kit's copies (e.g. "Map Delphi classes to a relational database using TMS Aurelius ORM attributes... Use when the user asks to create entity classes, add Aurelius mapping..."). `grep`-checked: no `SKILL.md` in this kit contains the string "tmssoftware/skills" or any other provenance note.
- Impact: If TMS updates their official skill (new Aurelius version, new attribute), this kit has no record of where to re-fetch from, and no way to detect that its copy is now behind the vendor's.
- Recommendation: Add a one-line provenance note to both `SKILL.md` files (source repo + retrieval date), and consider whether these two should be tracked the same way `skills-lock.json` tracks the workspace's own `npx skills`-installed content.

---

## WARNING — Uyarılar

### [WARNING-01] Guard-clause/memory-management content duplicated across 5 locations

- Category: WARNING
- Lens(es): Prompt Engineer & Analyst, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `AGENTS.md` (Clean Code + Memory Management sections), `.agents/skills/clean-code/SKILL.md`, `.agents/skills/delphi-memory-exceptions/SKILL.md`, `.agents/skills/code-review/SKILL.md`, `.agents/rules/memory-exceptions.md`
- Finding: The same guard-clause example (`ProcessOrder`/`Withdraw`-style nested-if → early-return rewrite) and the same `try..finally` "Gold Standard" example appear near-verbatim in at least four of these five files, plus a fifth variant in `refactoring/references/extraction-and-structure.md`.
- Evidence: Compared the guard-clause example text across `AGENTS.md:562-592`, `clean-code/SKILL.md:53-83`, and `refactoring/references/extraction-and-structure.md:172-222` — same structure, same exception types, cosmetic naming differences only.
- Impact: Not currently contradictory (checked — all five tell the same story), but a future correction to the recommended pattern (e.g. a new exception-hierarchy convention) requires editing five files to stay consistent, and nothing currently enforces that.
- Recommendation: Not urgent given zero current contradictions found, but worth deciding whether `clean-code` and `code-review` should reference `delphi-memory-exceptions`'s example rather than each restating it, the same way `design-patterns/SKILL.md` already does correctly for its own `references/*.md` files (states the pattern, defers the example).

### [WARNING-02] `acbr-components` skill barely goes deeper than `acbr-patterns.md` rule

- Category: WARNING
- Lens(es): Prompt Engineer & Analyst, DevOps/Config Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/skills/acbr-components/SKILL.md`, `.agents/rules/acbr-patterns.md`
- Finding: Every other rule/skill pair in this kit follows "rule = quick reference, skill = deeper reference files" (e.g. `firebird-patterns.md` rule vs. `firebird-database` skill's 4 reference files). `acbr-components` has no `references/` folder at all, and its content (component-wrapper pattern, TEF callback handling, dynamic memory management, prefix table) overlaps almost entirely with `acbr-patterns.md`'s own content — same wrapper example, same prefix table, same "no strong visual coupling" rule.
- Evidence: Both files' "Component Prefixes" tables list the same 7 ACBr components with matching prefixes; both show near-identical `TNFeEmissorACBr`/`INFeEmissor` wrapper examples.
- Impact: Not a defect (no contradiction), but this pairing doesn't earn its "skill" status the way the DB/framework pairs do — a reader gets no additional depth by consulting the skill after the rule.
- Recommendation: Either expand `acbr-components` with genuine additional depth (e.g. `references/` covering NFe/CTe/SAT-specific XML/signing details not in the rule), or fold it into the rule and remove the skill, consistent with how thin this kit otherwise keeps single-purpose skills.

### [WARNING-03] `rad-repo-scaffold` is the one bundled skill with no Delphi tie and no workspace-root counterpart

- Category: WARNING
- Lens(es): Repo Auditor, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: executed/observed-this-session
- Location: `.agents/skills/rad-repo-scaffold/`
- Finding: Unlike the other four bundled `rad-*` skills, `rad-repo-scaffold` has no counterpart under this workspace's own `.claude/skills/` — it isn't one of the six base skills `CLAUDE.md` documents, and it isn't Delphi-specific (its own description: "Gather a repository's purpose from a prompt... create a minimal, purpose-fit AI-ready structure").
- Evidence: `ls .claude/skills/` at the workspace root does not list `rad-repo-scaffold`; the workspace `CLAUDE.md`'s Base Skills section enumerates exactly six self-authored/sourced skills, and this isn't one of them.
- Impact: Not a defect, but its provenance and intentionality are unclear — it may be a genuinely useful general-purpose addition to this kit, or a leftover from an earlier `npx skills` install that was never reviewed the way `rad-python`/`rad-powershell-master` were at the workspace level (per `CLAUDE.md`'s own account of that review process).
- Recommendation: Confirm whether this skill's inclusion in `delphi-expert` was deliberate; if so, consider whether it belongs in the workspace's own base-skill set too (for consistency), and whether it should get the same "reviewed and fixed" treatment `rad-powershell-master` got.

---

## ADVISORY — İyileştirme Tavsiyeleri

### [ADVISORY-01] No disambiguation between Aurelius ORM and raw FireDAC+Repository pattern

- Category: ADVISORY
- Lens(es): Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/skills/aurelius-mapping/`, `.agents/skills/aurelius-objects/`, `.agents/skills/delphi-patterns/SKILL.md` (Repository Pattern section), the three DB skills
- Finding: The kit teaches two parallel, non-interoperable ways to talk to a database — hand-rolled `ICustomerRepository` + `TFDQuery` (the pattern used everywhere in `delphi-patterns`, `firebird-database`, `mysql-database`, `postgresql-database`) and TMS Aurelius's `TObjectManager` (a full ORM with its own object-lifetime rules) — with no stated guidance on which to reach for.
- Evidence: `delphi-patterns/SKILL.md`'s "When to Use" list includes "When creating a repository (data access)" with no mention of Aurelius as an alternative; `aurelius-objects/SKILL.md` never mentions the raw-FireDAC path either.
- Impact: A request like "create a repository for TCustomer" could plausibly trigger either skill with no clear tie-breaker, producing genuinely incompatible code (Aurelius entities must never be freed manually; FireDAC-repository entities must always be freed via `try..finally`).
- Recommendation: A short disambiguation note (mirroring `devexpress-components/SKILL.md`'s existing "don't call these components Dext" note) in one or both skills: e.g. "use Aurelius when the project already uses `TObjectManager`/has Aurelius mapping attributes on its entities; use the Repository+FireDAC pattern otherwise."

### [ADVISORY-02] `flexcel-net` teaches C#/.NET inside a Delphi-only kit

- Category: ADVISORY
- Lens(es): Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/skills/flexcel-net/SKILL.md` (description: "Use when writing C# / VB.NET / F# code...")
- Finding: Every other skill in this kit is Object Pascal/Delphi-specific (per `AGENTS.md`'s own identity: "You are a senior Delphi (Object Pascal) engineer"). `flexcel-net` is the sole exception — a full skill for a different language and runtime, presumably included because FlexCel VCL and FlexCel .NET are sibling products from the same vendor.
- Evidence: Compared `flexcel-vcl/SKILL.md` (Delphi/Pascal, `TXlsFile`) against `flexcel-net/SKILL.md` (C#/.NET, `XlsFile`) — same product family, different language entirely.
- Impact: Harmless (it will only trigger on genuinely .NET-flavored requests, per its own description), but worth a deliberate call: if this kit's users are Delphi-only, this skill is dead weight; if some maintain a companion .NET service, it's a reasonable inclusion.
- Recommendation: No action needed unless the kit's actual audience is confirmed Delphi-only — then remove for token efficiency.

---

## ADDITION — Ekleme Adayları

### [ADDITION-01] No skill for consuming external REST/HTTP APIs from Delphi

- Proposed content/capability: A skill covering `TNetHTTPClient`/`TRESTClient` (or a comparable HTTP-client library) for calling third-party HTTP/REST APIs from Delphi — request building, JSON response deserialization, timeout/retry handling, and error-code mapping.
- Sources that raised it: Prompt Engineer & Analyst lens, cross-referenced against every other skill in this kit.
- The gap it's meant to fill: Every REST-related skill in this kit (`horse-framework`, `dmvc-framework`, `dext-framework`) teaches building a server that accepts HTTP requests. None teach the opposite direction — a Delphi application calling out to an external API (a payment gateway's REST endpoint, a government fiscal-document webservice, a third-party SaaS). Given `acbr-patterns.md` and `delphi-conventions.md` already discuss wrapping external fiscal/payment integrations, this is a directly adjacent, currently unfilled gap.
- Whether that gap is real: Checked `rad-skill-finder`'s own npx-skills-ecosystem search channel via a live web search this session — found no existing Delphi-specific HTTP-client-consumption skill in the public skills marketplaces surveyed (skills.sh, ClaudeSkills.info, Agensi); TMS's own official skills repo (`github.com/tmssoftware/skills`) currently ships only the two Aurelius skills, nothing for HTTP consumption. The gap appears real, not just unfound.
- Recommendation: `ACCEPT` — a plausible, concretely-scoped addition; `rad-skill-finder` should still be run properly (with `rad-template-builder`'s full research workflow) before authoring one from scratch, in case a suitable community skill surfaces that a single web search missed.

### [ADDITION-02] No standalone logging skill

- Proposed content/capability: A skill for structured logging in Delphi (e.g. LoggerPro, a widely-used open-source Delphi logging library, or an equivalent) — setup, log-level conventions, and the sink patterns (file/console/remote) commonly paired with the frameworks already in this kit.
- Sources that raised it: DevOps/Config Engineer lens.
- The gap it's meant to fill: `Logger.LogError(...)`-style calls appear as incidental examples throughout this kit (`delphi-memory-exceptions/SKILL.md`, `clean-code/SKILL.md`, `threading/references/basics-and-ppl.md`) but no skill defines what `Logger` actually is, or how to wire it up — every other cross-cutting concern this kit touches (testing via DUnitX, threading, memory) has its own dedicated skill; logging does not.
- Whether that gap is real: Plausible given the pattern above, but not independently verified via a skill-finder search this pass (time-boxed to the single ADDITION-01 search) — flagging as a candidate for the next round rather than a confirmed gap.
- Recommendation: `DEFER` — real-looking gap, but needs its own `rad-skill-finder` pass before committing to write one from scratch.

### [ADDITION-03] No standalone JSON-handling skill

- Proposed content/capability: A skill for `System.JSON` (and/or a serializer library) covering (de)serialization of DTOs, handling optional/nullable fields, and date/enum conversion conventions — independent of any one web framework.
- Sources that raised it: Prompt Engineer & Analyst lens.
- The gap it's meant to fill: JSON handling is currently only taught incidentally, inside framework-specific skills (Horse's `Jhonson` middleware, DMVC's automatic `Render()` serialization) — there's no framework-agnostic guidance for a unit that just needs to parse or build JSON without a web framework in the picture (e.g. a config file, a log payload, a message-queue message).
- Whether that gap is real: Same caveat as ADDITION-02 — plausible from cross-referencing the existing skill set, not independently skill-finder-verified this pass.
- Recommendation: `DEFER` — same reasoning as ADDITION-02.

---

## REMOVAL / MERGE — Silme/Birleştirme Adayları

### [REMOVAL-01] `acbr-components` skill — candidate to merge into `acbr-patterns.md` rule

- Target content: `.agents/skills/acbr-components/SKILL.md`
- Rationale for removing/merging: See WARNING-02 — this skill currently adds no depth beyond the rule it pairs with, unlike every other rule/skill pair in this kit.
- Expected impact: Removing the skill (folding anything unique into the rule) would have no loss of guidance, based on this session's read; keeping both invites drift between two near-identical descriptions of the same wrapper pattern.
- Dependencies and risks: If `acbr-components` is referenced by name anywhere else in this kit (checked: `AGENTS.md`, `.claude/CLAUDE.md`, and all 16 rule files do not reference it by name), removal is low-risk.
- Recommendation: `MERGE` (into `acbr-patterns.md`, expanding the rule with anything the skill states that the rule doesn't) — proposal only, final call is the user's.

---

## DISCARDED — Elenen Bulgular

*None — no candidate finding failed verification against the actual current files this session.*
