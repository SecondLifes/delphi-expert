# Analysis — `clean-code` v1
**Reviewer:** codex · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/skills/clean-code/` (1 file; 149 lines, 4,694 B) — verified
**Lenses applied:** all five
**Mode:** Analysis (System Analysis seçkisi)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)
Kod ve bellek kuralları açıktır; bağımlıları aynı görevde değiştirmeyi zorunlu tutan kural kullanıcı kapsamını genişletebilir.

### [WARNING-1] Bağımlı düzenleme emri koşulsuz
- Category: WARNING
- Lens(es): Prompt Engineer & Analyst, Context Engineer
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/skills/clean-code/SKILL.md:128`
- Finding: “Edit the file + all dependents in the SAME task” kuralı, gerekli ve yetkili kapsamı ayırmaz.
- Evidence: Metinde kapsam veya onay istisnası yoktur.
- Impact: İstenmeyen geniş dosya değişikliklerine yol açabilir.
- Recommendation: Yalnızca gerekli ve kullanıcı kapsamındaki bağımlıları güncelleme koşulu ekleyin.

