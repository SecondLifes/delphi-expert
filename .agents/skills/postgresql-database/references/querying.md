## UPSERT — INSERT ... ON CONFLICT

```sql
-- Inserir ou atualizar se já existir (pela constraint unique)
INSERT INTO customers (cpf, name, email, status)
VALUES (:cpf, :name, :email, :status)
ON CONFLICT (cpf) DO UPDATE SET
  name = EXCLUDED.name,
  email = EXCLUDED.email,
  status = EXCLUDED.status;

-- Ignorar se já existir (sem atualizar)
INSERT INTO customer_tags (customer_id, tag)
VALUES (:customer_id, :tag)
ON CONFLICT DO NOTHING;
```

**In Delphi:**

```pascal
procedure TPostgreSQLCustomerRepository.Upsert(ACustomer: TCustomer);
var
  LQuery: TFDQuery;
begin
  LQuery := TFDQuery.Create(nil);
  try
    LQuery.Connection := FConnection;
    LQuery.SQL.Text :=
      'INSERT INTO customers (cpf, name, email, status) ' +
      'VALUES (:cpf, :name, :email, :status) ' +
      'ON CONFLICT (cpf) DO UPDATE SET ' +
      '  name = EXCLUDED.name, ' +
      '  email = EXCLUDED.email, ' +
      '  status = EXCLUDED.status ' +
      'RETURNING id';
    LQuery.ParamByName('cpf').AsString := ACustomer.Cpf;
    LQuery.ParamByName('name').AsString := ACustomer.Name;
    LQuery.ParamByName('email').AsString := ACustomer.Email;
    LQuery.ParamByName('status').AsSmallInt := Ord(ACustomer.Status);
    LQuery.Open;
    ACustomer.Id := LQuery.FieldByName('id').AsInteger;
  finally
    LQuery.Free;
  end;
end;
```

## JSONB — Semi-Structured Data

### Storage and Query

```sql
-- Tabela com coluna JSONB
CREATE TABLE customer_settings (
  customer_id  INTEGER REFERENCES customers(id),
  settings     JSONB NOT NULL DEFAULT '{}',
  PRIMARY KEY (customer_id)
);

-- Inserir JSON
INSERT INTO customer_settings (customer_id, settings)
VALUES (1, '{"theme": "dark", "language": "pt-BR", "notifications": true}');

-- Consultar campo específico
SELECT settings->>'theme' AS theme FROM customer_settings WHERE customer_id = 1;

-- Filtrar por valor JSON
SELECT * FROM customer_settings WHERE settings @> '{"theme": "dark"}';

-- Índice GIN para busca rápida em JSONB
CREATE INDEX idx_settings_gin ON customer_settings USING GIN (settings);
```

**In Delphi:**

```pascal
{ Inserir JSONB }
LQuery.SQL.Text :=
  'INSERT INTO customer_settings (customer_id, settings) ' +
  'VALUES (:customer_id, :settings::jsonb)';
LQuery.ParamByName('customer_id').AsInteger := ACustomerId;
LQuery.ParamByName('settings').AsString := AJsonString;
LQuery.ExecSQL;

{ Ler campo JSONB }
LQuery.SQL.Text :=
  'SELECT settings->>''theme'' AS theme ' +
  'FROM customer_settings WHERE customer_id = :id';
LQuery.ParamByName('id').AsInteger := ACustomerId;
LQuery.Open;
LTheme := LQuery.FieldByName('theme').AsString;
```

## Full-Text Search (FTS)

```sql
-- Coluna tsvector para busca textual
ALTER TABLE products ADD COLUMN search_vector TSVECTOR;

-- Trigger para atualizar automaticamente
CREATE OR REPLACE FUNCTION update_search_vector() RETURNS TRIGGER AS $$
BEGIN
  NEW.search_vector :=
    setweight(to_tsvector('portuguese', COALESCE(NEW.name, '')), 'A') ||
    setweight(to_tsvector('portuguese', COALESCE(NEW.description, '')), 'B');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_product_search BEFORE INSERT OR UPDATE
  ON products FOR EACH ROW EXECUTE FUNCTION update_search_vector();

-- Índice GIN para FTS
CREATE INDEX idx_product_search ON products USING GIN (search_vector);

-- Buscar
SELECT * FROM products
WHERE search_vector @@ plainto_tsquery('portuguese', 'camisa azul')
ORDER BY ts_rank(search_vector, plainto_tsquery('portuguese', 'camisa azul')) DESC;
```

**In Delphi:**

```pascal
function TProductRepository.Search(const ASearchTerm: string): TObjectList<TProduct>;
var
  LQuery: TFDQuery;
begin
  Result := TObjectList<TProduct>.Create(True);
  LQuery := TFDQuery.Create(nil);
  try
    LQuery.Connection := FConnection;
    LQuery.SQL.Text :=
      'SELECT id, name, price, description ' +
      'FROM products ' +
      'WHERE search_vector @@ plainto_tsquery(''portuguese'', :term) ' +
      'ORDER BY ts_rank(search_vector, plainto_tsquery(''portuguese'', :term)) DESC ' +
      'LIMIT :limit';
    LQuery.ParamByName('term').AsString := ASearchTerm;
    LQuery.ParamByName('limit').AsInteger := 50;
    LQuery.Open;

    while not LQuery.Eof do
    begin
      Result.Add(MapToProduct(LQuery));
      LQuery.Next;
    end;
  finally
    LQuery.Free;
  end;
end;
```

## CTEs (Common Table Expressions)

```sql
-- CTE para queries complexas e legíveis
WITH active_customers AS (
  SELECT id, name, email
  FROM customers
  WHERE status = 0
),
customer_orders AS (
  SELECT customer_id, COUNT(*) AS total_orders, SUM(total_amount) AS total_spent
  FROM orders
  GROUP BY customer_id
)
SELECT ac.name, ac.email, co.total_orders, co.total_spent
FROM active_customers ac
LEFT JOIN customer_orders co ON co.customer_id = ac.id
ORDER BY co.total_spent DESC NULLS LAST;
```

### Recursive CTE

```sql
-- Hierarquia de categorias
WITH RECURSIVE category_tree AS (
  SELECT id, name, parent_id, 0 AS level
  FROM categories
  WHERE parent_id IS NULL

  UNION ALL

  SELECT c.id, c.name, c.parent_id, ct.level + 1
  FROM categories c
  JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree ORDER BY level, name;
```

## Window Functions

```sql
-- Ranking de clientes por valor gasto
SELECT
  c.name,
  SUM(o.total_amount) AS total_spent,
  RANK() OVER (ORDER BY SUM(o.total_amount) DESC) AS ranking,
  ROW_NUMBER() OVER (ORDER BY SUM(o.total_amount) DESC) AS row_num,
  SUM(o.total_amount) / SUM(SUM(o.total_amount)) OVER () * 100 AS percent_total
FROM customers c
JOIN orders o ON o.customer_id = c.id
GROUP BY c.id, c.name;

-- Média móvel de vendas por dia
SELECT
  order_date::DATE AS day,
  SUM(total_amount) AS daily_total,
  AVG(SUM(total_amount)) OVER (ORDER BY order_date::DATE ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS moving_avg_7d
FROM orders
GROUP BY order_date::DATE
ORDER BY day;
```

## Functions (PL/pgSQL)

```sql
-- Function que retorna valor (equivale a function no Delphi)
CREATE OR REPLACE FUNCTION fn_customer_full_name(p_id INTEGER)
RETURNS VARCHAR AS $$
DECLARE
  v_name VARCHAR;
BEGIN
  SELECT name INTO v_name FROM customers WHERE id = p_id;
  IF NOT FOUND THEN
    RAISE EXCEPTION 'Customer % not found', p_id;
  END IF;
  RETURN v_name;
END;
$$ LANGUAGE plpgsql;

-- Function que retorna tabela (equivale a Selectable Procedure no Firebird)
CREATE OR REPLACE FUNCTION fn_customers_by_status(p_status SMALLINT)
RETURNS TABLE (
  o_id     INTEGER,
  o_name   VARCHAR,
  o_email  VARCHAR,
  o_status SMALLINT
) AS $$
BEGIN
  RETURN QUERY
    SELECT id, name, email, status
    FROM customers
    WHERE status = p_status
    ORDER BY name;
END;
$$ LANGUAGE plpgsql;

-- Procedure (PG 11+ — sem retorno, apenas ação)
CREATE OR REPLACE PROCEDURE sp_deactivate_customer(p_id INTEGER)
LANGUAGE plpgsql AS $$
BEGIN
  UPDATE customers SET status = 1, updated_at = NOW() WHERE id = p_id;
  IF NOT FOUND THEN
    RAISE EXCEPTION 'Customer % not found', p_id;
  END IF;
END;
$$;
```

**Call in Delphi:**

```pascal
{ Function escalar }
LQuery.SQL.Text := 'SELECT fn_customer_full_name(:id)';
LQuery.ParamByName('id').AsInteger := ACustomerId;
LQuery.Open;
LFullName := LQuery.Fields[0].AsString;

{ Function que retorna table (como SELECT) }
LQuery.SQL.Text := 'SELECT * FROM fn_customers_by_status(:status)';
LQuery.ParamByName('status').AsSmallInt := Ord(csActive);
LQuery.Open;

{ Procedure (PG 11+) }
LQuery.SQL.Text := 'CALL sp_deactivate_customer(:id)';
LQuery.ParamByName('id').AsInteger := ACustomerId;
LQuery.ExecSQL;
```

## ENUM Types

```sql
-- Tipo enum nativo do PostgreSQL
CREATE TYPE order_status AS ENUM ('pending', 'processing', 'shipped', 'delivered', 'cancelled');

CREATE TABLE orders (
  id          SERIAL PRIMARY KEY,
  customer_id INTEGER NOT NULL REFERENCES customers(id),
  status      order_status NOT NULL DEFAULT 'pending',
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Inserir
INSERT INTO orders (customer_id, status) VALUES (1, 'processing');
```

**In Delphi (map to Pascal enum):**

```pascal
type
  TOrderStatus = (osPending, osProcessing, osShipped, osDelivered, osCancelled);

const
  ORDER_STATUS_NAMES: array[TOrderStatus] of string = (
    'pending', 'processing', 'shipped', 'delivered', 'cancelled'
  );

function StringToOrderStatus(const AValue: string): TOrderStatus;
var
  LStatus: TOrderStatus;
begin
  for LStatus := Low(TOrderStatus) to High(TOrderStatus) do
    if SameText(ORDER_STATUS_NAMES[LStatus], AValue) then
      Exit(LStatus);
  raise EArgumentException.CreateFmt('Status inválido: "%s"', [AValue]);
end;

{ Ler do banco }
LOrder.Status := StringToOrderStatus(LQuery.FieldByName('status').AsString);

{ Gravar no banco }
LQuery.ParamByName('status').AsString := ORDER_STATUS_NAMES[AOrder.Status];
```

## UUID as Primary Key

```sql
-- Usar gen_random_uuid() nativo (PG 13+)
CREATE TABLE sessions (
  id         UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id    INTEGER NOT NULL,
  token      VARCHAR(255),
  expires_at TIMESTAMPTZ
);

-- Para PG < 13: CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
-- Então: DEFAULT uuid_generate_v4()
```

**In Delphi:**

```pascal
LQuery.SQL.Text :=
  'INSERT INTO sessions (user_id, token, expires_at) ' +
  'VALUES (:user_id, :token, :expires_at) RETURNING id';
LQuery.ParamByName('user_id').AsInteger := AUserId;
LQuery.ParamByName('token').AsString := AToken;
LQuery.ParamByName('expires_at').AsDateTime := AExpiresAt;
LQuery.Open;
LSessionId := LQuery.FieldByName('id').AsString; { UUID como string }
```
