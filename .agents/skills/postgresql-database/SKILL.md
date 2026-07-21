---
name: "PostgreSQL Database"
description: "Development patterns with PostgreSQL via FireDAC — connection, PL/pgSQL, sequences, JSONB, UPSERT, full-text search, migrations"
---

# PostgreSQL Database — Skill

Use this skill when working with PostgreSQL database in Delphi projects via FireDAC.

## When to Use

- When configuring FireDAC connection with PostgreSQL
- When creating tables, sequences, functions, triggers and views
- When implementing Repositories with FireDAC + PostgreSQL
- When working with advanced types (JSONB, Arrays, UUID, ENUM)
- When implementing UPSERT, CTEs, Full-Text Search or Window Functions
- When planning schema migrations (versioned scripts)

## Quick Reference

Minimum connection:

```pascal
FConnection.DriverName := 'PG';
FConnection.Params.Values['Server'] := 'localhost';
FConnection.Params.Values['Port'] := '5432';
FConnection.Params.Database := 'meubanco';
FConnection.Params.UserName := 'postgres';
FConnection.Params.Password := 'senha';
FConnection.Params.Values['CharacterSet'] := 'UTF8';
FConnection.TxOptions.Isolation := xiReadCommitted;
```

Golden rules: `IDENTITY` over `SERIAL` for new projects, `RETURNING` always needs `Open` (never `ExecSQL`), parameterized queries only, `JSONB` (not `TEXT`) for semi-structured data. Full details and code for each are in the reference files below — read the one that matches the task at hand rather than guessing.

## references/

Read only the file(s) relevant to the current task — this skill file is intentionally short; the depth lives in these files.

- `connection-and-types.md` — supported versions, FireDAC connection setup (driver link, pooling, SSL/TLS), PostgreSQL ↔ Delphi data type mapping.
- `schema-and-migrations.md` — sequences/IDENTITY columns, `RETURNING` in Delphi, schemas, partitioning, migration scripts, and runtime schema-existence checks from Delphi.
- `querying.md` — UPSERT, JSONB, Full-Text Search, CTEs (incl. recursive), Window Functions, PL/pgSQL functions/procedures, ENUM types, UUID primary keys.
- `transactions-and-ops.md` — transactions/isolation levels, SAVEPOINT, LISTEN/NOTIFY, error handling (`EFDDBEngineException`/SQLSTATE), useful extensions, anti-patterns, PostgreSQL-vs-Firebird differences, and the pre-flight checklist.
