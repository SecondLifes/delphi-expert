# Analysis — `dext-patterns` v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/dext-patterns.md` (76 lines, 2653 B) — verified
**Lenses applied:** all five | System Analysis mode
**Mode:** Analysis (Targeted analysis requested by user)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu analizde, Dext Framework (.NET tarzı Minimal API ve ORM içeren Delphi çatısı) için hazırlanan `dext-patterns.md` kural dosyası incelenmiştir. Yönlendirme (Routing) ve Dependency Injection kuralları çok açık ve faydalı örneklerle desteklenmiştir. Özellikle "DevExpress bileşenleriyle bu çerçeveyi (Dext) karıştırmayın" uyarısı yapay zekanın genel isim benzerliği nedeniyle yapabileceği kritik bir hatayı önleyen mükemmel bir korumadır. Ancak ORM örneklerinde kullanılan akıllı özellik (Smart Properties) tanımlayıcılarının (`O`, `P` gibi) nerede ve nasıl tanımlandığı gösterilmemiştir; bu eksiklik AI tarafından üretilen kodun derleme hatası vermesine neden olabilir.

### [OVERALL] Dext Framework Patterns Assessment

- Category: OVERALL
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: .agents/rules/dext-patterns.md
- Finding: The rules correctly capture the .NET-like Minimal API architecture of the Dext framework and explicitly prohibit manual service instantiation. The "do not confuse DevExpress with this Dext framework" rule is a high-value guardrail. However, the ORM examples leave out the declaration of the Query Expressions (`O` and `P`), which could cause the AI to hallucinate variables that don't exist or write uncompilable queries.
- Evidence: "var Orders := DbContext.Orders.Where((O.Status = TOrderStatus.Paid)...)" with no preceding declaration of `O`.
- Impact: Solid foundation, but the incomplete ORM example will likely result in uncompilable code when the AI generates database queries, requiring the user to fix "Undeclared identifier" errors manually.
- Recommendation: Add a line in the code snippet showing the declaration of the Smart Property variables before they are used in the query.

### [ADVISORY] Undefined Query Expression variables in ORM examples

- Category: ADVISORY
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Lines 51-63 (Dext.Entity ORM)
- Finding: The ORM examples use query expressions `O.Status` and `P.Category` inside `.Where()` clauses, but never demonstrate how `O` or `P` are defined or instantiated.
- Evidence: `Where((O.Status = TOrderStatus.Paid) and (O.Total > 1000))`
- Impact: The model will copy this pattern literally but will fail to define the `O` variable in its own generated code, directly leading to "Undeclared identifier: 'O'" compilation errors in Delphi.
- Recommendation: Update the code blocks to include the instantiation or retrieval of these query expressions (e.g., `var O := TOrder.Entities;` or however Dext specifically implements the alias definition) so the AI knows how to declare them properly.

## Coverage Manifest
- `.agents/rules/dext-patterns.md` — 76 lines, read in full
