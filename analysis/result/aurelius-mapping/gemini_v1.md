# Analysis — aurelius-mapping v1
**Reviewer:** Antigravity (gemini) · **Run:** v1 · **Date:** 2026-07-21T21:37:55+03:00
**Target:** `.agents/skills/aurelius-mapping` (884 lines total) — verified
**Lenses applied:** all five | Deep traversal of skill directory per Golden Rule 2
**Mode:** Analysis | "analiz et" → Analysis per prompt-engineer-analyst.md:58-60

## Kısa Özet (Türkçe — bilgilendirme amaçlı, değerlendirilmez)

Bu yetenek, TMS Aurelius ORM kullanımıyla Delphi sınıflarının veritabanı tablolarına nasıl haritalanacağını (mapping) açıklar. Hem otomatik eşleme (Automapping) hem de manuel (explicit) eşleme özellikleri çok iyi belgelenmiştir. Koleksiyonların yaşam döngüsü, ilişkilerin (association) yönetimi ve `RegisterEntity` gereksinimleri gibi kritik konular eksiksiz bir biçimde aktarılmış.

### OVERALL

The `aurelius-mapping` skill provides an exceptionally thorough and correct guide to using TMS Aurelius mapping attributes. It explicitly differentiates between Automapping and Explicit mapping approaches and provides clear, strict rules for the most common lifetime management pitfalls involving lazy collections and eager associations.

### [ADVISORY] Incomplete interface exposing in child entity example

- Category: ADVISORY
- Lens(es): Prompt Engineer & Analyst, Repo Auditor
- Verification verdict: VERIFIED
- Evidence type: static-review-based
- Location: `.agents/skills/aurelius-mapping/references/mapping.md` lines 435-442
- Finding: In the "Bidirectional (recommended)" example, `TMediaFileChild` maps a many-to-one relationship using `Proxy<TAlbumRef>` (`FAlbum: Proxy<TAlbumRef>;`). However, it doesn't expose this field as a property via a getter/setter to be used by consumers of the class, unlike other examples in the documentation which use a `function GetAlbum: TAlbumRef;` and expose it via a property.
- Evidence: 
  ```delphi
  // Child class:
    TMediaFileChild = class
    private
      [Association([TAssociationProp.Lazy], CascadeTypeAllButRemove)]
      [JoinColumn('ID_ALBUM', [])]
      FAlbum: Proxy<TAlbumRef>;
    end;
  ```
- Impact: It might mislead a developer into leaving the proxy field inaccessible, rendering the bidirectional relationship unusable from the child object's side in application code.
- Recommendation: Add a property `Album: TAlbumRef read GetAlbum write SetAlbum;` along with the corresponding methods, matching the pattern established in the previous "Lazy-Loading an Association" section.

## Coverage Manifest
- `.agents/skills/aurelius-mapping/SKILL.md` — 56 lines, read in full
- `.agents/skills/aurelius-mapping/references/mapping.md` — 828 lines, read in full
