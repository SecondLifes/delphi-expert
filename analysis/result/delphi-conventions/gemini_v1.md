# Analysis — `delphi-conventions` v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/delphi-conventions.md` (58 lines, 1572 B) — verified
**Lenses applied:** all five | System Analysis mode
**Mode:** Analysis (Targeted analysis requested by user)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu analizde, genel Delphi kodlama standartlarını belirleyen `delphi-conventions.md` incelenmiştir. İsimlendirme (PascalCase vb.), biçimlendirme (2 boşluk girinti, 120 karakter sınırı) ve bellek yönetimi (try/finally) kuralları genel olarak tutarlı ve nettir. Özellikle `with` ifadelerinin, global değişkenlerin ve jenerik istisna yakalamanın (generic catch) yasaklanması yapay zekanın kaliteli kod üretmesi için oldukça etkilidir. 

### [OVERALL] Delphi Conventions Assessment

- Category: OVERALL
- Lens(es): Prompt Engineer Analyst, Repo Auditor
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: .agents/rules/delphi-conventions.md
- Finding: The rules are concise and clear, acting as a strong baseline for Delphi AI code generation. The strict prohibitions on common Delphi anti-patterns (`with`, global variables, magic numbers) are excellent. The indentation rules and mandatory unit structure provide explicit formatting instructions for the AI to follow.
- Evidence: "Prohibitions: ❌ with statement, ❌ Global variables, ❌ Generic Catch"
- Impact: It provides a solid foundation, minimizing code smell and technical debt in generated code.
- Recommendation: No major changes are required. The rule set is well-scoped and direct.

### [ADVISORY] Ambiguity in exception handling prohibition

- Category: ADVISORY
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Line 54
- Finding: The prohibition "❌ Generic Catch (`except on E: Exception`)" is a good practice, but without instructing the AI what to do instead, the AI might leave exceptions unhandled, invent non-standard workarounds, or just use empty blocks.
- Evidence: "❌ Generic Catch (`except on E: Exception`)"
- Impact: The model might struggle to correctly implement exception handling if the generic catch is banned without explicitly stating the required alternative (e.g., catching specific exception types or logging and re-raising).
- Recommendation: Add a short positive instruction to the rule, e.g., "Catch specific exception types, or log and re-raise if catching generically is necessary".

## Coverage Manifest
- `.agents/rules/delphi-conventions.md` — 58 lines, read in full
