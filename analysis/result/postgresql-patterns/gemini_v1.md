# Analysis — postgresql-patterns v1
**Reviewer:** Antigravity (gemini_v1) · **Run:** v1 · **Date:** 2026-07-21T21:42:00+03:00
**Target:** `.agents/rules/postgresql-patterns.md` (159 lines, 4641 B) — verified
**Lenses applied:** all five | System Analysis Mode
**Mode:** Analysis ("review this file" -> Analysis per prompt-engineer-analyst.md:58-60)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu kural dosyası, Delphi projelerinde FireDAC aracılığıyla PostgreSQL kullanım standartlarını belirlemektedir. Özellikle eski `SERIAL` yapısı yerine yeni nesil `GENERATED ALWAYS AS IDENTITY` yapısının kullanılması, `RETURNING` ile veri okurken `ExecSQL` yerine `Open` kullanılması gerektiği gibi çok önemli detaylara değinmektedir. Yapı olarak kapsamlı ve doğrudur. 

### [ADVISORY] Improve language consistency in code comments

- Category: ADVISORY
- Lens(es): Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: lines 14, 62, 81, 131
- Finding: The document mixes Portuguese comments with mostly English instructional content.
- Evidence: Line 14: `{ Configuração base }`, Line 62: `{ Gravar JSONB }`, Line 81: `{ Transação explícita }`, Line 131: `-- Tabela existe?`.
- Impact: This dual-language approach reduces prompt consistency and readability.
- Recommendation: Unify the language of the code comments and inline explanations to either purely English or purely Portuguese.

## Coverage Manifest
- `.agents/rules/postgresql-patterns.md` — 159 lines, read in full
