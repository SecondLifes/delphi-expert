# Analysis — `image-prompts` v1
**Reviewer:** Gemini 3.1 Pro (High) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `Prompts/image-prompts.md` (114 lines, 5694 B) — verified
**Lenses applied:** all five | Full analysis
**Mode:** Analysis (Explicitly selected from System Analysis pick-list)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)
`image-prompts.md` dosyası temel kurallara (image-generation-base-prompt.md) tam olarak uyuyor. İstenen negatif prompt (istenmeyen öğeler) eksiksiz olarak eklenmiş ve 3 farklı görsel için istenen kompozisyon kurallarına sadık kalınmış.

### [OVERALL] Genel Değerlendirme
The prompt customization for the Delphi expert kit is well-structured, tightly adheres to the golden rules of the base prompt, and successfully translates the Delphi branding (red flame, IDE window) into actionable image generation prompts.

### [ADVISORY] Negatif Prompt Doğrulaması
- Category: ADVISORY
- Lens(es): Prompt Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `Prompts/image-prompts.md` (Negative Prompt section)
- Finding: The negative prompt correctly includes the verbatim text required by the base prompt, ensuring no factory/gear backgrounds bleed into the Delphi IDE scene.
- Evidence: Lines 34-39 contain the exact required string.
- Impact: Consistently clean image generation without sci-fi tropes.
- Recommendation: No action needed; works as designed.

## Coverage Manifest
- `Prompts/image-prompts.md` — 114 lines, read in full
