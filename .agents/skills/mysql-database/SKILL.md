---
name: "MySQL Database"
description: "Development patterns with MySQL/MariaDB via FireDAC — connection, stored procedures, AUTO_INCREMENT, JSON, triggers, replication, migrations"
---

# MySQL Database — Skill

Use this skill when working with MySQL or MariaDB databases in Delphi projects via FireDAC.

## When to Use

- When configuring FireDAC connection with MySQL or MariaDB
- When creating tables, stored procedures, functions, triggers and views
- When implementing Repositories with FireDAC + MySQL
- When working with native JSON (MySQL 5.7+), Full-Text Search, Partitioning
- When planning schema migrations (versioned scripts)
- When developing web applications with MySQL backend

## Quick Reference

Minimum connection:

```pascal
FConnection.DriverName := 'MySQL';
FConnection.Params.Values['Server'] := 'localhost';
FConnection.Params.Values['Port'] := '3306';
FConnection.Params.Database := 'meubanco';
FConnection.Params.UserName := 'root';
FConnection.Params.Password := 'senha';
FConnection.Params.Values['CharacterSet'] := 'utf8mb4';  //NEVER 'utf8' (only 3 bytes!)
FConnection.TxOptions.Isolation := xiReadCommitted;
```

Golden rules: `utf8mb4` always (never `utf8`), MySQL has **no** `RETURNING` — use `LAST_INSERT_ID()`, always `ENGINE=InnoDB` (never MyISAM in new tables), parameterized queries only. Full details and code for each are in the reference files below — read the one that matches the task at hand rather than guessing.

## references/

Read only the file(s) relevant to the current task — this skill file is intentionally short; the depth lives in these files.

- `connection-and-types.md` — supported MySQL/MariaDB versions, FireDAC connection setup (driver link, pooling, SSL/TLS), MySQL ↔ Delphi data type mapping.
- `schema-and-migrations.md` — AUTO_INCREMENT + `LAST_INSERT_ID()`, triggers, InnoDB vs MyISAM, partitioning, migration scripts, and runtime schema-existence checks from Delphi.
- `querying.md` — UPSERT (`ON DUPLICATE KEY UPDATE`), native JSON, Full-Text Search, stored procedures/functions, ENUM/SET, CTEs and Window Functions (8.0+), UUID primary keys.
- `transactions-and-ops.md` — transactions/isolation levels, SAVEPOINT, error handling (`EFDDBEngineException`/MySQL error codes), MySQL-vs-Firebird-vs-PostgreSQL differences, anti-patterns, and the pre-flight checklist.
