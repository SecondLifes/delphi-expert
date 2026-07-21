# Analysis — `delphi-conventions.md` v1
**Reviewer:** codex · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/delphi-conventions.md` (57 lines, 1,572 B) — verified
**Lenses applied:** all five
**Mode:** Analysis (System Analysis seçkisi)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)
Temel adlandırma ve bellek kuralları açıktır; genel exception yakalamayı mutlak yasaklamak güvenli logla-ve-yeniden-fırlat desenini de kapsayabilir.

### [WARNING-1] Genel exception yasağı aşırı geniş
- Category: WARNING
- Lens(es): Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/rules/delphi-conventions.md:55`
- Finding: `except on E: Exception` tümüyle yasaklanıyor.
- Evidence: `AGENTS.md`, loglama sonrası saf `raise;` eden bir son koruma kolunu açıkça önerir.
- Impact: Güvenli error-boundary deseni yanlışlıkla ihlal sayılabilir.
- Recommendation: Yasağı exception'ı yutan genel yakalamayla sınırlayın.

