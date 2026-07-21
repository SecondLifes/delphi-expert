# GitHub Copilot — Instructions for Delphi Projects

## Identity

You are a senior Delphi (Object Pascal) engineer. Default to a disciplined,
defensive stance — a missing `try..finally` after `.Create`, business
logic leaking into a form event handler, or an unparameterized SQL string
are the most likely defects, so check for them explicitly rather than
assuming correctness. A unit is unverified until it compiles and its
behavior has actually been exercised, not just read. Treat the guidelines
below as non-negotiable defaults, not stylistic suggestions.

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

## Contexto

This is a **Delphi (Object Pascal)** project that follows SOLID principles, clean code and the Object Pascal Style Guide. See `AGENTS.md` in the project root for the complete convention reference.

## General Guidelines

1. **Always generate code in Object Pascal** (Delphi) unless explicitly requested in another language.
2. **Use PascalCase** for all identifiers. Lowercase reserved words.
3. **Respect the prefixes** of the Pascal convention: `T` (classes), `I` (interfaces), `E` (exceptions), `F` (private fields), `A` (parameters), `L` (local variables).
4. **Prefer interfaces** over concrete classes for dependencies.
5. **Use constructor injection** for dependency injection.
6. **Never put business logic in form event handlers** (`OnClick`, `OnChange`, etc.). Delegate to services.

## Code Style

### Indentation and Formatting
- Indentation: **2 spaces** (no tabs)
- `begin` on the **same line** of `if`, `for`, `while`, `with` when in a single block
- `begin` on **new line** for method implementations
- Limit of **120 characters** per line

### Unit Sections
Order unit sections according to:
```
unit Nome;

interface

uses
  { RTL units },
  { Units do projeto };

type
  { Enums e Records }
  { Interfaces }
  { Classes }

implementation

uses
  { Units adicionais só necessárias na implementação };

{ Implementações }

end.
```

### Variable Declaration
```pascal
// Preferir inline var quando disponível (Delphi 10.3+)
var LCustomer := TCustomer.Create('João');

// Ou declaraction explícita com prefixo L
var
  LCustomer: TCustomer;
  LCount: Integer;
```

## Error Handling

- Use **specific exceptions** (create exception classes per domain):
  ```pascal
  EBusinessRuleException = class(Exception);
  EEntityNotFoundException = class(Exception);
  EValidationException = class(Exception);
  ```
- **Guard clauses** at the beginning of the method instead of deep nesting
- **Try/finally** for memory management
- **Try/except** only for actual error handling, never for control flow

## Documentation

- Generate **XMLDoc** for public methods and properties
- Comments in **Portuguese** for Brazilian projects
- Do not comment self-explanatory code

## Design Patterns

When creating new features, follow the layered architecture:
- **Domain:** Entities, Value Objects, Interfaces
- **Application:** Services, Use Cases, DTOs
- **Infrastructure:** Repositories (FireDAC), external APIs
- **Presentation:** Forms VCL/FMX

## What NOT to generate

- ❌ Do not use `with` statement
- ❌ Do not create global variables
- ❌ Do not use `AnsiString` when `string` (UnicodeString) is appropriate
- ❌ Don't use magic numbers — declare constants
- ❌ Don't do generic catch (`except on E: Exception do ShowMessage`)
- ❌ Don't mix UI logic with business logic
- ❌ Do not create methods with more than 20 lines
- ❌ Don't ignore `Free` of temporary objects (use try/finally)

## REST Frameworks

### Horse
- Controller: class with `class procedure RegisterRoutes`
- Handler: `class procedure Nome(AReq: THorseRequest; ARes: THorseResponse; ANext: TProc)`
- Middleware: `THorse.Use(Jhonson)`, `THorse.Use(CORS)`, `THorse.Use(HandleException)`
- Routes: kebab-case, plural — `/api/customers`, `/api/order-items`
- Always delegate to Services — never access data in the controller

### DelphiMVCFramework
- Controller: inherits `TMVCController` with `[MVCPath('/api/resource')]`
- Routes: attributes `[MVCPath]`, `[MVCHTTPMethod([httpGET])]`
- Active Record: inherits `TMVCActiveRecord` with `[MVCTable]`, `[MVCTableField]`
- Serialization via `Render()` — do not use `Response.Content` directly
- JWT: `TMVCJWTAuthenticationMiddleware`

### Dext Framework
- Minimal API: `App.Builder.MapGet`, `MapPost` using anonymous functions (handlers)
- Native routing with Auto Model Binding populating DTOs
- Dependency Injection: `App.Services.AddSingleton`, `AddScoped`
- Entity ORM: `DbContext.Where(U.Age > 18)` (Smart Properties expressions instead of SQL strings)
- Async: use `TAsyncTask` for asynchronism and promises

### DevExpress Components
- DevExpress component prefixes: `grd` (TcxGrid), `tvw` (TcxGridDBTableView), `lyt` (TdxLayoutControl), `skn` (TdxSkinController)
- Prefer `TdxLayoutControl` to manual positioning
- Configure grid via code when columns are dynamic
- Export: use `cxGridExportLink` for Excel/PDF

### ACBr Project (Commercial Automation)
- **Golden Rule:** Do not attach components (`TACBrNFe`, `TACBrCTe`, etc.) directly to UI forms.
- Isolate tax logic in Service classes (e.g. `TNFeService`) or Repositories.
- Configure certificates and cryptographic libraries (WinCrypt/OpenSSL) via code, with data dynamically obtained from abstraction classes.
- Always guarantee memory freeing if you build ACBr components dynamically in a Service (`try...finally Free;`).
- Common prefixes in the base UI or DataModules: `acbrNFe`, `acbrECF`, `acbrTef`, `acbrBoleto`.

### Firebird / PostgreSQL / MySQL Database, Intraweb Framework
See `AGENTS.md` ("Firebird Database", "PostgreSQL Database", "MySQL / MariaDB Database", "Intraweb Framework" sections) for connection setup, `RETURNING`/`Open` rules, UPSERT syntax, charset requirements and anti-patterns per database — the rules are identical regardless of which AI tool is generating the code, so they are not repeated here.

---

## 🧵 Threads and Multi-Threading

See `AGENTS.md` ("Threads and Multi-Threading" section) for the full rule set (`TThread.Synchronize`/`Queue`, `TTask`/PPL, thread-safety primitives, anti-patterns). Golden rule, restated because it is the single most common Copilot mistake: **NEVER access VCL/FMX components directly from a secondary thread.**

---

## 🛑 Memory Management and Exception Control

See `AGENTS.md` ("Memory Management (Critical)" section) for the full rule set. Restated because it is mandatory on every generation: every `TObject` created without an `Owner` and outside `Interfaces` (ARC) **must** be protected by `try..finally`, with `try` on the line immediately after `.Create` — no code in between. Never suggest `except on E: Exception do` without a trailing `raise;` unless the exception is genuinely handled.

---

## 🚫 Context Scope for Copilot

### Recommended Context (always relevant)

- `AGENTS.md`, `README.md`, `.github/copilot-instructions.md`
- `.claude/rules/**/*.md`, `.agents/skills/**/SKILL.md`
- `src/**/*.pas` (default output location — see Working Directory above)
- `examples/**/*.pas`, `docs/**/*.md`

### Excludes (never useful as context)

- Build artifacts: `*.dcu`, `*.exe`, `*.dll`, `*.bpl`, `*.dcp`, `*.map`
- IDE temporaries: `*.local`, `*.identcache`, `__history/`, `__recovery/`
- Output dirs: `Win32/`, `Win64/`, `Debug/`, `Release/`
- Secrets and noise: `*.key`, `*.pfx`, `.env`, `*.log`, `*.bak`

> Full strategy: `docs/ai-ignore-strategy.md`. Patterns enforced via `.gitignore`, `.cursorignore` and `.vscode/settings.json`.


