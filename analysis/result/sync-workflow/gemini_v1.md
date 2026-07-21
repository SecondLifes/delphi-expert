# Analysis — sync-workflow v1
**Reviewer:** Antigravity (gemini_v1) · **Run:** v1 · **Date:** 2026-07-21T21:42:00+03:00
**Target:** `.agents/rules/sync-workflow.md` (69 lines, 4242 B) — verified
**Lenses applied:** all five | System Analysis Mode
**Mode:** Analysis ("review this file" -> Analysis per prompt-engineer-analyst.md:58-60)

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu dosya, repodaki kural (rules), komut (commands) ve yetenek (skills) dosyalarının farklı yapay zeka araçları (Claude, Cursor, Copilot vb.) arasında nasıl senkronize edildiğini açıklar. Ana kaynak olan `.agents/` klasöründeki değişikliklerin ardından `tools/generate-ai-configs.ps1` betiğinin çalıştırılması zorunluluğu anlatılmakta ve Windows'ta symlink kullanımından doğabilecek kopyalama sorunları nedeniyle bu betiğe ihtiyaç duyulduğu gerekçelendirilmektedir. Genel yapı ve Devops prensipleri açısından oldukça sağlam ve açıklayıcıdır.

### [OVERALL] Strong Sync / Generator Architecture Definition

- Category: OVERALL
- Lens(es): DevOps/Config Engineer, Systems Forensics Analyst, Prompt Engineer & Analyst
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: entire file
- Finding: The document effectively establishes a single source of truth and clearly details the downstream generator pipeline.
- Evidence: Explicit instructions warn against editing `.claude/rules/*.md` and `.cursor/rules/*.md`, establishing the `.agents/` directory as canonical. The justification for avoiding symlinks is practical and accurate for cross-platform Git cloning.
- Impact: Protects the repository from silent file drift and ensures users know exactly how to propagate their AI configuration changes.
- Recommendation: Keep as is. It correctly models an idempotent sync architecture and explains the boundaries well.

## Coverage Manifest
- `.agents/rules/sync-workflow.md` — 69 lines, read in full
