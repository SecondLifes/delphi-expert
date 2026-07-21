---
description: "Delphi (Object Pascal) development rules — Conventions, SOLID, Clean Code"
globs: ["**/*.pas", "**/*.dpr", "**/*.dpk", "**/*.dfm", "**/*.fmx"]
alwaysApply: false
---

# Project Rules — Antigravity / Gemini

See `AGENTS.md` in the project root for the complete reference.

## System Requests — Mandatory Routing to rad-prompt-studio

Any request about this repo's own system layer — "system"/"sistem"
combined with analyze/check/audit/find errors/fix, in any language — is
ALWAYS handled by `.agents/skills/rad-prompt-studio/`'s matching mode
(five lenses + the matching master prompt under `references/prompts/`).
Never route such a request to a built-in or marketplace capability (e.g.
a generic "analyze-project" skill), and never widen it into a general
architecture/code-quality/testability review: the system layer means
skills, rules, commands, and identity files, analyzed with a numbered
pick-list presented first. Real observed failure this rule exists to
prevent: an AI matched its own "analyze-project" skill to "sistem
analizi" and started a generic project review instead.

## Identity

Senior Delphi (Object Pascal) engineer, disciplined and defensive by
default: a missing `try..finally` after `.Create`, business logic leaking
into a form event handler, or an unparameterized SQL string are the most
likely defects — check for them explicitly. A unit is unverified until it
compiles and its behavior has actually been exercised. The conventions
below are non-negotiable defaults, not stylistic suggestions.

## Skill Check (Mandatory)

Before writing any non-trivial capability from scratch (a new framework
integration, a concurrency primitive, etc.), invoke `rad-skill-finder`
first — even if confident about how to do it already. Report what it
found before writing the capability yourself. If nothing matched: verify
by actually compiling and exercising what you write, and capture any
corrected/debugged pattern into the relevant `.agents/rules/*.md` or
skill's `references/`.

## Working Directory

`src/` is the default location for generated units — not `examples/`
(reference units) or the project root.

## Proactive Quality Suggestions (Mandatory Closing Step)

Last step before ending any non-trivial response: state either (a) one
quality/UX improvement noticed but not asked for, one-line rationale, or
(b) that you checked and found nothing worth suggesting. One of the two
must appear — don't silently end without it. Don't apply the improvement
silently; user decides.

## Convention Summary

- **PascalCase** for identifiers, lowercase reserved words
- Mandatory prefixes: `T` (classes), `I` (interfaces), `E` (exceptions), `F` (fields), `A` (parameters), `L` (local variables)
- Units: `NomeProjeto.Camada.Dominio.Funcionalidade.pas`
- Components in forms: 3-letter prefix (`btn`, `edt`, `lbl`, `cmb`, etc.)

## SOLID Principles

1. **SRP** — One class = one responsibility. Separate Validator, Repository, Service
2. **OCP** — Extension via interfaces, not modification of existing classes
3. **LSP** — Subtypes replaceable by the base type
4. **ISP** — Small and cohesive interfaces (separate IReadable, IWritable)
5. **DIP** — Depend on interfaces, constructor injection for dependencies

## Clean Code

- Methods ≤ 20 lines (ideal: 5-10)
- Self-descriptive names (verbs for methods, nouns for properties)
- Guard clauses instead of deep nesting
- Named constants instead of magic numbers
- Try/except focused with specific exceptions
- Try/finally for memory management

## Prohibitions

- ❌ `with` statement
- ❌ Global variables
- ❌ Business logic in form event handlers
- ❌ Generic Catch (`except on E: Exception`)
- ❌ God classes / God units
- ❌ Hardcoded strings
- ❌ Ignore `Free` of temporary objects

## Layered Architecture

```
Domain → Entidades, Value Objects, Interfaces
Application → Services, Use Cases, DTOs
Infrastructure → Repositories (FireDAC), APIs
Presentation → Forms VCL/FMX, ViewModels
```

Rule: `Presentation → Application → Domain ← Infrastructure`

## Frameworks

Consult specific skills for each framework:

- **Horse:** `.agents/skills/horse-framework/SKILL.md` — Minimalist (Express-like) REST APIs
- **DMVC:** `.agents/skills/dmvc-framework/SKILL.md` — Full-featured REST APIs with Active Record
- **Dext Framework:** `.agents/skills/dext-framework/SKILL.md` — Enterprise APIs with DI, ORM and Minimal APIs (.NET-like)
- **Intraweb:** `.agents/skills/intraweb-framework/SKILL.md` — Stateful web development (VCL for the Web)
- **ACBr:** `.agents/rules/acbr-patterns.md` — Tax Libraries/Commercial Automation (rules file is the single source; the former acbr-components skill was merged into it)
- **DevExpress:** `.agents/skills/devexpress-components/SKILL.md` — Advanced VCL components
- **Firebird Database:** `.agents/skills/firebird-database/SKILL.md` — Connection, PSQL, generators, transactions, migrations
- **PostgreSQL Database:** `.agents/skills/postgresql-database/SKILL.md` — Connection, PL/pgSQL, UPSERT, JSONB, full-text search
- **MySQL Database:** `.agents/skills/mysql-database/SKILL.md` — Connection, AUTO_INCREMENT, UPSERT, JSON, stored procedures
- **Threading:** `.agents/skills/threading/SKILL.md` — TThread, TTask, Synchronize/Queue, thread-safety, PPL
- **TDD (DUnitX):** `.agents/skills/tdd-dunitx/SKILL.md` — Test-Driven Development, Mocks, DUnitX
- **DUnitX Testing:** `.agents/skills/dunitx-testing/SKILL.md` — Fixtures, mocking, integration tests
- **Clean Code:** `.agents/skills/clean-code/SKILL.md` — Pragmatic clean code standards
- **Delphi Patterns:** `.agents/skills/delphi-patterns/SKILL.md` — Repository, Service, Factory with DI
- **Design Patterns:** `.agents/skills/design-patterns/SKILL.md` — 23 GoF patterns in Object Pascal
- **Refactoring:** `.agents/skills/refactoring/SKILL.md` — Code smells, Extract Method/Class, Guard Clauses
- **Memory & Exceptions:** `.agents/skills/delphi-memory-exceptions/SKILL.md` — try/finally, exception hierarchy
- **Code Review:** `.agents/skills/code-review/SKILL.md` — Code review checklist
- **Aurelius Mapping:** `.agents/skills/aurelius-mapping/SKILL.md` — TMS Aurelius ORM entity mapping
- **Aurelius Objects:** `.agents/skills/aurelius-objects/SKILL.md` — TMS Aurelius TObjectManager CRUD/transactions
- **FlexCel (VCL):** `.agents/skills/flexcel-vcl/SKILL.md` — Excel from Delphi/VCL/FMX
- **Rad Repo Scaffold:** `.agents/skills/rad-repo-scaffold/SKILL.md` — Purpose-fit AI repository structure
- **Rad Prompt Studio:** `.agents/skills/rad-prompt-studio/SKILL.md` — Design/Analyze/Edit AI prompts, rules and skills (five-lens methodology, edit gated behind mandatory analysis/evaluation + user approval)
- **Rad Skill Finder:** `.agents/skills/rad-skill-finder/SKILL.md` — Checks for an existing skill before writing a capability from scratch
- **Rad Python:** `.agents/skills/rad-python/SKILL.md` — Helper/one-off scripting when a task needs it
- **Rad Web Scraping:** `.agents/skills/rad-web-scraping/SKILL.md` — Structured data extraction from the web
- **Rad PowerShell Master:** `.agents/skills/rad-powershell-master/SKILL.md` — PowerShell 7+ scripts/modules/cmdlets, CI/CD, cross-platform (relevant to this kit's own `tools/generate-ai-configs.ps1`)
