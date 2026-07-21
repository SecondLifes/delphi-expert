# 🚀 Delphi AI Spec-Kit

<div align="center">

**Delphi geliştirmesini Yapay Zeka ile state-of-the-art seviyeye taşıyan; kurallar, *skill*'ler ve *steering*'lerden oluşan uçtan uca bir ekosistem.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Delphi](https://img.shields.io/badge/Delphi-Object%20Pascal-red?logo=delphi)](https://www.embarcadero.com/products/delphi)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Ready-blue?logo=github)](https://github.com/features/copilot)
[![Cursor](https://img.shields.io/badge/Cursor-Rules-purple)](https://cursor.sh)
[![Claude](https://img.shields.io/badge/Claude-Code-brown?logo=anthropic)](https://claude.ai)
[![Gemini](https://img.shields.io/badge/Gemini-Skills-orange?logo=google)](https://gemini.google.com)
[![Kiro](https://img.shields.io/badge/Kiro-Steering-teal)](https://kiro.dev)

*[🇬🇧 English](README.md) · [Katkıda Bulunma](CONTRIBUTING.tr-TR.md) · [Davranış Kuralları](CODE_OF_CONDUCT.md) · [Güvenlik](SECURITY.tr-TR.md) · [Teşekkürler](ACKNOWLEDGMENTS.tr-TR.md)*

![Genel Bakış](docs/images/overview.png)

</div>

## Sponsorluk

**Delphi/Lazarus bileşenleri**
<www.inovefast.com.br>

Ödeme platformları ve servisleriyle entegrasyonlar (Asaas, MercadoPago, Cielo, PagSeguro, D4sign, Webstore, MelhorEnvio, Groq)

**i9DBTools**
<www.inovefast.com.br/i9dbTools/>
MySQL, PostgreSQL, Firebird ve SQLite'ı tek bir yerden yönetin; doğal dilde SQL üretip açıklayan, sorguları optimize eden ve saniyeler içinde Brezilya'ya özgü sahte veri üreten yapay zeka ile.

## 📋 İçindekiler

- [Bu proje nedir?](#-bu-proje-nedir)
- [Neden kullanmalı?](#-neden-kullanmali)
- [Desteklenen AI Araçları](#-desteklenen-ai-araçları)
- [Öğretilen Ana Kurallar](#-yapay-zekaya-öğretilen-ana-kurallar)
- [Desteklenen Framework'ler](#️-desteklenen-framework-ve-kütüphaneler)
- [Kit Yapısı](#-kit-yapisi)
- [Hızlı Başlangıç](#-hızlı-başlangıç)
- [İyi Pratik Örnekleri](#-iyi-pratik-örnekleri)
- [Tasarım ve Felsefe](#-tasarım-ve-felsefe)
- [Teşekkürler](#-teşekkürler)
- [Katkılar](#-katkılar)

---

## 💡 Bu proje nedir?

**Delphi AI Spec-Kit**, bir kod framework'ü değil — favori yapay zekanız için bir **davranış kılavuzu** setidir. Sihirbaza Delphi kodunu şu şekilde yazmayı "öğretir":

- ✅ **Temiz** — *god class* yok, `OnClick` içinde iş mantığı yok
- ✅ **Güvenli** — `try..finally` ve (ARC) interface'lerle sıfır *memory leak*
- ✅ **Test edilebilir** — DUnitX ile TDD, interface üzerinden Fake'ler, testlerde gerçek veritabanı yok
- ✅ **Mimarili** — SOLID, DDD, Repository/Service Pattern ve *clean architecture*

> Veritabanı erişimini sunum katmanıyla karıştıran, `try..finally`'yi unutan veya Dependency Injection'ı görmezden gelen yapay zekaya artık elveda.

---

## 🤔 Neden kullanmalı?

| Spec-Kit olmadan | Spec-Kit ile |
|---|---|
| AI, `OnClick` içinde mantık üretir | AI katmanları doğru şekilde izole eder |
| `try..finally` olmadan `TStringList.Create` | Bellek altın standardı her zaman uygulanır |
| Gerçek bankaya bağlı testler | Interface üzerinden Fake'ler, hızlı ve izole testler |
| Tutarsız isimlendirme | `A`-parametreler, `F`-alanlar, `T`-tipler, metotlarda fiiller |
| `with` ifadesi ve global değişkenler | Code smell'ler proaktif olarak engellenir |

---

## 🤖 Desteklenen AI Araçları

| Araç | Yapılandırma Dosyası | Nasıl Çalışır |
|---|---|---|
| **GitHub Copilot** | `.github/copilot-instructions.md` | Workspace/Chat'e enjekte edilen pre-prompt |
| **Cursor** | `.cursor/rules/*.md` (üretilmiş) | Bağlama göre yüklenen kurallar |
| **Claude Code** | `.claude/` (kurallar üretilmiş, skill'ler paylaşımlı) | Bağlama göre kurallar ve terminaldeki skill'ler |
| **Codex CLI** | `AGENTS.md` | Doğrudan okur, özel klasör gerekmez |
| **Google Gemini / Antigravity** | `.gemini/rules/project-rules.md` | `AGENTS.md` gibi özet, yoğunlaştırılmış kurallar |
| **Kiro AI** | `.kiro/steering/*.md` | Stack ve mimari kısıtlar |
| **Herhangi bir AI** | `AGENTS.md` | Evrensel kurallar (proje kökü) |
| **Yukarıdakilerin hepsi** | `.agents/skills/*/SKILL.md` | Paylaşımlı skill'ler — Agent Skills açık standardı, her araç için tek kopya |

> Kurallar ve komutların tek kanonik kaynağı `.agents/rules/` ve
> `.agents/commands/`'tır; `.claude/rules`, `.cursor/rules` ve
> `.claude/commands` bundan `tools/generate-ai-configs.ps1` tarafından
> üretilir — bkz. `.agents/rules/sync-workflow.md`.

---

## 🌟 Yapay Zekaya Öğretilen Ana Kurallar

![Temel Özellikler](docs/images/core-features.png)

### 🧠 Sıfır-Leak Bellek Yönetimi

AI şu deseni zorunlu kılar: *Owner*'ı olmayan her `.Create`, **hemen** ardındaki satırda `try..finally` gerektirir. Ayrıca manuel `Free` olmadan otomatik yaşam döngüsü yönetimi için **Interface**'lerin (ARC referans sayımı) kullanımını öğretir.

```pascal
//✅ Altın Standart — Spec-Kit'li AI tarafından HER ZAMAN üretilir
var LList: TStringList;
begin
  LList := TStringList.Create;
  try
    LList.Add('item');
  finally
    LList.Free;
  end;
end;
```

### 🧪 DUnitX ile TDD

Interface başına izole Fake'lerle *Red-Green-Refactor* akışı. Testlerde veritabanına bağımlılık yok.

```pascal
[Test]
procedure ProcessOrder_WithoutStock_RaisesException;
begin
  Assert.WillRaise(
    procedure begin FSut.Process(FEmptyOrder); end,
    EInvalidOrderException
  );
end;
```

### 🏛️ SOLID ve DDD

- **S** — Tek sınıf, tek sorumluluk. `TCustomerValidator` bankaya kaydetmez.
- **O** — Mevcut kodu değiştirmeden interface'ler üzerinden genişletme.
- **L** — Sadece net bir sözleşmeyle kalıtım. Interface'ler tercih edilir.
- **I** — Küçük ve spesifik interface'ler. Dev interface'lerden kaçının.
- **D** — Constructor'da dependency injection, asla hardcoded somut instance'lar değil.

```pascal
//✅ Pratikte DIP
constructor TOrderService.Create(
  ARepo: IOrderRepository;
  ANotifier: INotificationService);
begin
  FRepo := ARepo;
  FNotifier := ANotifier;
end;
```

### 📖 Clean Code — Pascal Kılavuzu

Tutarlı ve zorunlu isimlendirmeler:

| Kategori | Kural | Örnek |
|---|---|---|
| Parametreler | `A` öneki | `ACustomerName` |
| Private alanlar | `F` öneki | `FCustomerName` |
| Yerel değişkenler | `L` öneki | `LCustomer` |
| Sınıflar | `T` öneki | `TCustomerService` |
| Interface'ler | `I` öneki | `ICustomerRepository` |
| Exception'lar | `E` öneki | `ECustomerNotFound` |

---

## 🛠️ Desteklenen Framework ve Kütüphaneler

| Framework | Alan | Dahil Edilen Kurallar |
|---|---|---|
| **Horse** | Minimalist REST API'ler | Controller/Service/Repository yapısı, middleware |
| **Dext Framework** | .NET tarzı API'ler, ORM, DI, Async | Minimal API'ler, Entity ORM, `TAsyncTask.Run` |
| **DelphiMVC (DMVC)** | Attribute'lu REST API'ler | `[MVCPath]`, Active Record, JWT, RQL |
| **ACBR** | Ticari Otomasyon (NFe, CF-e, Boleto) | UI ile karışmadan vergi izolasyonu |
| **Intraweb** | Delphi'de Stateful WebApp'ler | `UserSession`, global session değişkeni yok |
| **DevExpress** | Gelişmiş Kurumsal UI | `TcxGrid`, `TdxLayoutControl`, skin'ler ve export |
| **Firebird Database** | Kurumsal Veritabanı | FireDAC bağlantısı, PSQL, generator'lar, transaction'lar, migration'lar |
| **PostgreSQL Database** | Modern Veritabanı | FireDAC bağlantısı, UPSERT, JSONB, Full-Text Search, PL/pgSQL |
| **MySQL / MariaDB** | Popüler Veritabanı | FireDAC bağlantısı, AUTO_INCREMENT, UPSERT, JSON, FULLTEXT |
| **DUnitX** | Unit Testler | Red-Green-Refactor, interface üzerinden Fake'ler |
| **Design Patterns GoF** | Tasarım Kalıpları | Interface ve ARC ile Creational, Structural ve Behavioral |
| **Threading** | Multi-Threading | TThread, TTask, Synchronize/Queue, TCriticalSection, PPL |
| **Code Refactoring** | Code Smell'ler ve Teknikler | Extract Method/Class, Guard Clauses, Strategy, Parameter Object |

---

## 📂 Kit Yapısı

```
delphi-spec-kit/
│
├── AGENTS.md                        # 🌐 Evrensel kurallar (Codex, Copilot, Kiro, Antigravity, Gemini)
│
├── .agents/                         # 📦 TEK GERÇEK KAYNAK — sadece burada düzenlenir
│   ├── rules/                       # Konuya özel kurallar (15 konu dosyası + sync-workflow.md)
│   ├── commands/
│   │   └── review.md                # Slash-komut kaynağı: /review
│   └── skills/                      # İsteğe bağlı skill'ler (klasör başına SKILL.md) — Claude Code,
│                                     # Cursor, Codex CLI, Copilot ve Gemini/Antigravity tarafından
│                                     # doğrudan okunur; 22 Delphi-özgü skill + 6 genel-amaçlı
│                                     # skill (rad-repo-scaffold, rad-prompt-studio,
│                                     # rad-skill-finder, rad-python, rad-web-scraping,
│                                     # rad-powershell-master)
│
├── tools/
│   └── generate-ai-configs.ps1      # .claude/rules, .cursor/rules, .claude/commands'ı
│                                     # .agents/'ten yeniden üretir
│
├── .claude/                         # CLAUDE.md + üretilmiş rules/commands
├── .github/                         # copilot-instructions.md (elle yazılır)
├── .cursor/                         # üretilmiş rules
├── .gemini/rules/project-rules.md   # Elle yazılan özet, Gemini'ye özel AGENTS.md muadili
├── .kiro/steering/                  # product.md, tech.md, structure.md, frameworks.md
├── .specify/                        # AI destekli spec şablonları
├── src/                             # AI'nin ürettiği her şey için varsayılan çalışma dizini
├── docs/                            # proje-haritasi.md, ai-ignore-strategy.md, beş-mercek öz-denetimi
└── examples/                        # Curated Delphi kod örnekleri (17 dosya + 2 tam uygulama)
```

> Tam ve güncel dosya-dosya döküm için [docs/proje-haritasi.md](docs/proje-haritasi.md)'ye bakın.

---

## ⚡ Hızlı Başlangıç

### 1. Kiti klonlayın veya indirin

```bash
git clone https://github.com/delphicleancode/delphi-spec-kit.git
```

### 2. Delphi projenizin köküne kopyalayın

```
YourProject/
├── MyApp.dpr
├── AGENTS.md          ← kökten kopyalayın
├── .agents/           ← klasörü kopyalayın (tek gerçek kaynak: kurallar, komutlar, skill'ler)
├── tools/             ← klasörü kopyalayın (generate-ai-configs.ps1)
├── .claude/           ← klasörü kopyalayın (üretilmiş kurallar/komutlar dahil)
├── .github/           ← klasörü kopyalayın
├── .cursor/           ← klasörü kopyalayın (üretilmiş kurallar dahil)
├── .gemini/           ← klasörü kopyalayın
├── .kiro/              ← klasörü kopyalayın
└── .specify/           ← klasörü kopyalayın (opsiyonel — spec şablonları)
```

`.agents/rules/` veya `.agents/commands/` altında sonradan dosya eklerseniz
ya da düzenlerseniz, `.claude/rules`, `.cursor/rules` ve
`.claude/commands`'ı tazelemek için proje kökünden
`pwsh tools/generate-ai-configs.ps1`'i yeniden çalıştırın.

### 3. AI kuralları otomatik olarak devralır

- **Claude Code** — `.claude/CLAUDE.md`'yi uygular, `.claude/rules/*.md`'yi (üretilmiş) ve `.agents/skills/*/SKILL.md`'yi doğrudan okur
- **Cursor** — `.cursor/rules/*.md`'yi (üretilmiş) bağlama göre otomatik okur
- **Codex CLI** — Proje kökündeki `AGENTS.md`'yi, artı `.agents/skills/*/SKILL.md`'yi okur
- **GitHub Copilot** — Workspace'teki `.github/copilot-instructions.md`'yi, artı `.agents/skills/*/SKILL.md`'yi okur
- **Antigravity / Gemini** — `.gemini/rules/project-rules.md`'yi, artı `.agents/skills/*/SKILL.md`'yi okur
- **Kiro** — `.kiro/steering/*.md`'yi sabit ürün bağlamı olarak okur

> **Ek yapılandırma gerekmez.** Projeyi açın, tercih ettiğiniz AI'yı kullanın ve farkı görün.

---

## 💡 İyi Pratik Örnekleri

### Katman Mimarisi

```
src/
├── Domain/         ← Entity'ler, Value Object'ler, Repository Interface'leri
├── Application/    ← Service'ler, Use Case'ler, DTO'lar
├── Infrastructure/ ← FireDAC Repository'ler, harici API'ler
└── Presentation/   ← VCL/FMX Form'lar, ViewModel'ler
tests/
└── Unit/           ← İzole Fake'lerle DUnitX projeleri
```

> **Bağımlılık kuralı:** `Presentation → Application → Domain ← Infrastructure`
> **Domain, hiçbir zaman** diğer katmanlara bağımlı değildir.

### Guard Clause'lar (gereksiz iç içe geçme yok)

```pascal
procedure ProcessOrder(AOrder: TOrder);
begin
  if not Assigned(AOrder) then
    raise EArgumentNilException.Create('AOrder cannot be nil');
  if AOrder.Items.Count = 0 then
    raise EBusinessRuleException.Create('Order must have at least one item');
  if not AOrder.IsValid then
    raise EValidationException.Create('Order validation failed');

  //gerçek mantık burada, iç içe geçme yok
  FRepository.Save(AOrder);
  FNotifier.Send(AOrder.Customer.Email);
end;
```

### Interface Üzerinden Fake ile Test

```pascal
type
  TFakeOrderRepository = class(TInterfacedObject, IOrderRepository)
  private
    FOrders: TObjectList<TOrder>;
  public
    constructor Create;
    destructor Destroy; override;
    procedure Save(AOrder: TOrder);
    function FindById(AId: Integer): TOrder;
  end;

[TestFixture]
TOrderServiceTest = class
private
  FSut: TOrderService;
  FRepo: IOrderRepository;
public
  [Setup]
  procedure SetUp;
  [Test]
  procedure PlaceOrder_ValidOrder_SavesToRepository;
  [Test]
  procedure PlaceOrder_EmptyItems_RaisesException;
end;
```

---

## 🎯 Tasarım ve Felsefe

![Tasarım ve Felsefe](docs/images/design-philosophy.png)

**İnşa Yoluyla Sıfır-Leak.**

Bu kit, her şeyin üzerinde tek bir taviz verilmez değeri optimize eder: bir
handle'ı, bir nesneyi veya bir veritabanı bağlantısını asla sızdırmayan bir
Delphi kod tabanı — "genellikle temizler" değil, yapısal olarak unutmaya
*muktedir olmayan*. Bellek kuralının (her `.Create`'in hemen ardındaki
satırda `try..finally`) bir stil önerisi değil sıkı bir desen olarak
zorunlu kılınmasının nedeni budur; service ve repository'ler için manuel
`Free` çağrıları yerine interface'lerin (ARC) varsayılan olarak
öğretilmesinin nedeni de budur. Kasıtlı tercih şu: bu kit, minimal bir
örneğin kesinlikle ihtiyaç duyduğundan daha fazla `try..finally` bloğuna
ve daha fazla interface dolaylamasına doğru itmeye devam edecek — çünkü
uzun ömürlü bir Delphi kod tabanında, atlanan bir `Free`'nin bedeli
yıllarca sessizce katlanarak büyürken, fazladan bir `try..finally`'nin
bedeli yalnızca yazım anında, bir kez ödenir ve bir daha asla ödenmez.

---

## 🚫 AI Ignore / Bağlam Kontrol Listesi

Bu proje, AI ajanlarının neyi indeksleyip bağlam olarak kullanacağını kontrol eden çok katmanlı bir strateji uygular. PR göndermeden önce:

- [ ] Yeni bir alt projenin build çıktı klasörleri `.gitignore` tarafından kapsanıyor
- [ ] `.cursorignore`, yeni ağır veya binary yolları içeriyor
- [ ] Temel talimat dosyaları (`AGENTS.md`, kurallar, skill'ler, örnekler) **hariç tutulmuyor**
- [ ] `.vscode/settings.json`, yeni artifact tipleri için güncel
- [ ] Hiçbir secret (`*.key`, `*.pfx`, `.env`) commit edilmiyor veya referans verilmiyor

> Tam gerekçe ve bakım kılavuzu için [docs/ai-ignore-strategy.md](docs/ai-ignore-strategy.md)'ye bakın.

---

## 🙏 Teşekkürler

Bu kitin üzerine inşa edildiği açık kaynak projeler, ticari araçlar ve
referanslar için [ACKNOWLEDGMENTS.tr-TR.md](ACKNOWLEDGMENTS.tr-TR.md)'ye bakın.

---

## 🤝 Katkılar

Pull Request'ler memnuniyetle karşılanır! Favori Delphi framework'ünüz veya kütüphaneniz AI için bir kılavuza ihtiyaç duyuyorsa, ekleyin:

1. **Kural** → `.agents/rules/framework-adiniz.md`, ardından `.claude/rules/` ve `.cursor/rules/`'u yeniden üretmek için `pwsh tools/generate-ai-configs.ps1` çalıştırın — bu iki klasörü doğrudan elle düzenlemeyin, değişikliğiniz bir sonraki çalıştırmada üzerine yazılır.
2. **Skill** → `.agents/skills/framework-adiniz/SKILL.md` (tek kopya, her destekli araç tarafından doğrudan okunur — çoğaltılacak içerik yok, ama Claude Code'un eşleşen `/framework-adiniz` komut sarmalayıcısını da alması için sonrasında `pwsh tools/generate-ai-configs.ps1` çalıştırın).
3. **Referans** → `AGENTS.md`'de (framework/veritabanına özgüyse `.gemini/rules/project-rules.md`'de de, mevcut girdilerle tutarlı şekilde) bahsedin.

Katkıda bulunma sürecinin tamamı için [CONTRIBUTING.md](CONTRIBUTING.md) / [CONTRIBUTING.tr-TR.md](CONTRIBUTING.tr-TR.md)'ye bakın.

### Nasıl katkıda bulunulur

```bash
# Fork'layın ve klonlayın
git fork https://github.com/delphicleancode/delphi-spec-kit
git clone https://github.com/YOUR-FORK/delphi-spec-kit

# Açıklayıcı bir branch oluşturun
git checkout -b feat/add-remobjects-patterns

# Commit'leyin ve Pull Request açın
git commit -m "feat: add RemObjects SDK patterns"
git push origin feat/add-remobjects-patterns
```

---

## 🗣️ Kullanabileceğiniz AI Komutları

**Bu kit'in kendisini** çalışma klasörü olarak herhangi bir desteklenen AI CLI ile açın (Claude Code, Codex, Gemini/Antigravity, Cursor) — aşağıdaki komutlar, pakete gömülü `rad-prompt-studio` skill'i ve kit'in kendi `AGENTS.md`'si üzerinden yerel çalışır:

| Siz derseniz | Ne olur |
|---|---|
| `Sistemi analiz et` | Bu kit'in kendi sistem katmanını analiz eder (`.agents/skills/`, `.agents/rules/`, `.agents/commands/`, `AGENTS.md`, `.claude/CLAUDE.md`) — `examples/`, `docs/`, `src/`, `tools/` siz istemedikçe kapsam dışıdır. Rapor kit'in kendi `analysis/result/{ai}_v{n}.md` klasörüne düşer — yerel bir çalışma dosyasıdır, bilerek gitignore'lanmıştır; uygulanan düzeltmelerin kalıcı kaydı git geçmişi + issue'lar + CHANGELOG'dur. |
| `Değerlendir` | `analysis/result/` içindeki mevcut raporları güncel içerikle karşılaştırıp not verir (`STILL_VALID`/`STALE`/`REFUTED`...), düzeltme listesini sunar ve onayınızı bekler. |
| `Düzelt: <hedef>` | Onay-kapılı düzenleme: analiz → eski raporların değerlendirmesi → açık onayınız → düzenleme. Düzenlenen dosya paylaşılan gömülü bir skill (`rad-*`) ise ve bu kit, üst AI-Spec-Kits-Maker workspace'inin içindeyse, aynı düzeltme üstteki master kopyaya da uygulanır — iki taraf hep güncel kalır. |
| `<konu> için skill var mı?` | Gömülü `rad-skill-finder` yerel → `npx skills` ekosistemi → dizinler → web sırasıyla arar; onayınız olmadan asla kurulum yapmaz. |

---

<div align="center">

Pix ile yazara bir kahve ısmarlayın: <pix@inovefast.com.br> ☕

**Delphi** topluluğu için ❤️ ile yapıldı.

*[🇬🇧 English](README.md) · [Katkıda Bulunma](CONTRIBUTING.tr-TR.md) · [Davranış Kuralları](CODE_OF_CONDUCT.md) · [Güvenlik](SECURITY.tr-TR.md) · [Teşekkürler](ACKNOWLEDGMENTS.tr-TR.md)*

*Bu kit işinize yaradıysa, depoda bir ⭐ bırakın!*

</div>
