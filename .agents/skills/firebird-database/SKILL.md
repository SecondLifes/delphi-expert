---
name: "Firebird Database"
description: "Development patterns with Firebird database via FireDAC — connection, PSQL, generators, transactions, migrations"
---

# Firebird Database — Skill

Use this skill when working with Firebird database in Delphi projects via FireDAC.

## When to Use

- When configuring FireDAC connection with Firebird
- When creating tables, generators, stored procedures, triggers, domains and views
- When implementing Repositories with FireDAC + Firebird
- When working with transactions, isolation levels and concurrency
- When planning schema migrations (versioned scripts)
- When optimizing queries and indexes for Firebird

## Quick Reference

Minimum connection:

```pascal
FConnection.DriverName := 'FB';
FConnection.Params.Values['CharacterSet'] := 'UTF8';    //ALWAYS UTF8
FConnection.Params.Values['SQLDialect'] := '3';          //NEVER Dialect 1
FConnection.Params.Values['Protocol'] := 'TCPIP';        //Or 'Local' for embedded
FConnection.Params.Values['PageSize'] := '16384';         // 16KB recomendado
FConnection.TxOptions.Isolation := xiReadCommitted;       //Standard isolation
```

Golden rules: Dialect 3 always (never 1), `RETURNING` always needs `Open` (never `ExecSQL`), CharacterSet UTF8 always, parameterized queries only, `CREATE TABLE IF NOT EXISTS` does not exist (check `RDB$RELATIONS` instead). Full details and code for each are in the reference files below — read the one that matches the task at hand rather than guessing.

## references/

Read only the file(s) relevant to the current task — this skill file is intentionally short; the depth lives in these files.

- `connection-and-types.md` — supported versions, FireDAC connection setup (driver link, pooling), dialect 1 vs 3, Firebird ↔ Delphi data type mapping.
- `schema-and-migrations.md` — generators/sequences, `IDENTITY` columns, `RETURNING` in Delphi, domains, triggers, migration scripts, runtime schema-existence checks, and `gbak` backup/restore.
- `querying.md` — selectable vs executable stored procedures, `EXECUTE BLOCK`, and Packages (Firebird 3+).
- `transactions-and-ops.md` — transactions/isolation levels, event alerter (`POST_EVENT`), anti-patterns, Firebird Embedded integration testing, and the pre-flight checklist.
