## Transactions and Isolation Levels

### Isolation Levels in PostgreSQL

| Level | FireDAC | Usage |
|-------|---------|-----|
| **Read Committed** | `xiReadCommitted` | ✅ Standard — see committed data |
| **Repeatable Read** | `xiRepeatableRead` | Reports — snapshot at start of transaction |
| **Serializable** | `xiSerializable` | Maximum consistency (may result in serialization failure) |

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

### SAVEPOINT (Partial Transaction)

```pascal
{ PostgreSQL suporta SAVEPOINT para rollback parcial }
FConnection.StartTransaction;
try
  FCustomerRepo.Insert(LCustomer);

  FConnection.ExecSQL('SAVEPOINT before_order');
  try
    FOrderRepo.Insert(LOrder);
  except
    FConnection.ExecSQL('ROLLBACK TO SAVEPOINT before_order');
    { Customer foi salvo, order não }
  end;

  FConnection.Commit;
except
  FConnection.Rollback;
  raise;
end;
```

## LISTEN / NOTIFY (Bank Events)

```sql
-- No PostgreSQL: notificações assíncronas
-- Em uma trigger:
CREATE OR REPLACE FUNCTION notify_new_order() RETURNS TRIGGER AS $$
BEGIN
  PERFORM pg_notify('new_order', json_build_object('id', NEW.id, 'customer_id', NEW.customer_id)::TEXT);
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_order_notify AFTER INSERT ON orders
  FOR EACH ROW EXECUTE FUNCTION notify_new_order();
```

```pascal
{ No Delphi: escutar eventos via FireDAC Event Alerter }
uses
  FireDAC.Phys.PG;

var
  LAlerter: TFDEventAlerter;
begin
  LAlerter := TFDEventAlerter.Create(nil);
  try
    LAlerter.Connection := FConnection;
    LAlerter.DriverName := 'PG';
    LAlerter.Names.Text := 'new_order';
    LAlerter.Options.Timeout := 0;
    LAlerter.OnAlert := HandleNewOrderEvent;
    LAlerter.Active := True;
  finally
    { Manter vivo enquanto a aplicação rodar }
  end;
end;

procedure TMyService.HandleNewOrderEvent(ASender: TFDCustomEventAlerter;
  const AEventName: string; const AArgument: Variant);
begin
  { AArgument contém o payload JSON enviado pelo pg_notify }
  if AEventName = 'new_order' then
    ProcessNewOrderNotification(VarToStr(AArgument));
end;
```

## PostgreSQL Error Handling

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
        raise EConflictException.Create('Registro bloqueado por outra transação');
      ekServerGone:
        raise EConnectionLostException.Create('Conexão com PostgreSQL perdida');
    else
      raise;
    end;
  end;
end;

{ Verificar código de erro PostgreSQL específico }
except
  on E: EFDDBEngineException do
  begin
    { Códigos SQLSTATE do PostgreSQL: }
    { 23505 = unique_violation }
    { 23503 = foreign_key_violation }
    { 23502 = not_null_violation }
    { 40001 = serialization_failure }
    { 40P01 = deadlock_detected }
    if E.Errors[0].ErrorCode = 23505 then
      raise EDuplicateException.Create('Valor duplicado')
    else
      raise;
  end;
end;
```

## Useful Extensions

```sql
-- pgcrypto: criptografia
CREATE EXTENSION IF NOT EXISTS pgcrypto;
SELECT crypt('senha123', gen_salt('bf'));  -- bcrypt hash

-- pg_trgm: busca por similaridade (LIKE otimizado)
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE INDEX idx_customer_name_trgm ON customers USING GIN (name gin_trgm_ops);
SELECT * FROM customers WHERE name % 'Joao';  -- busca fuzzy

-- uuid-ossp: geração de UUID (PG < 13, se gen_random_uuid não disponível)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

## PostgreSQL Anti-Patterns to Avoid

```pascal
//❌ Concatenar SQL — SQL Injection
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = ''' + AName + '''';

//✅ Parameterized parameters
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = :name';
LQuery.ParamByName('name').AsString := AName;

//❌ ExecSQL with RETURNING — loses the result
LQuery.SQL.Text := 'INSERT INTO ... RETURNING id';
LQuery.ExecSQL;

//✅ Open com RETURNING
LQuery.SQL.Text := 'INSERT INTO ... RETURNING id';
LQuery.Open;

//❌ SELECT * on large tables
LQuery.SQL.Text := 'SELECT * FROM orders';

//✅ Select only necessary columns + LIMIT
LQuery.SQL.Text := 'SELECT id, customer_id, total FROM orders LIMIT :limit';

//❌ N+1 queries (loop with query inside)
for I := 0 to LCustomers.Count - 1 do
begin
  LQuery.SQL.Text := 'SELECT COUNT(*) FROM orders WHERE customer_id = :id';
  // ...
end;

//✅ JOIN ou subquery
LQuery.SQL.Text :=
  'SELECT c.*, (SELECT COUNT(*) FROM orders o WHERE o.customer_id = c.id) AS order_count ' +
  'FROM customers c';

//❌ Use SERIAL when IDENTITY is better
CREATE TABLE t (id SERIAL PRIMARY KEY);

//✅ Usar IDENTITY (SQL Standard)
CREATE TABLE t (id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY);

//❌ Ignore indexes on filtered columns
SELECT * FROM orders WHERE customer_id = 1;  -- sem índice = full scan

//✅ Create indexes for columns used in WHERE, JOIN, ORDER BY
CREATE INDEX idx_order_customer ON orders (customer_id);
```

## Key Differences: PostgreSQL vs Firebird

| Feature | PostgreSQL | Firebird |
|---------|-----------|----------|
| Auto-increment | `SERIAL`, `IDENTITY` | Generator + Trigger BI |
| UPSERT | `ON CONFLICT` | Non-native (partial MERGE FB5) |
| JSON | `JSONB` (indexable) | Non-native |
| Full-Text Search | `tsvector` native | Non-native |
| Arrays | `TEXT[]`, `INT[]` native | Non-native |
| ENUM | `CREATE TYPE ... AS ENUM` | Domain + CHECK |
| Embedded | No (server required) | Yes (fbclient.dll) |
| Schemas | Yes (schema separation) | No (single namespace) |
| IF EXISTS | `CREATE TABLE IF NOT EXISTS` | ❌ Does not exist (use `RDB$RELATIONS`) |
| Partitioning | Native (PG 10+) | Non-native |
| Procedures | `CREATE PROCEDURE` (PG 11+) | `CREATE PROCEDURE` (with `SUSPEND`) |
| Window Functions | Extensive support | FB 3+ (basic support) |
| FireDAC Driver | `PG` | `FB` |
| Client Library | `libpq.dll` | `fbclient.dll` |

## PostgreSQL Checklist

- [ ] Driver `PG` configured?
- [ ] Connection string with `Server`, `Port`, `Database`, `UserName`, `Password`?
- [ ] `CharacterSet := 'UTF8'` defined?
- [ ] Parameterized queries (without string concatenation)?
- [ ] `RETURNING` with `Open` (not `ExecSQL`)?
- [ ] `IDENTITY` instead of `SERIAL` for new projects?
- [ ] Explicit transactions for compound operations?
- [ ] Errors handled via `EFDDBEngineException.Kind`?
- [ ] libpq.dll (32/64-bit) in PATH or configured in VendorLib?
- [ ] Indexes created for columns used in WHERE and JOIN?
- [ ] Foreign Keys with appropriate `ON DELETE`/`ON UPDATE`?
- [ ] JSONB for semi-structured data (instead of TEXT)?
- [ ] `information_schema` to check metadata (not `RDB$`)?
