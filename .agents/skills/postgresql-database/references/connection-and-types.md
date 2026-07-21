## PostgreSQL Versions

| Version | Relevant News |
|--------|----------------------|
| **12** | Generated Columns, CTE inlining, Partitioning improvements |
| **13** | Incremental sorting, Parallel vacuum, Deduplication in B-tree |
| **14** | Multirange types, `SEARCH`/`CYCLE` in recursive CTEs |
| **15** | `MERGE` statement, JSON logging, `UNIQUE NULL NOT DISTINCT` |
| **16** | Logical replication from standby, `ANY_VALUE()`, ICU default collations |
| **17** | `RETURNING OLD/NEW` no `MERGE`, `JSON_TABLE`, Identity columns improvements |

> **Recommendation:** Use PostgreSQL 14+ for new projects. Enjoy `MERGE`, JSONB and partitioning.

## FireDAC Connection with PostgreSQL

### Minimum Configuration

```pascal
unit MeuApp.Infra.Database.PostgreSQL.Connection;

interface

uses
  FireDAC.Comp.Client,
  FireDAC.Phys.PG,         //Driver PostgreSQL
  FireDAC.Phys.PGDef,      //Defaults do PostgreSQL
  FireDAC.Stan.Def,
  FireDAC.Stan.Pool,
  FireDAC.DApt;

type
  ///<summary>
  ///PostgreSQL connection factory via FireDAC.
  ///</summary>
  TPostgreSQLConnectionFactory = class
  public
    ///<summary>
    ///Creates and configures a PostgreSQL connection.
    ///</summary>
    ///<param name="AServer">Server address</param>
    ///<param name="ADatabase">Database name</param>
    ///<param name="AUserName">User (default: postgres)</param>
    ///<param name="APassword">Bank password</param>
    ///<param name="APort">Port (default: 5432)</param>
    ///<returns>FireDAC connection configured and opened</returns>
    class function CreateConnection(
      const AServer: string;
      const ADatabase: string;
      const AUserName: string = 'postgres';
      const APassword: string = '';
      APort: Integer = 5432
    ): TFDConnection;

    ///<summary>
    ///Creates connection via full connection string.
    ///</summary>
    class function CreateFromConnectionString(
      const AConnectionString: string
    ): TFDConnection;
  end;

implementation

uses
  System.SysUtils;

class function TPostgreSQLConnectionFactory.CreateConnection(
  const AServer, ADatabase, AUserName, APassword: string;
  APort: Integer): TFDConnection;
begin
  if ADatabase.Trim.IsEmpty then
    raise EArgumentException.Create('ADatabase não pode ser vazio');

  Result := TFDConnection.Create(nil);
  try
    Result.DriverName := 'PG';
    Result.Params.Values['Server'] := AServer;
    Result.Params.Values['Port'] := APort.ToString;
    Result.Params.Database := ADatabase;
    Result.Params.UserName := AUserName;
    Result.Params.Password := APassword;

    { Configurações recomendadas }
    Result.Params.Values['CharacterSet'] := 'UTF8';

    { Opções do driver FireDAC }
    Result.FormatOptions.StrsTrim2Len := True;
    Result.FetchOptions.Mode := fmAll;
    Result.ResourceOptions.AutoReconnect := True;
    Result.TxOptions.Isolation := xiReadCommitted;

    { Schema padrão — 'public' por default, alterar se necessário }
    //Result.Params.Values['MetaDefSchema'] := 'public';

    Result.Connected := True;
  except
    Result.Free;
    raise;
  end;
end;

class function TPostgreSQLConnectionFactory.CreateFromConnectionString(
  const AConnectionString: string): TFDConnection;
begin
  Result := TFDConnection.Create(nil);
  try
    Result.ConnectionString := AConnectionString;
    Result.Connected := True;
  except
    Result.Free;
    raise;
  end;
end;
```

### FDPhysPGDriverLink — Configure Client Library

```pascal
uses
  FireDAC.Phys.PGWrapper,
  FireDAC.Phys.PG;

var
  LDriverLink: TFDPhysPGDriverLink;
begin
  LDriverLink := TFDPhysPGDriverLink.Create(nil);
  try
    { Apontar libpq.dll customizado (32/64-bit) }
    LDriverLink.VendorLib := 'C:\PostgreSQL\bin\libpq.dll';

    { Windows: precisa também libintl-9.dll, libeay32.dll, ssleay32.dll no PATH }
  finally
    { DriverLink vive por toda a aplicação — criar no DataModule }
  end;
end;
```

### Connection Pooling

```pascal
{ Via FDManager }
FDManager.ConnectionDefs.AddConnectionDef;
with FDManager.ConnectionDefs.ConnectionDefByName('PG_POOL') do
begin
  DriverID := 'PG';
  Server := 'localhost';
  Port := 5432;
  Database := 'meubanco';
  UserName := 'postgres';
  Password := 'senha';
  Params.Values['CharacterSet'] := 'UTF8';
  Params.Values['Pooled'] := 'True';
  Params.Values['POOL_MaximumItems'] := '50';
  Params.Values['POOL_CleanupTimeout'] := '30000';
  Params.Values['POOL_ExpireTimeout'] := '90000';
end;
```

### SSL/TLS

```pascal
{ Conexão segura com SSL }
Result.Params.Values['PGAdvanced'] := 'sslmode=require';
{ Para certificado de cliente: }
//Result.Params.Values['PGAdvanced'] :=
//'sslmode=verify-full;sslcert=client-cert.pem;sslkey=client-key.pem;sslrootcert=ca.pem';
```

## Data Types — PostgreSQL Mapping ↔ Delphi

| PostgreSQL | Delphi (FireDAC) | Note |
|------------|------------------|------------|
| `INTEGER` / `INT4` | `ftInteger` / `AsInteger` | 32-bit |
| `BIGINT` / `INT8` | `ftLargeint` / `AsLargeInt` | 64-bit |
| `SMALLINT` / `INT2` | `ftSmallint` / `AsSmallInt` | 16-bit |
| `SERIAL` | `ftAutoInc` / `AsInteger` | 32-bit auto-increment |
| `BIGSERIAL` | `ftAutoInc` / `AsLargeInt` | 64-bit auto-increment |
| `VARCHAR(N)` | `ftString` / `AsString` | Limited text |
| `TEXT` | `ftMemo` / `AsString` | Unlimited Text |
| `NUMERIC(P,S)` | `ftBCD` / `AsCurrency` | Monetary values ​​|
| `DOUBLE PRECISION` | `ftFloat` / `AsFloat` | Ponto flutuante |
| `REAL` / `FLOAT4` | `ftSingle` / `AsSingle` | 32-bit float |
| `DATE` | `ftDate` / `AsDateTime` | Date only |
| `TIME` | `ftTime` / `AsDateTime` | Just in time |
| `TIMESTAMP` | `ftDateTime` / `AsDateTime` | Date + Time (without timezone) |
| `TIMESTAMPTZ` | `ftDateTime` / `AsDateTime` | Date + Time (with timezone) |
| `BOOLEAN` | `ftBoolean` / `AsBoolean` | `TRUE`/`FALSE` native |
| `UUID` | `ftGuid` / `AsString` | Use `gen_random_uuid()` (PG 13+) |
| `JSON` | `ftMemo` / `AsString` | JSON text (validated) |
| `JSONB` | `ftMemo` / `AsString` | Binary JSON (indexable) |
| `BYTEA` | `ftBlob` / `AsBytes` | Binary data |
| `ARRAY` | `ftMemo` / `AsString` | PostgreSQL Array as Text |
| `INET` / `CIDR` | `ftString` / `AsString` | Network addresses |
