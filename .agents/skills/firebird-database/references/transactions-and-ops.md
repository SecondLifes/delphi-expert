## Transactions and Isolation Levels

### Isolation Levels in Firebird

| Level | FireDAC | Usage |
|-------|---------|-----|
| **Read Committed** | `xiReadCommitted` | ✅ Standard — reads committed data, without dirty reads |
| **Snapshot** (Concurrency) | `xiSnapshot` | Reports — consistent view of START momentum |
| **Snapshot Table Stability** | `xiSerializable` | Rare — exclusive lock on table |

### Manual Transaction Control

```pascal
///<summary>
///Performs operation within explicit transaction.
///</summary>
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

{ Uso }
ExecuteInTransaction(FConnection,
  procedure
  begin
    FCustomerRepo.Insert(LCustomer);
    FOrderRepo.Insert(LOrder);
    FStockRepo.DecreaseStock(LOrder.Items);
  end
);
```

### Transaction with Specific Isolation Level

```pascal
var
  LTransaction: TFDTransaction;
begin
  LTransaction := TFDTransaction.Create(nil);
  try
    LTransaction.Connection := FConnection;
    LTransaction.Options.Isolation := xiSnapshot; { Leitura consistente }
    LTransaction.Options.ReadOnly := True;
    LTransaction.StartTransaction;
    try
      { Queries de relatório aqui — snapshot imutável }
      LTransaction.Commit;
    except
      LTransaction.Rollback;
      raise;
    end;
  finally
    LTransaction.Free;
  end;
end;
```

## Event Alerter (Bank Events)

```sql
/* No Firebird: */
CREATE OR ALTER TRIGGER TRG_ORDER_NOTIFY FOR orders
  ACTIVE AFTER INSERT POSITION 0
AS
BEGIN
  POST_EVENT 'NEW_ORDER';
END
```

```pascal
{ No Delphi: escutar eventos do banco }
uses
  FireDAC.Phys.FB; //TFDPhysFBEventAlerts

var
  LAlerter: TFDEventAlerter;
begin
  LAlerter := TFDEventAlerter.Create(nil);
  try
    LAlerter.Connection := FConnection;
    LAlerter.Names.Text := 'NEW_ORDER';
    LAlerter.Options.Timeout := 0;  { Sem timeout — espera indefinidamente }
    LAlerter.OnAlert := HandleNewOrderEvent;
    LAlerter.Active := True;
  finally
    { Manter vivo enquanto a aplicação rodar — liberação no Destroy }
  end;
end;

procedure TMyService.HandleNewOrderEvent(ASender: TFDCustomEventAlerter;
  const AEventName: string; const AArgument: Variant);
begin
  if AEventName = 'NEW_ORDER' then
    RefreshOrderList;
end;
```

## Firebird Anti-Patterns to Avoid

```pascal
//❌ Dialect 1 — ambiguity with DATE
Result.Params.Values['SQLDialect'] := '1';

//✅ Dialect 3 — ALWAYS
Result.Params.Values['SQLDialect'] := '3';

//❌ Concatenar SQL — SQL Injection
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = ''' + AName + '''';

//✅ Parameterized parameters
LQuery.SQL.Text := 'SELECT * FROM customers WHERE name = :name';
LQuery.ParamByName('name').AsString := AName;

//❌ Ignore CharacterSet — problems with accents
Result.DriverName := 'FB';
Result.Params.Database := APath;
Result.Connected := True;  //Sem CharacterSet!

//✅ Always set CharacterSet
Result.Params.Values['CharacterSet'] := 'UTF8';

//❌ ExecSQL with RETURNING — returns nothing!
LQuery.SQL.Text := 'INSERT INTO ... RETURNING id';
LQuery.ExecSQL;  //go LOST!

//✅ Open with RETURNING — returns the resultset
LQuery.SQL.Text := 'INSERT INTO ... RETURNING id';
LQuery.Open;  //id available in Fields[0]
ACustomer.Id := LQuery.Fields[0].AsInteger;

//❌ Create database without appropriate Page Size
CREATE DATABASE '...' PAGE_SIZE 4096;  //Too small for large tables

//✅ Optimized Page Size
CREATE DATABASE '...' PAGE_SIZE 16384 DEFAULT CHARACTER SET UTF8;

//❌ Do not treat deadlocks
LQuery.ExecSQL;  //can cause deadlock in competition

//✅ Tratar deadlocks com retry
try
  LQuery.ExecSQL;
except
  on E: EFDDBEngineException do
  begin
    if E.Kind = ekRecordLocked then
      raise EConflictException.Create('Registro bloqueado por outra transação')
    else
      raise;
  end;
end;
```

## Integration Testing with Firebird Embedded

```pascal
[TestFixture]
TCustomerRepositoryFirebirdTest = class
private
  FConnection: TFDConnection;
  FDriverLink: TFDPhysFBDriverLink;
  FRepository: ICustomerRepository;
  FTestDbPath: string;
public
  [Setup]
  procedure Setup;

  [TearDown]
  procedure TearDown;

  [Test]
  procedure Insert_ValidCustomer_ShouldReturnGeneratedId;
end;

procedure TCustomerRepositoryFirebirdTest.Setup;
begin
  FTestDbPath := TPath.Combine(TPath.GetTempPath, 'test_' + TGUID.NewGuid.ToString + '.fdb');

  { Configurar driver embedded }
  FDriverLink := TFDPhysFBDriverLink.Create(nil);
  FDriverLink.VendorLib := 'fbclient.dll'; { Embedded no path da aplicação }

  { Criar banco de teste }
  FConnection := TFDConnection.Create(nil);
  FConnection.DriverName := 'FB';
  FConnection.Params.Database := FTestDbPath;
  FConnection.Params.UserName := 'SYSDBA';
  FConnection.Params.Password := 'masterkey';
  FConnection.Params.Values['Protocol'] := 'Local';
  FConnection.Params.Values['CharacterSet'] := 'UTF8';
  FConnection.Params.Values['CreateDatabase'] := 'Yes';  { Cria o .fdb automaticamente }
  FConnection.Connected := True;

  { Criar schema de teste }
  FConnection.ExecSQL('CREATE TABLE customers (id INTEGER PRIMARY KEY, name VARCHAR(100))');
  FConnection.ExecSQL('CREATE GENERATOR GEN_CUSTOMER_ID');

  FRepository := TFirebirdCustomerRepository.Create(FConnection);
end;

procedure TCustomerRepositoryFirebirdTest.TearDown;
begin
  FConnection.Connected := False;
  FConnection.Free;
  FDriverLink.Free;

  { Limpar banco temporário }
  if TFile.Exists(FTestDbPath) then
    TFile.Delete(FTestDbPath);
end;
```

## Firebird Checklist

- [ ] Dialect 3 configured (`SQLDialect := '3'`)?
- [ ] CharacterSet UTF8 defined?
- [ ] Page Size ≥ 8192 (ideal: 16384)?
- [ ] Parameterized queries (without string concatenation)?
- [ ] Generators/Sequences for auto-increment with BI trigger?
- [ ] `RETURNING` with `Open` (not `ExecSQL`)?
- [ ] Explicit transactions for compound operations?
- [ ] Deadlocks treated with `EFDDBEngineException.Kind = ekRecordLocked`?
- [ ] Correct FBClient.dll (32/64-bit) configured in VendorLib?
- [ ] Indexes created for columns used in WHERE and JOIN?
- [ ] Foreign Keys with appropriate `ON DELETE`/`ON UPDATE`?
- [ ] Regular backup via `gbak`?
- [ ] Versioned migration scripts?
