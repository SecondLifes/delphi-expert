## MySQL Versions

| Version | Relevant News |
|--------|----------------------|
| **5.7** | Native JSON, Generated Columns, `sys` schema, Group Replication |
| **8.0** | Recursive CTEs, Window Functions, `DEFAULT (expr)`, Roles, `INVISIBLE` indexes, `NOWAIT`/`SKIP LOCKED` |
| **8.4 LTS** | LTS release, Firewall improvements, Plugin improvements |
| **9.0+** | Vector type, JavaScript stored programs (preview) |

### MariaDB

| Version | Relevant News |
|--------|----------------------|
| **10.2** | Recursive CTEs, Window Functions, `DEFAULT (expr)` |
| **10.3** | `INVISIBLE` columns, `INTERSECT`/`EXCEPT`, Sequences |
| **10.5** | `INET6` type, `JSON_TABLE`, S3 storage engine |
| **11.0+** | Release Calendar, UUID v7, `VECTOR` type |

> **Recommendation:** Use MySQL 8.0+ or ​​MariaDB 10.5+ for new projects.

## FireDAC connection with MySQL

### Minimum Configuration

```pascal
unit MeuApp.Infra.Database.MySQL.Connection;

interface

uses
  FireDAC.Comp.Client,
  FireDAC.Phys.MySQL,       //Driver MySQL
  FireDAC.Phys.MySQLDef,    //Defaults do MySQL
  FireDAC.Stan.Def,
  FireDAC.DApt;

type
  ///<summary>
  ///MySQL connection factory via FireDAC.
  ///</summary>
  TMySQLConnectionFactory = class
  public
    class function CreateConnection(
      const AServer: string;
      const ADatabase: string;
      const AUserName: string = 'root';
      const APassword: string = '';
      APort: Integer = 3306
    ): TFDConnection;
  end;

implementation

uses
  System.SysUtils;

class function TMySQLConnectionFactory.CreateConnection(
  const AServer, ADatabase, AUserName, APassword: string;
  APort: Integer): TFDConnection;
begin
  if ADatabase.Trim.IsEmpty then
    raise EArgumentException.Create('ADatabase não pode ser vazio');

  Result := TFDConnection.Create(nil);
  try
    Result.DriverName := 'MySQL';
    Result.Params.Values['Server'] := AServer;
    Result.Params.Values['Port'] := APort.ToString;
    Result.Params.Database := ADatabase;
    Result.Params.UserName := AUserName;
    Result.Params.Password := APassword;

    { Configurações recomendadas }
    Result.Params.Values['CharacterSet'] := 'utf8mb4';  //ALWAYS utf8mb4 (suporta emoji/4-byte)

    { Opções do driver FireDAC }
    Result.FormatOptions.StrsTrim2Len := True;
    Result.FetchOptions.Mode := fmAll;
    Result.ResourceOptions.AutoReconnect := True;
    Result.TxOptions.Isolation := xiReadCommitted;

    Result.Connected := True;
  except
    Result.Free;
    raise;
  end;
end;
```

### FDPhysMySQLDriverLink — Configure Client Library

```pascal
uses
  FireDAC.Phys.MySQLWrapper,
  FireDAC.Phys.MySQL;

var
  LDriverLink: TFDPhysMySQLDriverLink;
begin
  LDriverLink := TFDPhysMySQLDriverLink.Create(nil);
  try
    { Para MySQL 8.x: libmysql.dll }
    LDriverLink.VendorLib := 'C:\MySQL\lib\libmysql.dll';
    { Para MariaDB: libmariadb.dll }
    //LDriverLink.VendorLib := 'C:\MariaDB\lib\libmariadb.dll';
  finally
    { DriverLink vive por toda a aplicação — criar no DataModule }
  end;
end;
```

> **ATTENTION:** `utf8` in MySQL is only 3 bytes (does not support emoji 🎉). **always use `utf8mb4`** for full charset. MySQL's `utf8` is an alias for `utf8mb3`.

### Connection Pooling

```pascal
{ Via FDManager }
with FDManager.ConnectionDefs.AddConnectionDef do
begin
  Name := 'MySQL_Pool';
  DriverID := 'MySQL';
  Params.Values['Server'] := 'localhost';
  Params.Values['Port'] := '3306';
  Params.Values['Database'] := 'meubanco';
  Params.Values['User_Name'] := 'root';
  Params.Values['Password'] := 'senha';
  Params.Values['CharacterSet'] := 'utf8mb4';
  Params.Values['Pooled'] := 'True';
  Params.Values['POOL_MaximumItems'] := '50';
  Params.Values['POOL_CleanupTimeout'] := '30000';
end;
```

### SSL/TLS

```pascal
Result.Params.Values['SSL_ca'] := '/path/to/ca-cert.pem';
Result.Params.Values['SSL_cert'] := '/path/to/client-cert.pem';
Result.Params.Values['SSL_key'] := '/path/to/client-key.pem';
```

## Data Types — MySQL Mapping ↔ Delphi

| MySQL | Delphi (FireDAC) | Note |
|-------|------------------|------------|
| `INT` / `INTEGER` | `ftInteger` / `AsInteger` | 32-bit signed |
| `BIGINT` | `ftLargeint` / `AsLargeInt` | 64-bit |
| `SMALLINT` | `ftSmallint` / `AsSmallInt` | 16-bit |
| `TINYINT` | `ftSmallint` / `AsSmallInt` | 8-bit (`ftByte` does not exist) |
| `TINYINT(1)` | `ftBoolean` / `AsBoolean` | MySQL Convention for Boolean |
| `VARCHAR(N)` | `ftString` / `AsString` | Limited text |
| `TEXT` | `ftMemo` / `AsString` | Long text (up to 64KB) |
| `LONGTEXT` | `ftMemo` / `AsString` | Very long text (up to 4GB) |
| `DECIMAL(P,S)` | `ftBCD` / `AsCurrency` | Monetary values ​​|
| `DOUBLE` | `ftFloat` / `AsFloat` | Ponto flutuante |
| `FLOAT` | `ftSingle` / `AsSingle` | 32-bit float |
| `DATE` | `ftDate` / `AsDateTime` | Date only |
| `TIME` | `ftTime` / `AsDateTime` | Just in time |
| `DATETIME` | `ftDateTime` / `AsDateTime` | Date + Time (without timezone) |
| `TIMESTAMP` | `ftDateTime` / `AsDateTime` | Data + Hora (auto-update, UTC) |
| `BOOLEAN` / `BOOL` | `ftBoolean` / `AsBoolean` | Alias ​​for `TINYINT(1)` |
| `JSON` | `ftMemo` / `AsString` | Native JSON (MySQL 5.7+) |
| `BLOB` | `ftBlob` / `AsBytes` | Binary data |
| `LONGBLOB` | `ftBlob` / `AsBytes` | Large binary (up to 4GB) |
| `ENUM(...)` | `ftString` / `AsString` | Up to 65535 values ​​|
| `SET(...)` | `ftString` / `AsString` | Combination of values ​​|
| `CHAR(36)` | `ftString` / `AsString` | UUID as string |
