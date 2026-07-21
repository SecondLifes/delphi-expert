## Transactions and Isolation Levels

### Isolation Levels in MySQL

| Level | FireDAC | Usage |
|-------|---------|-----|
| **Read Uncommitted** | `xiDirtyRead` | Almost never — reads uncommitted data |
| **Read Committed** | `xiReadCommitted` | ✅ Recommended pattern |
| **Repeatable Read** | `xiRepeatableRead` | InnoDB default — snapshot at start of tx |
| **Serializable** | `xiSerializable` | Maximum consistency (implicit locks) |

> **Note:** InnoDB's default isolation is `REPEATABLE READ`, unlike Firebird/PostgreSQL which use `READ COMMITTED`.

### Explicit Transaction

```pascal
procedure ExecuteInTransaction(AConnection: TFDConnection; AProc: TProc);
begin
  AConnection.StartTransaction;
  try
    AProc;
    AConnection.Commit;
  except
    AConnection.Rollback;
    raise;
  end;
end;
```

### SAVEPOINT

```pascal
FConnection.StartTransaction;
try
  FCustomerRepo.Insert(LCustomer);

  FConnection.ExecSQL('SAVEPOINT before_order');
  try
    FOrderRepo.Insert(LOrder);
  except
    FConnection.ExecSQL('ROLLBACK TO SAVEPOINT before_order');
  end;

  FConnection.Commit;
except
  FConnection.Rollback;
  raise;
end;
```

## MySQL Error Handling

```pascal
except
  on E: EFDDBEngineException do
  begin
    case E.Kind of
      ekUKViolated:
        raise EDuplicateException.Create('Registro duplicado: ' + E.Message);
      ekFKViolated:
        raise EDependencyException.Create('Violação de FK: ' + E.Message);
      ekRecordLocked:
        raise EConflictException.Create('Registro bloqueado — deadlock');
      ekServerGone:
        raise EConnectionLostException.Create('Conexão com MySQL perdida');
    else
      raise;
    end;
  end;
end;

{ Verificar código de erro MySQL específico }
except
  on E: EFDDBEngineException do
  begin
    { Códigos de erro MySQL comuns: }
    { 1062 = ER_DUP_ENTRY (duplicate key) }
    { 1451 = ER_ROW_IS_REFERENCED_2 (FK restrict) }
    { 1452 = ER_NO_REFERENCED_ROW_2 (FK no parent) }
    { 1213 = ER_LOCK_DEADLOCK }
    { 1205 = ER_LOCK_WAIT_TIMEOUT }
    { 2006 = CR_SERVER_GONE_ERROR }
    { 2013 = CR_SERVER_LOST }
    if E.Errors[0].ErrorCode = 1062 then
      raise EDuplicateException.Create('Valor duplicado')
    else
      raise;
  end;
end;
```

## Key Differences: MySQL vs Firebird vs PostgreSQL

| Feature | MySQL | Firebird | PostgreSQL |
|---------|-------|----------|------------|
| Auto-increment | `AUTO_INCREMENT` | Generator + BI Trigger | `SERIAL` / `IDENTITY` |
| Get generated ID | `LAST_INSERT_ID()` | `RETURNING id` | `RETURNING id` |
| UPSERT | `ON DUPLICATE KEY UPDATE` | Non-native (partial FB5) | `ON CONFLICT` |
| JSON | `JSON` (5.7+) | Non-native | `JSONB` (indexable) |
| Full-Text Search | `FULLTEXT` index | Non-native | `tsvector` |
| ENUM | `ENUM(...)` native | Domain + CHECK | `CREATE TYPE` |
| Embedded | No | Yes (fbclient.dll) | No |
| Engine Choice | InnoDB, MyISAM, etc. | Single engine | Single engine |
| Recommended Charset | `utf8mb4` | `UTF8` | `UTF8` |
| FireDAC Driver | `MySQL` | `FB` | `PG` |
| Client Library | `libmysql.dll` | `fbclient.dll` | `libpq.dll` |
| Default Isolation | Repeatable Read | Read Committed | Read Committed |
| StoredProcs | `CALL sp()` | `EXECUTE PROCEDURE` | `CALL sp()` (PG 11+) |
| Windows Functions | 8.0+ | 3.0+ (basic) | Ample |
| CTEs | 8.0+ | 3.0+ | All versions |

## MySQL Anti-Patterns to Avoid

```pascal
//❌ Concatenar SQL
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = ''' + AName + '''';

//✅ Parameterized parameters
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = :name';
LQuery.ParamByName('name').AsString := AName;

//❌ Use utf8 (3 bytes, does not support emoji)
Result.Params.Values['CharacterSet'] := 'utf8';

//✅ Use utf8mb4 (4 bytes, full support)
Result.Params.Values['CharacterSet'] := 'utf8mb4';

//❌ Try using RETURNING (it doesn't exist in MySQL!)
LQuery.SQL.Text := 'INSERT INTO ... RETURNING id';

//✅ Usar LAST_INSERT_ID()
LQuery.ExecSQL;
LQuery.SQL.Text := 'SELECT LAST_INSERT_ID()';
LQuery.Open;

//❌ Use MyISAM for new tables
CREATE TABLE t (...) ENGINE=MyISAM;

//✅ Always use InnoDB
CREATE TABLE t (...) ENGINE=InnoDB;

//❌ SELECT * sem LIMIT
LQuery.SQL.Text := 'SELECT * FROM orders';

//✅ LIMIT for pagination
LQuery.SQL.Text := 'SELECT id, customer_id, total FROM orders LIMIT :limit OFFSET :offset';

//❌ Ignore indexes on WHERE/JOIN columns
//✅ Create indexes for columns used in filters
```

## MySQL Checklist

- [ ] Driver `MySQL` configured on FireDAC?
- [ ] `CharacterSet := 'utf8mb4'` (NOT `utf8`)?
- [ ] `libmysql.dll` (32/64-bit) in PATH or `VendorLib`?
- [ ] Tables created with `ENGINE=InnoDB`?
- [ ] `AUTO_INCREMENT` in PKs with `LAST_INSERT_ID()` in Delphi?
- [ ] Parameterized queries (without concatenation)?
- [ ] Explicit transactions for compound operations?
- [ ] Errors handled via `EFDDBEngineException.Kind`?
- [ ] Indexes created for columns in WHERE and JOIN?
- [ ] Foreign Keys with appropriate `ON DELETE`/`ON UPDATE`?
- [ ] `COLLATE utf8mb4_unicode_ci` in the tables for correct comparison?
- [ ] `information_schema` to check metadata?
