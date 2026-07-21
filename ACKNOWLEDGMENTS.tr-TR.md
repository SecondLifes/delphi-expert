# 🙏 Teşekkürler

**Delphi AI Spec-Kit**, aşağıdaki açık kaynak projelere, ticari araçlara
ve topluluklara dayanıyor. Bu sayfa onları açıkça takdir etmek için var —
sadece README'nin bir köşesinde tek bir linkle geçiştirmek yerine.

> Not: GitHub'ın `SECURITY.md` için yaptığı gibi ayrı bir "Acknowledgments"
> sekmesi yok — bu dosya görünürlüğünü README'nin rozet satırından ve
> indeksinden link verilerek kazanıyor.

## 📖 Açık Kaynak

| Proje | Burada ne için kullanıldı | Lisans |
|---|---|---|
| [Horse](https://github.com/HashLoad/horse) | REST API framework'ü — `.agents/rules/horse-patterns.md` ve `horse-framework` skill'i Controller/Service/Repository yapısını ve middleware zincirini öğretir | MIT |
| [DelphiMVCFramework (DMVC)](https://github.com/danieleteti/delphimvcframework) | Attribute tabanlı REST framework'ü — `.agents/rules/dmvc-patterns.md` ve `dmvc-framework` skill'i `[MVCPath]`, Active Record, JWT ve RQL konvansiyonlarını öğretir | Apache-2.0 |
| [Dext Framework](https://github.com/cesarliws/dext) | Delphi için ASP.NET tarzı Minimal API'ler, DI container ve `Dext.Entity` ORM — `.agents/rules/dext-patterns.md` ve `dext-framework` skill'i routing, DI ve Fluent Query API konvansiyonlarını öğretir | Apache-2.0 |
| [ACBr Project](https://github.com/frones/ACBr) (resmi [Projeto ACBr](https://projetoacbr.com.br)'ın topluluk aynası) | Brezilya ticari otomasyon bileşenleri (NFe, CTe, Boleto, TEF, mali yazıcılar) — `.agents/rules/acbr-patterns.md` `TACBrNFe`/`TACBrCTe` vb. etrafındaki service-wrapper izolasyonu ve yaşam döngüsü kurallarını öğretir | LGPL-2.1 |
| [DUnitX](https://github.com/VSoftTechnologies/DUnitX) | Unit test framework'ü — `.agents/rules/tdd-patterns.md` ve `tdd-dunitx`/`dunitx-testing` skill'leri `[TestFixture]`/`[Test]`, Red-Green-Refactor ve interface üzerinden Fake'leri öğretir | Apache-2.0 |
| [Firebird](https://www.firebirdsql.org/) | Gömülü/kurumsal SQL veritabanı — `.agents/rules/firebird-patterns.md` ve `firebird-database` skill'i FireDAC bağlantı ayarlarını, generator'ları, `RETURNING`'i ve transaction izolasyon seviyelerini öğretir | IDPL (Initial Developer's Public License, MPL tabanlı) |
| [PostgreSQL](https://www.postgresql.org/) | Modern SQL veritabanı — `.agents/rules/postgresql-patterns.md` ve `postgresql-database` skill'i UPSERT (`ON CONFLICT`), JSONB ve `IDENTITY` kolonlarını öğretir | PostgreSQL Lisansı (izin verici, MIT/BSD tarzı) |
| [MariaDB](https://mariadb.org/) | Açık kaynak MySQL-uyumlu veritabanı — `.agents/rules/mysql-patterns.md` ve `mysql-database` skill'i MySQL/MariaDB ailesi için `AUTO_INCREMENT`/`LAST_INSERT_ID()`, UPSERT ve native JSON'ı öğretir | GPL-2.0 |

## 💼 Ticari

| Ürün/Sağlayıcı | Burada ne için kullanıldı | Notlar |
|---|---|---|
| [TMS Aurelius](https://www.tmssoftware.com/site/aurelius.asp) (TMS Software) | Delphi için ORM — `aurelius-mapping` ve `aurelius-objects` skill'leri entity-mapping attribute'larını ve `TObjectManager` persistence/yaşam döngüsü kurallarını öğretir | Gerçek projeleri derlemek/çalıştırmak için ticari lisans gerekir; skill içeriği rehberliktir, kütüphanenin gömülü bir kopyası değildir |
| [TMS FlexCel](https://www.tmssoftware.com/site/flexcel.asp) (TMS Software) | Excel/PDF/HTML üretim kütüphanesi — `flexcel-vcl` skill'i Delphi için `TXlsFile` ve `TFlexCelReport` API'lerini öğretir | Ticari lisans gerekir; çalışma zamanında Office/Excel kurulumu gerekmez |
| [DevExpress VCL Subscription](https://www.devexpress.com/products/vcl/) | Gelişmiş Delphi UI bileşen paketi — `.agents/rules` framework tablosunda referans verir ve `devexpress-components` skill'i `TcxGrid`, `TdxLayoutControl` ve skin konvansiyonlarını öğretir | Ticari lisans gerekir |
| [IntraWeb](https://www.atozed.com/intraweb/) (Atozed Software) | Stateful VCL-for-the-web framework'ü — `.agents/rules/intraweb-patterns.md` ve `intraweb-framework` skill'i `UserSession` tabanlı durum izolasyonunu ve async-event desenlerini öğretir | Üretim kullanımı için ticari lisans gerekir |

## 📚 Referanslar ve İlham Kaynakları

Bu kitin kural ve konvansiyonlarını oluştururken danışılan stil
kılavuzları, resmi dokümantasyon ve önceki çalışmalar (bağımlılık değil —
sadece rehberlik kaynakları):

- [Embarcadero Delphi/Object Pascal dokümantasyonu](https://docwiki.embarcadero.com/RADStudio/en/Main_Page) — `delphi-conventions.md` ve `memory-exceptions.md`'deki isimlendirme ve bellek yönetimi kurallarının temelindeki dil ve RTL referansı
- Gang of Four *Design Patterns* — `design-patterns.md` ve `design-patterns` skill'inde ele alınan 23 kalıbın yapısı ve terminolojisi
- Martin Fowler'ın *Refactoring*'i — `refactoring.md`'de kullanılan code-smell kataloğu ve teknik isimleri (Extract Method, Extract Class, Guard Clauses, vb.)

## 👥 Proje Katkıda Bulunanları

Bu kitin kendisine (yukarıda teşekkür edilen upstream projelere değil,
bu kitin kurallarına/skill'lerine/konvansiyonlarına) katkıda bulunan
kişiler.

- baspinar99@gmail.com
- emr.pov@gmail.com
- re.baspinar@gmail.com

---

*Bu kit burada teşekkür edilmemiş bir şey kullanıyorsa lütfen bir issue
açın — eksiklikler kasıtlı değil, gözden kaçmadır.*
