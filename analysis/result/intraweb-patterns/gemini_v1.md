# Analysis — `intraweb-patterns` v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/intraweb-patterns.md` (49 lines, 2731 B) — verified
**Lenses applied:** all five | System Analysis mode
**Mode:** Analysis (Targeted analysis requested by user)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu analizde, Delphi'nin stateful web framework'ü olan Intraweb için hazırlanmış `intraweb-patterns.md` kural dosyası incelenmiştir. Özellikle Intraweb'in çok iş parçacıklı (multi-threaded) doğası gereği global değişken kullanımının yasaklanıp `UserSession` nesnesinin emredilmesi ve VCL'nin engelleyici (blocking) diyalog pencerelerinin engellenmesi çok kritik ve doğru mimari kurallardır. Ancak metin içindeki bazı İngilizce çeviri hataları (örneğin veritabanı yerine "bank" denmesi ve çift olumsuzlama nedeniyle bir kuralın anlamının tam tersine dönmesi) yapay zekanın davranışını olumsuz etkileyebilir.

### [OVERALL] Intraweb Patterns Assessment

- Category: OVERALL
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: .agents/rules/intraweb-patterns.md
- Finding: The rules accurately address the most common and critical failure points in Intraweb development: thread-safety across sessions (UserSession vs globals) and preventing blocking UI calls (VCL dialogs on a web server). The technical guidance is excellent. However, there are translation issues—most notably a double negative that reverses the meaning of a rule, and translating "database" as "bank"—that significantly reduce instructional clarity.
- Evidence: "No business's primary insertion engine cannot live in this TIWAppForm.pas"
- Impact: Solid foundation, but poor English translation can confuse the AI or human readers, particularly the double negative which instructs the AI to do the opposite of what is intended.
- Recommendation: Correct the mistranslations and grammatical errors to ensure the AI perfectly understands the constraints.

### [ERROR] Double negative reverses intended rule

- Category: ERROR
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Line 38
- Finding: The rule states: "No business's primary insertion engine cannot live in this TIWAppForm.pas." This is a double negative ("No ... cannot") that literally translates to "Every business engine must live here," which is the exact opposite of the Single Responsibility Principle (SRP) intended.
- Evidence: "No business's primary insertion engine cannot live in this TIWAppForm.pas."
- Impact: The AI might misunderstand this rule due to the grammar error and decide it is required to place business logic inside the form, violating the stated SRP architectural goal.
- Recommendation: Change to clear, positive constraint phrasing: "The primary business logic must not live in TIWAppForm.pas" or "No business logic should live in TIWAppForm.pas".

### [ERROR] Mistranslation of "database" as "bank"

- Category: ERROR
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Line 28
- Finding: "Bank recordings" is an incorrect literal translation of the Portuguese "gravações no banco" (Database records/saves).
- Evidence: `//Bank recordings and color change in the local interface`
- Impact: Will cause confusing code comment generation by the AI, especially in non-financial applications.
- Recommendation: Change to `// Database saves and color change in the local interface`.

## Coverage Manifest
- `.agents/rules/intraweb-patterns.md` — 49 lines, read in full
