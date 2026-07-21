# Analysis — tdd-patterns v1
**Reviewer:** Antigravity (gemini_v1) · **Run:** v1 · **Date:** 2026-07-21T21:42:00+03:00
**Target:** `.agents/rules/tdd-patterns.md` (13 lines, 1279 B) — verified
**Lenses applied:** all five | System Analysis Mode
**Mode:** Analysis ("review this file" -> Analysis per prompt-engineer-analyst.md:58-60)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu dosya, Delphi'de DUnitX framework'ü ile Test Güdümlü Geliştirme (TDD) standartlarını kısa ve öz bir şekilde listelemektedir. `Metodo_Cenario_Expectativa` isimlendirme formatı, dış bağımlılıkların (veritabanı, ağ vs.) FAKE nesnelerle kesinlikle izole edilmesi zorunluluğu ve istisnaların `Assert.WillRaise()` kullanılarak nasıl doğrulanacağı çok açık olarak belirtilmiştir.

### [ADVISORY] Inconsistent language mix in naming conventions

- Category: ADVISORY
- Lens(es): Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: line 9
- Finding: The text provides English instructions but gives a Portuguese structure pattern for naming conventions.
- Evidence: Line 9 instructs using `Metodo_Cenario_Expectativa` while the example is English `CalculateTax_EmptyOrder_ThrowsException`.
- Impact: Language mixing slightly harms the readability and could cause confusion about whether the final test names should actually be in English or Portuguese.
- Recommendation: Ensure that the structure pattern accurately matches the language of the example (e.g., `Method_Scenario_Expectation`).

## Coverage Manifest
- `.agents/rules/tdd-patterns.md` — 13 lines, read in full
