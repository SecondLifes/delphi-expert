# 🙏 Acknowledgments

**Delphi AI Spec-Kit** stands on the shoulders of the open-source projects,
commercial tools, and communities below. This page exists to credit them
explicitly — not just link to them once in a README aside.

> Note: GitHub doesn't have a dedicated "Acknowledgments" tab the way it
> does for Security (via `SECURITY.md`) — this file earns its visibility
> by being linked from the README's badge row and index instead.

## 📖 Open Source

| Project | What it's used for here | License |
|---|---|---|
| [Horse](https://github.com/HashLoad/horse) | REST API framework — `.agents/rules/horse-patterns.md` and the `horse-framework` skill teach its Controller/Service/Repository structure and middleware chain | MIT |
| [DelphiMVCFramework (DMVC)](https://github.com/danieleteti/delphimvcframework) | Attribute-based REST framework — `.agents/rules/dmvc-patterns.md` and the `dmvc-framework` skill teach `[MVCPath]`, Active Record, JWT and RQL conventions | Apache-2.0 |
| [Dext Framework](https://github.com/cesarliws/dext) | ASP.NET-style Minimal APIs, DI container and `Dext.Entity` ORM for Delphi — `.agents/rules/dext-patterns.md` and the `dext-framework` skill teach its routing, DI and Fluent Query API conventions | Apache-2.0 |
| [ACBr Project](https://github.com/frones/ACBr) (community mirror of the official [Projeto ACBr](https://projetoacbr.com.br)) | Brazilian commercial-automation components (NFe, CTe, Boleto, TEF, fiscal printers) — `.agents/rules/acbr-patterns.md` and the `acbr-components` skill teach service-wrapper isolation and lifetime rules around `TACBrNFe`/`TACBrCTe`/etc. | LGPL-2.1 |
| [DUnitX](https://github.com/VSoftTechnologies/DUnitX) | Unit-testing framework — `.agents/rules/tdd-patterns.md` and the `tdd-dunitx`/`dunitx-testing` skills teach `[TestFixture]`/`[Test]`, Red-Green-Refactor and Fakes-via-interface | Apache-2.0 |
| [Firebird](https://www.firebirdsql.org/) | Embedded/corporate SQL database — `.agents/rules/firebird-patterns.md` and the `firebird-database` skill teach FireDAC connection settings, generators, `RETURNING`, and transaction isolation levels | IDPL (Initial Developer's Public License, MPL-based) |
| [PostgreSQL](https://www.postgresql.org/) | Modern SQL database — `.agents/rules/postgresql-patterns.md` and the `postgresql-database` skill teach UPSERT (`ON CONFLICT`), JSONB, and `IDENTITY` columns | PostgreSQL License (permissive, MIT/BSD-style) |
| [MariaDB](https://mariadb.org/) | Open-source MySQL-compatible database — `.agents/rules/mysql-patterns.md` and the `mysql-database` skill teach `AUTO_INCREMENT`/`LAST_INSERT_ID()`, UPSERT and native JSON for the MySQL/MariaDB family | GPL-2.0 |

## 💼 Commercial

| Product/Vendor | What it's used for here | Notes |
|---|---|---|
| [TMS Aurelius](https://www.tmssoftware.com/site/aurelius.asp) (TMS Software) | ORM for Delphi — the `aurelius-mapping` and `aurelius-objects` skills teach entity-mapping attributes and `TObjectManager` persistence/lifetime rules | Commercial license required to build/run real projects; skill content is guidance, not a bundled copy of the library |
| [TMS FlexCel](https://www.tmssoftware.com/site/flexcel.asp) (TMS Software) | Excel/PDF/HTML generation library — the `flexcel-vcl` and `flexcel-net` skills teach the `TXlsFile`/`XlsFile` and `TFlexCelReport`/`FlexCelReport` APIs for Delphi and .NET respectively | Commercial license required; no Office/Excel installation needed at runtime |
| [DevExpress VCL Subscription](https://www.devexpress.com/products/vcl/) | Advanced Delphi UI component suite — `.agents/rules` reference it in the frameworks table and the `devexpress-components` skill teaches `TcxGrid`, `TdxLayoutControl` and skin conventions | Commercial license required |
| [IntraWeb](https://www.atozed.com/intraweb/) (Atozed Software) | Stateful VCL-for-the-web framework — `.agents/rules/intraweb-patterns.md` and the `intraweb-framework` skill teach `UserSession`-based state isolation and async-event patterns | Commercial license required for production use |

## 📚 References & Inspiration

Style guides, official documentation, and prior art consulted while
building this kit's rules and conventions (not dependencies — just
sources of guidance):

- [Embarcadero Delphi/Object Pascal documentation](https://docwiki.embarcadero.com/RADStudio/en/Main_Page) — language and RTL reference underlying the naming conventions and memory-management rules in `delphi-conventions.md` and `memory-exceptions.md`
- Gang of Four *Design Patterns* — structure and terminology for the 23 patterns covered in `design-patterns.md` and the `design-patterns` skill
- Martin Fowler's *Refactoring* — code-smell catalogue and technique names used in `refactoring.md` (Extract Method, Extract Class, Guard Clauses, etc.)

---

*If this kit uses something not credited here, please open an issue —
omissions are oversights, not deliberate.*
