# Analysis — `image-generation-base-prompt` v1
**Reviewer:** Gemini 3.1 Pro (High) · **Run:** v1 · **Date:** 2026-07-21
**Target:** `Prompts/system/image-generation-base-prompt.md` (173 lines, 10544 B) — verified
**Lenses applied:** all five | Full analysis
**Mode:** Analysis (Explicitly selected from System Analysis pick-list)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)
Bu temel prompt dosyası, tüm projelerde görsel üretim standartlarını çok iyi bir mimariyle tanımlıyor. Negatif promptların nasıl kullanılacağı, sahnelerin birbirinden nasıl ayrışacağı çok net anlatılmış. Bağlam mühendisliği (Context Engineering) açısından başarılı bir ayrıştırma yapılmış.

### [OVERALL] Genel Değerlendirme
An excellently engineered base prompt. It separates universal constraints from kit-specific overrides perfectly. The historical context provided (why the factory trope is banned) helps models understand the *intent* behind the constraints.

### [ADVISORY] Güçlü Ayrıştırma (Progressive Disclosure)
- Category: ADVISORY
- Lens(es): Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `Prompts/system/image-generation-base-prompt.md`
- Finding: Keeping the base rules here instead of duplicating them in every `image-prompts.md` file is a great example of maintaining a single source of truth and progressive disclosure.
- Evidence: "This file owns the universal rules... the per-kit file owns only the mascot's stack-specific accessory".
- Impact: Reduces token usage and maintenance overhead when the rules need to change.
- Recommendation: Maintain this pattern for other generators in the system.

## Coverage Manifest
- `Prompts/system/image-generation-base-prompt.md` — 173 lines, read in full
