# Analysis — `sync-workflow.md` v1
**Reviewer:** codex · **Run:** v1 · **Date:** 2026-07-21
**Target:** `.agents/rules/sync-workflow.md` (68 lines, 4,242 B) — verified
**Lenses applied:** all five
**Mode:** Analysis (System Analysis seçkisi)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)
Kural ve komut kopyaları için kaynak–hedef ayrımı açık. Salt-okunur hash karşılaştırması `.agents/rules` ile `.claude/rules` ve `.cursor/rules` arasında drift bulmadı; skill wrapper'ları kasıtlı ek dosyalardır.

## OVERALL
Generator çalıştırılmadı; mutasyon riski nedeniyle mimari statik olarak incelendi.

