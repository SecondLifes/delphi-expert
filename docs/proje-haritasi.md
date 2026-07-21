# Proje Haritası — Delphi AI Spec-Kit

> Bu dosya, kitin altındaki her dosya/klasörün **ne işe yaradığını, ne olduğunu ve ne olmadığını** açıklar. Amaç: yeni birinin (insan ya da AI) kaynağı tek tek açmadan, sadece bu haritadan yönünü bulabilmesi. 2026-07-17'de oluşturuldu, 2026-07-19'da workspace'in `batch-script-expert`/`prompt-analyzer-expert` standardına göç ettirildi (bkz. Kök dosyalar ve Araç-özel adaptörler tabloları); yapı değiştikçe güncellenmelidir.
>
> **Güncel kalması nasıl sağlanıyor:** `.agents/rules/`, `.agents/commands/` veya `.agents/skills/` altına bir şey eklenip/çıkarılınca bu dosya da aynı turda elle güncellenir (bkz. `.agents/rules/sync-workflow.md`). `tools/generate-ai-configs.ps1`, her çalıştığında `.agents/` altındaki her dosyanın burada adı geçip geçmediğini kontrol edip eksik olanları **uyarı olarak** yazdırır — ama açıklamayı otomatik yazmaz, sadece unutulmadığını garanti eder.

## Bu kit nedir, ne değildir

**Nedir:** Delphi (Object Pascal) ile kod yazan AI asistanlarına (Claude Code, Cursor, Codex CLI, GitHub Copilot, Gemini/Antigravity, Kiro) verilen bir **davranış/kural şablonu**dur — SOLID, Clean Code, bellek yönetimi, framework-özel (Horse/DMVC/Dext/ACBr/Intraweb/DevExpress), veritabanı-özel (Firebird/PostgreSQL/MySQL) ve threading kuralları içerir.

**Ne değildir:**
- Çalıştırılabilir bir Delphi kütüphanesi/framework'ü değildir — herhangi bir gerçek projeye bağımlılığı yoktur, tamamen bağımsız ve genel amaçlı bir şablondur.
- `examples/` klasörü hariç, derlenebilir/çalışan kod içermez — geri kalan her şey AI'ye yönelik talimat metnidir.

## Mimari — tek cümlede

Kuralların, komutların ve becerilerin (skills) **gerçek içeriği sadece `.agents/` altında yaşar**; `.claude/`, `.cursor/` klasörlerindeki kural dosyaları oradan **otomatik üretilir** (elle düzenlenmez). Nedeni ve mekanizması: [.agents/rules/sync-workflow.md](../.agents/rules/sync-workflow.md).

---

## Kök dosyalar

| Dosya | Ne işe yarar |
|---|---|
| `AGENTS.md` | Codex CLI, Cursor, GitHub Copilot, Gemini/Antigravity ve Kiro'nun **doğrudan okuduğu** evrensel kural özeti — Identity, Skill Check, Working Directory, Proactive Quality Suggestions bölümleri dahil. Detay için `.agents/rules/*.md`'ye yönlendirir. **Elle yazılır, script'ten üretilmez.** |
| `README.md` / `README.tr-TR.md` | İnsan okuyucu için proje tanıtımı, kurulum (Quick Start), kit yapısının genel görünümü — paralel İngilizce/Türkçe çift, aynı "Delphi AI Spec-Kit" başlığı. |
| `LICENSE` | MIT lisansı, telif hakkı sahibi `Delphi Clean Code` — **elle değiştirilmez.** Bu kit [delphicleancode/delphi-spec-kit](https://github.com/delphicleancode/delphi-spec-kit)'in bir fork'udur; orijinal lisans ve telif hakkı korunur (bkz. `CONTRIBUTING.md`'nin "Provenance" bölümü). Diğer workspace kitlerinden (`batch-script-expert`, `prompt-analyzer-expert`) farklı olarak Apache-2.0'a çevrilmedi — bu kasıtlı bir istisna. |
| `CODE_OF_CONDUCT.md` | Contributor Covenant v1.4 — Türkçe çevirisi kasıtlı olarak yok. |
| `CONTRIBUTING.md` / `CONTRIBUTING.tr-TR.md` | "Contributing to Delphi AI Spec-Kit" — hata bildirimi, PR süreci, teknik standartlar, fork kökeni notu. |
| `SECURITY.md` / `SECURITY.tr-TR.md` | Güvenlik açığı bildirme süreci. |
| `ACKNOWLEDGMENTS.md` / `ACKNOWLEDGMENTS.tr-TR.md` | Bu kitin kurallarının/skill'lerinin öğrettiği veya bağımlı olduğu gerçek açık kaynak (Horse, DMVC, Dext Framework, ACBr, DUnitX, Firebird, PostgreSQL, MariaDB) ve ticari (TMS Aurelius, TMS FlexCel, DevExpress VCL, IntraWeb) projeler için "credits" sayfası — README'nin rozet satırından, indeksinden ve footer'ından link verilir. |
| `.gitignore` | Delphi derleme çıktıları, IDE geçici dosyaları vb. için git-ignore kuralları. |
| `.cursorignore` | Cursor'un indekslemeyeceği yollar (derleme çıktıları vb.) — `.agents/`, `tools/` gibi önemli yollar burada asla dışlanmaz. |

## `.agents/` — Tek Kaynak (Single Source of Truth)

Bu klasör kitin **kalbi**dir. Yeni bir kural/komut/skill eklerken/düzenlerken HER ZAMAN burada çalışılır.

### `.agents/rules/` — 16 dosya, konu-bazlı kurallar

| Dosya | Konu |
|---|---|
| `sync-workflow.md` | **Önce bunu oku.** Bu mimarinin nasıl çalıştığı, `.agents` değişince ne yapılması gerektiği. |
| `delphi-conventions.md` | PascalCase, T/I/E/F/A/L prefix'leri, unit bölümleri, formatlama. |
| `memory-exceptions.md` | try/finally zorunluluğu, Interface/ARC, exception yakalama disiplini. |
| `solid-patterns.md` | SOLID prensipleri + Repository/Service kurulum şablonu. |
| `design-patterns.md` | GoF tasarım kalıpları (Factory, Singleton, Strategy, Observer vb.) Pascal örnekleriyle. |
| `refactoring.md` | Code smell kataloğu + Extract Method/Class, Guard Clause teknikleri. |
| `tdd-patterns.md` | DUnitX ile TDD (Red-Green-Refactor), test isimlendirme. |
| `acbr-patterns.md` | ACBr (Brezilya mali otomasyon) bileşenleri için izolasyon kuralları. |
| `dext-patterns.md` | Dext Framework (.NET tarzı DI/ORM backend) — **DevExpress ile karıştırılmamalı**. |
| `dmvc-patterns.md` | DelphiMVCFramework (Controller/ActiveRecord/JWT). |
| `horse-patterns.md` | Horse (minimalist REST framework). |
| `intraweb-patterns.md` | Intraweb (stateful web) — `UserSession` kullanımı, global değişken yasağı. |
| `firebird-patterns.md` | Firebird/FireDAC — Dialect 3, RETURNING+Open, generator'lar. |
| `postgresql-patterns.md` | PostgreSQL/FireDAC — IDENTITY, JSONB, UPSERT. |
| `mysql-patterns.md` | MySQL-MariaDB/FireDAC — utf8mb4, LAST_INSERT_ID(). |
| `threading-patterns.md` | TThread/TTask/PPL, Synchronize/Queue, thread-safety. |

### `.agents/commands/` — 1 dosya

| Dosya | Ne işe yarar |
|---|---|
| `review.md` | `/review` slash-komutunun kaynağı — git diff'i proje kurallarına göre incelet. |

### `.agents/skills/` — 28 klasör, isteğe bağlı yüklenen derin bilgi

Skill'ler, rules'tan farklı olarak **otomatik yüklenmez** — AI, konuyla ilgili bir istek geldiğinde ilgili `SKILL.md`'yi kendisi seçip okur (progressive disclosure). Her skill'in kendi klasöründe `SKILL.md` (giriş noktası) ve bazılarında `references/` (derin referans) bulunur.

**Genel Delphi/mimari (spec-kit'in kendi, 18 adet — bazıları tabloda tek satırda gruplanmıştır):**

| Skill | Ne işe yarar |
|---|---|
| `clean-code` | Kısa metod, anlamlı isim, guard clause disiplini. |
| `delphi-memory-exceptions` | try/finally, exception hiyerarşisi — `memory-exceptions.md` kuralının genişletilmiş hali. |
| `delphi-patterns` | Repository/Service/Factory/Strategy şablonları (kod üretimi için). |
| `design-patterns` | 23 GoF deseninin tam Pascal implementasyonu. |
| `refactoring` | 10 refactoring tekniği, önce/sonra örnekli. |
| `tdd-dunitx` | TDD akışı + Fake/Mock yazma disiplini. |
| `dunitx-testing` | DUnitX proje yapısı, fixture, assertion referansı. |
| `code-review` | Kod inceleme kontrol listesi (güvenlik, performans, SOLID, bellek). |
| `horse-framework`, `dmvc-framework`, `dext-framework` | İlgili REST framework'ün derinlemesine kullanım rehberi. |
| `acbr-components`, `intraweb-framework`, `devexpress-components` | İlgili kütüphane/framework'ün derinlemesine kullanım rehberi. |
| `firebird-database`, `postgresql-database`, `mysql-database` | İlgili veritabanının FireDAC ile derinlemesine kullanımı (bağlantı, migration, hata kodları). |
| `threading` | Threading'in derinlemesine hali (Producer-Consumer, custom thread pool, cancellation token). |

**Genel amaçlı, sonradan eklenen (5 adet — 2026-07-17'de eklendi):**

| Skill | Ne işe yarar | Önemli not |
|---|---|---|
| `aurelius-mapping` | TMS Aurelius ORM ile Delphi sınıflarını veritabanına mapleme (attribute'lar, automapping, inheritance). | Genel — herhangi bir Aurelius projesinde çalışır. |
| `aurelius-objects` | Aurelius `TObjectManager` ile CRUD, transaction, concurrency. | Genel — herhangi bir Aurelius projesinde çalışır. |
| `flexcel-net` | TMS FlexCel ile .NET'ten (C#/VB.NET) Excel okuma/yazma/PDF export. | Genel — Delphi projesiyle ilgisi yok, .NET tarafı için. |
| `flexcel-vcl` | TMS FlexCel ile Delphi/VCL/FMX'ten Excel okuma/yazma/PDF export. | Genel — herhangi bir Delphi projesinde çalışır. |
| `rad-repo-scaffold` *(eski adı: create-ai-repository)* | Bir proje tanımından/spec'ten yola çıkıp minimal, amaca uygun bir AI-repository iskeleti (klasör/dosya planı) üretir. | Genel amaçlı — Delphi'ye özgü değil, Codex için tasarlanmış (`agents/openai.yaml` içerir). |

**Genel-amaçlı workspace araçları (5 adet — daha önceki `rad-prompter`'ın yerine 2026-07-19'da eklendi):**

`rad-template-builder`'ın `blank-scaffold`'undan birebir kopyalanan, workspace'in tüm `spec-kits/*` şablonlarında ortak standart olan dört skill:

| Skill | Ne işe yarar | Önemli not |
|---|---|---|
| `rad-prompt-studio` | AI promptu/kural dosyası/skill'i sıfırdan tasarlar (Design), mevcudunu beş-mercek (Prompt Engineer & Analyst, Repo Auditor, DevOps/Config Engineer, Systems Forensics Analyst, Context Engineer) kimliğiyle analiz eder — tek dosya veya tüm proje taraması (Analysis) — ve zorunlu analiz + (varsa önceki analizlerin) değerlendirme geçişi ile kullanıcı onayından sonra güvenli biçimde düzenler (Edit). | 2026-07-20'de bir üst-versiyon skill'in yerini aldı — beş mercek referans dosyası (`references/five-lenses.md`) ve üç mod için ayrı master prompt'lar (`references/prompts/{analysis,evaluation,edit}-base-prompt.md`) güncel ve bağımsız gömülü kopyadır, dış bir yola bağlı değildir. `AGENTS.md`'nin başındaki not, bu skill'e herhangi bir şekilde referans verilmesini beş merceği birden yükleme talimatı sayıyor. |
| `rad-skill-finder` | Sıfırdan yazmadan önce ilgili bir skill olup olmadığını kontrol eder — `AGENTS.md`'nin "Skill Check (Mandatory)" bölümünün dayandığı araç. | Delphi'ye özgü değil, genel amaçlı. |
| `rad-python` | AI'nin yardımcı script yazması gerektiğinde (ör. bir analiz sonucunu işleyen tek seferlik bir script) kullanılır. | Delphi'ye özgü değil, genel amaçlı. |
| `rad-web-scraping` | Web'den yapılandırılmış veri çıkarma — bu kitin ana konusuyla doğrudan ilgili değil, `blank-scaffold`'un varsayılan bundle'ının bir parçası olarak geldi; kullanılmazsa zararsız. | Delphi'ye özgü değil, genel amaçlı. |
| `rad-powershell-master` | PowerShell 7+ betik/modül/cmdlet uzmanlığı, CI/CD (GitHub Actions/Azure DevOps) entegrasyonu, cross-platform scripting, PSGallery modül keşfi. | Workspace'in kendi `.agents/skills/rad-powershell-master/`'ının (kaynağı: `josiahsiegel/claude-plugin-marketplace`, upstream adı `powershell-master`) birebir kopyası — 2026-07-19'da eklendi. Bu kitte doğrudan kullanım alanı var: `tools/generate-ai-configs.ps1` PowerShell'dir, bu skill onu yazan/düzenleyen AI için referans niteliğinde. `blank-scaffold`'un varsayılan 4-skill bundle'ının bir parçası değil — kullanıcı özellikle bu kit için istedi. |

---

## Araç-özel adaptörler

Bu klasörlerin çoğu **üretilmiş** (generated) içerik barındırır — kaynağı `.agents/`'tır, elle düzenlenmez.

| Klasör/Dosya | Durum | Ne işe yarar |
|---|---|---|
| `.claude/CLAUDE.md` | Elle yazılır | Claude Code'un otomatik okuduğu kök talimat — proje özeti + `.agents/` mimarisine yönlendirme. |
| `.claude/settings.json` | Elle yazılır | İzin ayarları (`allowCommands`, `denyPaths`). |
| `.claude/rules/*.md` (16 dosya) | ⚙️ **Üretilmiş** | `.agents/rules/`'ın birebir kopyası — Claude Code'un native olarak buradan okuduğu format. |
| `.claude/commands/review.md` | ⚙️ **Üretilmiş** | `.agents/commands/review.md`'nin kopyası — `/review` komutu. |
| `.claude/commands/<skill-adı>.md` (28 dosya) | ⚙️ **Üretilmiş** | Her `.agents/skills/*` klasörü için otomatik üretilen ince komut sarmalayıcısı (ör. `/rad-prompt-studio`) — skill'i doğal-dil eşleşmesine güvenmeden, deterministik şekilde ilk adımından başlatır. Sadece Claude Code'da var (Cursor'da henüz komut klasörü yok). |
| `.cursor/rules/*.md` (16 dosya) | ⚙️ **Üretilmiş** | `.agents/rules/`'ın Cursor formatındaki kopyası. |
| `.gemini/rules/project-rules.md` | Elle yazılır | Gemini/Antigravity için `AGENTS.md`'ye benzer, Gemini'ye özel kısa özet — Identity/Skill Check/Working Directory/Proactive Quality Suggestions dahil. İsim, workspace'teki diğer kitlerle tutarlılık için 2026-07-19'da `delphi-rules.md`'den değiştirildi. |
| `.github/copilot-instructions.md` | Elle yazılır | GitHub Copilot'un workspace'e enjekte ettiği ön-prompt; `AGENTS.md`'ye referans verir, tekrar etmez. |
| `.kiro/steering/*.md` (4 dosya: `product.md`, `tech.md`, `structure.md`, `frameworks.md`) | Elle yazılır | Kiro AI'nin "steering" (yönlendirme) dokümanları — kural değil, daha çok "proje bağlamı" niteliğinde, bu yüzden `.agents/` senkron şemasının dışında tutuluyor. |
| `.specify/*.md` (4 şablon) | Elle yazılır | Spec-driven geliştirme şablonları (`constitution.md`, `spec-template.md`, `plan-template.md`, `tasks-template.md`) — büyük bir özellik eklerken kod yazmadan önce doldurulması önerilen, opsiyonel şablonlar. |
| `.vscode/settings.json` | Elle yazılır | VS Code'un `files.exclude`/`search.exclude` ayarları (derleme çıktılarını gizler). |

## `tools/`

| Dosya | Ne işe yarar |
|---|---|
| `generate-ai-configs.ps1` | `.agents/rules` ve `.agents/commands`'ı okuyup `.claude/rules`, `.cursor/rules`, `.claude/commands`'a kopyalayan PowerShell script. Ayrıca `.agents/skills/*` altındaki her klasör için `.claude/commands/<skill-adı>.md` adında ince bir komut sarmalayıcısı üretir (skill'i `/<skill-adı>` ile doğrudan, adım sırasını bozmadan çağırabilmek için) — isim bir hand-authored komutla çakışırsa o skill için üretim atlanır ve uyarı basılır. `.agents/rules`, `.agents/commands` altında bir dosya eklenip/silinip/değiştirildiğinde VEYA `.agents/skills` altına bir skill eklenip/kaldırıldığında çalıştırılması **zorunludur** (bkz. `sync-workflow.md`). |

## `docs/`

| Dosya | Ne işe yarar |
|---|---|
| `proje-haritasi.md` | Bu dosya. |
| `ai-ignore-strategy.md` | Hangi dosyaların AI bağlamından hariç tutulacağı/tutulmayacağı stratejisi (`.gitignore`, `.cursorignore`, `.vscode/settings.json` ile ilişkisi). |
| `delphi-expert-analysis.md` | Beş-mercek öz-denetimi (Prompt Engineer & Analyst, Repo Auditor, DevOps/Config Engineer, Systems Forensics Analyst, Context Engineer) — bu kitin 2026-07-19 göçünden sonraki durumunu değerlendirir. |
| `Prompts/image-prompts.md` | README banner'ları için 3 AI görsel üretim prompt'u (Overview, Core Features, Design & Philosophy) — accent renk "deep red and burnt-orange" (Delphi'nin klasik kırmızı alev logosu), Image 2'nin özellik listesi bu kitin gerçek kurallarını (zero-leak bellek yönetimi, SOLID/DI, TDD red-green-refactor, katmanlı mimari) görsel metaforlarla anlatır. Gerçek PNG'lerin üretimi kapsam dışı — README'deki üç HTML-comment placeholder, görsel üretilene kadar yorumda kalır. |

## `src/`

AI'nin ürettiği her şey için varsayılan çalışma/çıktı dizini — `AGENTS.md`'nin "Working Directory" bölümü bkz. `examples/`'dan farkı: `examples/` küratörlü, kalıcı referans örnekler; `src/` geçici bir çalışma alanı. İçinde önerilen katman yapısı (`Domain/`, `Application/`, `Infrastructure/`, `Presentation/`) `AGENTS.md`'nin "Layer Structure (Architecture)" bölümüyle birebir eşleşir.

## `examples/`

Derlenebilir/örnek Pascal kaynak kodu — kural dosyalarındaki kısa kod parçacıklarının aksine, burada **tam çalışan örnekler** var. AI, bir desenin gerçek uygulamasını görmek istediğinde buraya bakar.

- **Düz dosyalar (16 adet):** Her biri tek bir konuyu (örn. `repository-pattern.pas`, `threading-example.pas`, `design-patterns-example.pas`, `tdd-dunitx-example.pas`, framework-özel `horse-api-example.pas`/`dmvc-controller-example.pas`/`dext-api-example.pas`/`acbr-service-example.pas`/`intraweb-form-example.pas`, veritabanı-özel `firebird-/postgresql-/mysql-repository-example.pas`) tek dosyada gösterir.
- **`file-copy-app/`:** Servis katmanlı, testli, uçtan uca küçük bir örnek uygulama (dosya kopyalama).
- **`i18n-app/`:** Çok dilli (i18n) bir örnek uygulama — kaynak, view, test ve veritabanı şeması dahil tam proje iskeleti. 2026-07-19'da 3 IDE/derleme kalıntısı dosyası (`I18nApp.dproj.local`, `I18nApp.identcache`, `I18nApp.res`) temizlendi — bunlar kitin kendi `.gitignore`/`.cursorignore`/`AGENTS.md` "AI Must Never Use as Context" listesindeki desenlerle çelişiyordu.
