# Analysis — mysql-patterns v1
**Reviewer:** Antigravity (gemini_v1) · **Run:** v1 · **Date:** 2026-07-21T21:42:00+03:00
**Target:** `.agents/rules/mysql-patterns.md` (150 lines, 4460 B) — verified
**Lenses applied:** all five | System Analysis Mode
**Mode:** Analysis ("review this file" -> Analysis per prompt-engineer-analyst.md:58-60)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu dosya, Delphi'de FireDAC kullanarak MySQL/MariaDB veritabanlarına bağlanma ve işlem yapma kalıplarını açıklamaktadır. Özellikle `utf8` yerine `utf8mb4` kullanımı, `RETURNING` ifadesinin MySQL'de olmaması nedeniyle `LAST_INSERT_ID()` kullanımı, InnoDB motoru zorunluluğu ve spesifik FireDAC hata yönetimi (ekUKViolated, ekFKViolated vb.) gibi çok kritik teknik kuralları net bir şekilde ortaya koymaktadır. Yapısı oldukça tutarlı ve eksiksiz görünmektedir.

### [ADVISORY] Improve structural hierarchy and language consistency

- Category: ADVISORY
- Lens(es): Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: lines 14, 57, 103-109
- Finding: The document mixes English and Portuguese text in its comments and string literals.
- Evidence: Line 14: `{ Configuração base }`, Line 57: `{ Gravar JSON }`, Line 103: `'Registro duplicado'`, but the headers and other explanations are in English (e.g., `## Important Care`, `//⚠️ MySQL DOES NOT support RETURNING!`).
- Impact: Mixing languages causes slight inconsistency for LLMs parsing the file and reduces uniform readability.
- Recommendation: Standardize all comments and string literals to match the primary language of the instructions (English or Portuguese).

## Coverage Manifest
- `.agents/rules/mysql-patterns.md` — 150 lines, read in full
