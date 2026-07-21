## Stored Procedures in Firebird

### Selectable (returns resultset — uses SUSPEND)

```sql
CREATE OR ALTER PROCEDURE SP_CUSTOMERS_BY_STATUS (
  P_STATUS SMALLINT
)
RETURNS (
  O_ID       INTEGER,
  O_NAME     VARCHAR(100),
  O_CPF      VARCHAR(14),
  O_STATUS   SMALLINT
)
AS
BEGIN
  FOR SELECT id, name, cpf, status
      FROM customers
      WHERE status = :P_STATUS
      INTO :O_ID, :O_NAME, :O_CPF, :O_STATUS
  DO
    SUSPEND;  /* Retorna cada linha (como um cursor) */
END
```

**Call in Delphi (treated as SELECT):**

```pascal
LQuery.SQL.Text := 'SELECT * FROM SP_CUSTOMERS_BY_STATUS(:P_STATUS)';
LQuery.ParamByName('P_STATUS').AsSmallInt := Ord(csActive);
LQuery.Open;
```

### Executable (performs action — does not use SUSPEND)

```sql
CREATE OR ALTER PROCEDURE SP_DEACTIVATE_CUSTOMER (
  P_CUSTOMER_ID INTEGER
)
AS
BEGIN
  UPDATE customers SET status = 1 WHERE id = :P_CUSTOMER_ID;
END
```

**Call in Delphi:**

```pascal
LQuery.SQL.Text := 'EXECUTE PROCEDURE SP_DEACTIVATE_CUSTOMER(:P_CUSTOMER_ID)';
LQuery.ParamByName('P_CUSTOMER_ID').AsInteger := ACustomerId;
LQuery.ExecSQL;
```

## Execute Block (Anonymous SQL with PSQL)

```sql
/* Útil para lotes e scripts sem criar procedure permanente */
EXECUTE BLOCK (P_LIMIT INTEGER = :P_LIMIT)
RETURNS (O_NAME VARCHAR(100), O_TOTAL INTEGER)
AS
BEGIN
  FOR SELECT name, COUNT(*) FROM orders
      GROUP BY name
      HAVING COUNT(*) > :P_LIMIT
      INTO :O_NAME, :O_TOTAL
  DO
    SUSPEND;
END
```

## Packages (Firebird 3+)

```sql
/* Packages agrupam procedures e functions relacionadas */

/* Header (interface) */
CREATE OR ALTER PACKAGE PKG_CUSTOMER
AS
BEGIN
  PROCEDURE DEACTIVATE(P_ID INTEGER);
  FUNCTION GET_FULL_NAME(P_ID INTEGER) RETURNS VARCHAR(200);
END

/* Body (implementation) */
CREATE OR ALTER PACKAGE BODY PKG_CUSTOMER
AS
BEGIN
  PROCEDURE DEACTIVATE(P_ID INTEGER)
  AS
  BEGIN
    UPDATE customers SET status = 1 WHERE id = :P_ID;
  END

  FUNCTION GET_FULL_NAME(P_ID INTEGER) RETURNS VARCHAR(200)
  AS
    DECLARE VARIABLE V_NAME VARCHAR(200);
  BEGIN
    SELECT name FROM customers WHERE id = :P_ID INTO :V_NAME;
    RETURN V_NAME;
  END
END
```

**Call in Delphi:**

```pascal
LQuery.SQL.Text := 'EXECUTE PROCEDURE PKG_CUSTOMER.DEACTIVATE(:ID)';
LQuery.ParamByName('ID').AsInteger := ACustomerId;
LQuery.ExecSQL;

{ Function }
LQuery.SQL.Text := 'SELECT PKG_CUSTOMER.GET_FULL_NAME(:ID) FROM RDB$DATABASE';
LQuery.ParamByName('ID').AsInteger := ACustomerId;
LQuery.Open;
LFullName := LQuery.Fields[0].AsString;
```
