# Analysis — `horse-patterns` v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/horse-patterns.md` (58 lines, 1612 B) — verified
**Lenses applied:** all five | System Analysis mode
**Mode:** Analysis (Targeted analysis requested by user)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu analizde, Delphi için Horse REST API çerçevesinin kullanım kurallarını belirten `horse-patterns.md` incelenmiştir. Yönlendirme (Routing), Middleware kullanımı ve Controller kuralları çok iyi ve öz olarak özetlenmiştir. Ayrıca "Controller'dan doğrudan veritabanına erişim" ve "İş mantığını Controller'da tutmak" gibi temel anti-pattern'ler kesin bir şekilde yasaklanmıştır. Ancak dokümanda bazı çeviri hataları göze çarpmaktadır: Portekizce veritabanı anlamına gelen "banco de dados" kelimesinin yanlış çevirisi sonucu "bank" yazılmış ve "Cada handler" gibi bazı Portekizce ifadeler unutulmuştur. Bunların düzeltilmesi önerilir.

### [OVERALL] Horse Patterns Assessment

- Category: OVERALL
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: .agents/rules/horse-patterns.md
- Finding: The rules present a solid, concise architectural constraint set for the Horse framework. It rightfully emphasizes a service-oriented approach by prohibiting direct database access or business logic in the controller, and correctly maps standard HTTP methods to RESTful conventions. However, the document contains poor English translations (e.g., "bank" instead of "database") and residual Portuguese ("Cada handler") which slightly compromises the instructional quality.
- Evidence: "Cada handler:" and "Controllers DO NOT access the bank directly".
- Impact: While an AI model can likely infer the contextual meaning of "bank" from "access the bank directly", it is an unprofessional translation that may lead to the AI generating strange variable names or comments.
- Recommendation: Correct the mistranslations and unify the language to English.

### [ERROR] Mistranslation of "database" as "bank"

- Category: ERROR
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Line 15
- Finding: The instruction translates the Portuguese "banco de dados" or "banco" (database) incorrectly as "bank".
- Evidence: "Controllers DO NOT access the bank directly — they delegate it to Services"
- Impact: This can confuse human developers reading AI-generated comments, or theoretically cause the AI to generate comments/variables referring to financial banks instead of a database if context is somehow lost.
- Recommendation: Change the word "bank" to "database".

### [ADVISORY] Residual Portuguese language

- Category: ADVISORY
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Line 14
- Finding: The phrase "Cada handler:" remains in Portuguese.
- Evidence: "Cada handler: class procedure Nome(..."
- Impact: Stylistic inconsistency that could bleed into generated code comments.
- Recommendation: Translate to "Each handler:".

## Coverage Manifest
- `.agents/rules/horse-patterns.md` — 58 lines, read in full
