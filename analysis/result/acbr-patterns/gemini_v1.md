# Analysis — `acbr-patterns` v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/acbr-patterns.md` (56 lines, 2010 B) — verified
**Lenses applied:** all five | System Analysis mode
**Mode:** Analysis (Targeted analysis requested by user)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu analizde, Delphi'de ACBr proje bileşenleri için belirlenen kuralları içeren `acbr-patterns.md` dosyası incelenmiştir. Genel mimari kurallar (SOLID, bileşenlerin UI dışında servis olarak yapılandırılması) oldukça doğru ve güvenlidir. Ancak kural dosyasının bazı tavsiyeleri (örneğin hata yönetimi ve yapılandırılmış yanıtlar) kod örneği içermediğinden, AI asistanın tutarlı kod üretmesi için yetersiz kalabilir.

### [OVERALL] ACBr Patterns Assessment

- Category: OVERALL
- Lens(es): Prompt Engineer Analyst, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: .agents/rules/acbr-patterns.md
- Finding: The document effectively sets boundaries for ACBr component usage (avoiding direct UI attachment, enforcing dynamic creation and memory management). However, it relies heavily on textual descriptions without providing examples for some of its more complex instructions (like configuration injection and error formatting), which limits how well an AI can replicate the desired pattern.
- Evidence: Sections "Dynamic Settings" and "Error and Return Handling" provide guidelines but no code structures.
- Impact: Without concrete examples, the model might invent its own implementation of `IConfiguration` or Object Results, leading to inconsistent application code.
- Recommendation: Add short, concrete code snippets for the Configuration Injection and Error/Return Handling sections.

### [ADVISORY] Lack of concrete examples for Error Handling and Configuration

- Category: ADVISORY
- Lens(es): Prompt Engineer Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Lines 37-44 (Dynamic Settings, Error and Return Handling)
- Finding: The rules instruct to load configs via `IConfiguration` and convert component returns into structured records. While conceptually sound, there are no examples of what these records or interfaces should look like.
- Evidence: "Convert long component returns (e.g. status, rejection reasons) into application Object Results/Records"
- Impact: Models generate code based on patterns. Without an example of the desired Object Result, the AI will hallucinate a structure, reducing consistency across the codebase.
- Recommendation: Provide a snippet showing how an `INFeService.Emitir` response record is structured and populated from ACBr status codes.

### [ADVISORY] Missing scope constraints for UI prefixes

- Category: ADVISORY
- Lens(es): Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: Lines 46-55 (Prefix Conventions)
- Finding: The rule gives prefixes for design-time usage but previously states not to drop components on UI forms. It clarifies "If you need to drop the component...". While acceptable, it slightly muddies the strong "Do not instantiate... directly to UI" rule at the top.
- Evidence: "If you need to drop the component or instantiate it at runtime, follow these naming conventions"
- Impact: Low impact, but can cause the model to feel permitted to use UI drops if it justifies it.
- Recommendation: Reiterate that UI drops are strictly for legacy maintenance, not new code.

## Coverage Manifest
- `.agents/rules/acbr-patterns.md` — 56 lines, read in full
