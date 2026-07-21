# src/

Default working/output root for this project. Unless you name a different
location, units, services, and repository implementations you ask for get
written here — inside the `Domain/Application/Infrastructure/Presentation`
layering described in `AGENTS.md`'s "Layer Structure (Architecture)"
section:

```
src/
├── Domain/         ← Entities, Value Objects, Repository Interfaces
├── Application/    ← Services, Use Cases, DTOs
├── Infrastructure/ ← FireDAC Repositories, external APIs
└── Presentation/   ← VCL/FMX Forms, ViewModels
tests/
└── Unit/           ← DUnitX projects with isolated Fakes
```

Not `examples/` (curated reference units, hand-maintained) and not the
project root. See `AGENTS.md`'s "Working Directory" section for the full
rule.
