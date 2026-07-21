## Firebird Versions

| Version | Relevant News |
|--------|----------------------|
| **2.5** | Trace API, `LIST()` aggregate, Windows Trusted Auth |
| **3.0** | Native `BOOLEAN`, `IDENTITY` columns, Packages, UDR (replaces UDF), Window Functions (`OVER`), Encryption |
| **4.0** | `DECFLOAT`, `INT128`, `TIME/TIMESTAMP WITH TIME ZONE`, Replication, Batch API, `LATERAL` join |
| **5.0** | `WHEN NOT MATCHED BY SOURCE`, Parallel Backup, SQL Security hardening, Profiler |

> **Recommendation:** Use Firebird 3.0+ for new projects. Avoid deprecated features like UDFs.

## FireDAC connection with Firebird

### Minimum Configuration

```pascal
unit MeuApp.Infra.Database.Connection;

interface

uses
  FireDAC.Comp.Client,
  FireDAC.Phys.FB,        //Driver Firebird
  FireDAC.Phys.FBDef,     //Defaults do Firebird
  FireDAC.Stan.Def,
  FireDAC.Stan.Pool,
  FireDAC.DApt;

type
  ///<summary>
  ///Firebird connection factory via FireDAC.
  ///</summary>
  TFirebirdConnectionFactory = class
  public
    ///<summary>
    ///Creates and configures a Firebird connection.
    ///</summary>
    ///<param name="ADatabasePath">Full path of the .fdb file</param>
    ///<param name="AUserName">User (default: SYSDBA)</param>
    ///<param name="APassword">Bank password</param>
    ///<returns>FireDAC connection configured and opened</returns>
    class function CreateConnection(
      const ADatabasePath: string;
      const AUserName: string = 'SYSDBA';
      const APassword: string = 'masterkey'
    ): TFDConnection;

    ///<summary>
    ///Creates a connection via Embedded Server (without fbserver).
    ///</summary>
    class function CreateEmbeddedConnection(
      const ADatabasePath: string
    ): TFDConnection;
  end;

implementation

uses
  System.SysUtils;

class function TFirebirdConnectionFactory.CreateConnection(
  const ADatabasePath: string;
  const AUserName: string;
  const APassword: string): TFDConnection;
begin
  if ADatabasePath.Trim.IsEmpty then
    raise EArgumentException.Create('ADatabasePath não pode ser vazio');

  Result := TFDConnection.Create(nil);
  try
    Result.DriverName := 'FB';
    Result.Params.Database := ADatabasePath;
    Result.Params.UserName := AUserName;
    Result.Params.Password := APassword;

    { Configurações recomendadas }
    Result.Params.Values['CharacterSet'] := 'UTF8';
    Result.Params.Values['Protocol'] := 'TCPIP';     // Local: 'Local'
    Result.Params.Values['Server'] := 'localhost';
    Result.Params.Values['Port'] := '3050';
    Result.Params.Values['SQLDialect'] := '3';        //ALWAYS Dialect 3
    Result.Params.Values['PageSize'] := '16384';      // 16KB recomendado

    { Opções do driver FireDAC }
    Result.FormatOptions.StrsTrim2Len := True;         //Trim CHAR for VARCHAR
    Result.FetchOptions.Mode := fmAll;                 // Fetch completo
    Result.ResourceOptions.AutoReconnect := True;      //Automatic reconnection
    Result.TxOptions.Isolation := xiReadCommitted;     //Standard isolation

    Result.Connected := True;
  except
    Result.Free;
    raise;
  end;
end;

class function TFirebirdConnectionFactory.CreateEmbeddedConnection(
  const ADatabasePath: string): TFDConnection;
begin
  Result := TFDConnection.Create(nil);
  try
    Result.DriverName := 'FB';
    Result.Params.Database := ADatabasePath;

    { Embedded: sem servidor, sem user/password obrigatórios no FB3+ }
    Result.Params.Values['Protocol'] := 'Local';
    Result.Params.Values['CharacterSet'] := 'UTF8';
    Result.Params.Values['SQLDialect'] := '3';

    Result.Connected := True;
  except
    Result.Free;
    raise;
  end;
end;
```

### FDPhysFBDriverLink — Configure Client Library

```pascal
uses
  FireDAC.Phys.FBWrapper,
  FireDAC.Phys.FB;

var
  LDriverLink: TFDPhysFBDriverLink;
begin
  LDriverLink := TFDPhysFBDriverLink.Create(nil);
  try
    { Apontar fbclient.dll customizado (32/64-bit) }
    LDriverLink.VendorLib := 'C:\Firebird\fbclient.dll';

    { Embedded: usar fbclient.dll local ao .exe }
    //LDriverLink.VendorLib := ExtractFilePath(ParamStr(0)) + 'fbclient.dll';
  finally
    { DriverLink geralmente vive por toda a aplicação — criar no DataModule }
  end;
end;
```

### Connection Pooling

```pascal
{ No FDManager ou no Connection Definition }
FDManager.ConnectionDefs.AddConnectionDef;
with FDManager.ConnectionDefs.ConnectionDefByName('FB_POOL') do
begin
  DriverID := 'FB';
  Database := 'C:\Data\MeuBanco.fdb';
  UserName := 'SYSDBA';
  Password := 'masterkey';
  Params.Values['CharacterSet'] := 'UTF8';
  Params.Values['Pooled'] := 'True';
  Params.Values['POOL_MaximumItems'] := '50';
  Params.Values['POOL_CleanupTimeout'] := '30000';
  Params.Values['POOL_ExpireTimeout'] := '90000';
end;
```

## Dialects — ALWAYS Dialect 3

| Feature | Dialect 1 | Dialect 3 |
|---------|-----------|-----------|
| `DATE` | Includes time | Date only (use `TIMESTAMP` for date+time) |
| `"Identificadores"` | Syntax error | Allows case-sensitive names with double quotes |
| Numerical precision | `DOUBLE PRECISION` | `NUMERIC(18, x)` up to 18 digits |
| Recommendation | ❌ Legacy | ✅ **Mandatory for new projects** |

> ⚠️ **Rule:** Always `SQLDialect := 3`. Dialect 1 is legacy from InterBase and causes ambiguities with `DATE`.

## Data Types — Firebird Mapping ↔ Delphi

| Firebird | Delphi (FireDAC) | Note |
|----------|------------------|------------|
| `INTEGER` | `ftInteger` / `AsInteger` | 32-bit |
| `BIGINT` | `ftLargeint` / `AsLargeInt` | 64-bit |
| `SMALLINT` | `ftSmallint` / `AsSmallInt` | 16-bit |
| `VARCHAR(N)` | `ftString` / `AsString` | Use with `CHARACTER SET UTF8` |
| `CHAR(N)` | `ftFixedChar` | Fill with spaces — prefer `VARCHAR` |
| `NUMERIC(P,S)` | `ftBCD` / `AsCurrency` | Monetary values ​​|
| `DOUBLE PRECISION`| `ftFloat` / `AsFloat` | Ponto flutuante |
| `DATE` | `ftDate` / `AsDateTime` | Date only (Dialect 3) |
| `TIME` | `ftTime` / `AsDateTime` | Just in time |
| `TIMESTAMP` | `ftDateTime` / `AsDateTime` | Data + Hora |
| `BOOLEAN` (FB3+) | `ftBoolean` / `AsBoolean` | `TRUE`/`FALSE` native |
| `BLOB SUB_TYPE TEXT` | `ftMemo` / `AsString` | Texto grande (CLOB) |
| `BLOB SUB_TYPE 0` | `ftBlob` / `AsBytes` | Binary data |
