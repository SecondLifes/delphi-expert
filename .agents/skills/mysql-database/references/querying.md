## UPSERT — INSERT ... ON DUPLICATE KEY UPDATE

```sql
-- Inserir ou atualizar se a PK/UNIQUE já existir
INSERT INTO customers (cpf, name, email, status)
VALUES (:cpf, :name, :email, :status)
ON DUPLICATE KEY UPDATE
  name = VALUES(name),
  email = VALUES(email),
  status = VALUES(status);

-- MySQL 8.0.19+: Alias com AS
INSERT INTO customers (cpf, name, email, status)
VALUES (:cpf, :name, :email, :status) AS new_data
ON DUPLICATE KEY UPDATE
  name = new_data.name,
  email = new_data.email,
  status = new_data.status;
```

**In Delphi:**

```pascal
procedure TMySQLCustomerRepository.Upsert(ACustomer: TCustomer);
var
  LQuery: TFDQuery;
begin
  LQuery := TFDQuery.Create(nil);
  try
    LQuery.Connection := FConnection;
    LQuery.SQL.Text :=
      'INSERT INTO customers (cpf, name, email, status) ' +
      'VALUES (:cpf, :name, :email, :status) ' +
      'ON DUPLICATE KEY UPDATE ' +
      '  name = VALUES(name), email = VALUES(email), status = VALUES(status)';
    LQuery.ParamByName('cpf').AsString := ACustomer.Cpf;
    LQuery.ParamByName('name').AsString := ACustomer.Name;
    LQuery.ParamByName('email').AsString := ACustomer.Email;
    LQuery.ParamByName('status').AsSmallInt := Ord(ACustomer.Status);
    LQuery.ExecSQL;

    { Obter ID (seja insert ou update) }
    LQuery.SQL.Text := 'SELECT LAST_INSERT_ID() AS new_id';
    LQuery.Open;
    ACustomer.Id := LQuery.FieldByName('new_id').AsInteger;
  finally
    LQuery.Free;
  end;
end;
```

## Native JSON (MySQL 5.7+)

### Storage and Query

```sql
-- Tabela com coluna JSON
CREATE TABLE customer_settings (
  customer_id  INT NOT NULL REFERENCES customers(id),
  settings     JSON NOT NULL,
  PRIMARY KEY (customer_id)
);

-- Inserir JSON
INSERT INTO customer_settings (customer_id, settings)
VALUES (1, '{"theme": "dark", "language": "pt-BR", "notifications": true}');

-- Consultar campo específico (operador ->>)
SELECT JSON_UNQUOTE(JSON_EXTRACT(settings, '$.theme')) AS theme
FROM customer_settings WHERE customer_id = 1;

-- Sintaxe curta com ->>
SELECT settings->>'$.theme' AS theme FROM customer_settings WHERE customer_id = 1;

-- Filtrar por valor JSON
SELECT * FROM customer_settings
WHERE JSON_CONTAINS(settings, '"dark"', '$.theme');

-- Índice virtual para busca em JSON (Generated Column + Index)
ALTER TABLE customer_settings
  ADD COLUMN theme VARCHAR(50) GENERATED ALWAYS AS (settings->>'$.theme') VIRTUAL,
  ADD INDEX idx_theme (theme);
```

**In Delphi:**

```pascal
{ Inserir JSON }
LQuery.SQL.Text :=
  'INSERT INTO customer_settings (customer_id, settings) ' +
  'VALUES (:customer_id, :settings)';
LQuery.ParamByName('customer_id').AsInteger := ACustomerId;
LQuery.ParamByName('settings').AsString := AJsonString;
LQuery.ExecSQL;

{ Ler campo JSON }
LQuery.SQL.Text :=
  'SELECT settings->>''$.theme'' AS theme ' +
  'FROM customer_settings WHERE customer_id = :id';
LQuery.ParamByName('id').AsInteger := ACustomerId;
LQuery.Open;
LTheme := LQuery.FieldByName('theme').AsString;
```

## Full-Text Search (InnoDB)

```sql
-- Índice FULLTEXT (InnoDB, MyISAM)
ALTER TABLE products ADD FULLTEXT INDEX ft_product_search (name, description);

-- Busca Natural Language
SELECT *, MATCH(name, description) AGAINST('camisa azul' IN NATURAL LANGUAGE MODE) AS relevance
FROM products
WHERE MATCH(name, description) AGAINST('camisa azul' IN NATURAL LANGUAGE MODE)
ORDER BY relevance DESC;

-- Busca Boolean Mode (mais controle)
SELECT * FROM products
WHERE MATCH(name, description) AGAINST('+camisa +azul -infantil' IN BOOLEAN MODE);
```

## Stored Procedures and Functions

```sql
-- Procedure (equivale a Executable no Firebird)
DELIMITER //
CREATE PROCEDURE sp_deactivate_customer(IN p_id INT)
BEGIN
  UPDATE customers SET status = 1, updated_at = NOW() WHERE id = p_id;
  IF ROW_COUNT() = 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Customer not found';
  END IF;
END //
DELIMITER ;

-- Function escalar
DELIMITER //
CREATE FUNCTION fn_customer_full_name(p_id INT) RETURNS VARCHAR(200)
  READS SQL DATA
BEGIN
  DECLARE v_name VARCHAR(200);
  SELECT name INTO v_name FROM customers WHERE id = p_id;
  IF v_name IS NULL THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Customer not found';
  END IF;
  RETURN v_name;
END //
DELIMITER ;
```

**Call in Delphi:**

```pascal
{ Procedure }
LQuery.SQL.Text := 'CALL sp_deactivate_customer(:p_id)';
LQuery.ParamByName('p_id').AsInteger := ACustomerId;
LQuery.ExecSQL;

{ Function escalar }
LQuery.SQL.Text := 'SELECT fn_customer_full_name(:p_id) AS full_name';
LQuery.ParamByName('p_id').AsInteger := ACustomerId;
LQuery.Open;
LFullName := LQuery.FieldByName('full_name').AsString;
```

> **Note:** MySQL Procedures are called with `CALL`, Functions with `SELECT`. Procedures can return result sets via `SELECT` inside the body.

## ENUM and SET

```sql
-- ENUM: valor único de uma lista
CREATE TABLE orders (
  id        INT AUTO_INCREMENT PRIMARY KEY,
  status    ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled')
              NOT NULL DEFAULT 'pending',
  priority  ENUM('low', 'medium', 'high') NOT NULL DEFAULT 'medium'
);

-- SET: múltiplos valores de uma lista
CREATE TABLE products (
  id      INT AUTO_INCREMENT PRIMARY KEY,
  name    VARCHAR(100),
  tags    SET('new', 'sale', 'featured', 'limited') NOT NULL DEFAULT ''
);

-- Inserir SET
INSERT INTO products (name, tags) VALUES ('Camisa', 'new,featured');
```

**In Delphi (map to Pascal enum):**

```pascal
type
  TOrderStatus = (osPending, osProcessing, osShipped, osDelivered, osCancelled);

const
  ORDER_STATUS_NAMES: array[TOrderStatus] of string = (
    'pending', 'processing', 'shipped', 'delivered', 'cancelled'
  );

{ Ler do banco }
LOrder.Status := StringToOrderStatus(LQuery.FieldByName('status').AsString);

{ Gravar no banco }
LQuery.ParamByName('status').AsString := ORDER_STATUS_NAMES[AOrder.Status];
```

## CTEs and Window Functions (MySQL 8.0+)

```sql
-- CTE (MySQL 8.0+)
WITH active_customers AS (
  SELECT id, name, email FROM customers WHERE status = 0
)
SELECT ac.name, COUNT(o.id) AS total_orders
FROM active_customers ac
LEFT JOIN orders o ON o.customer_id = ac.id
GROUP BY ac.id, ac.name;

-- CTE Recursiva (hierarquia)
WITH RECURSIVE category_tree AS (
  SELECT id, name, parent_id, 0 AS level
  FROM categories WHERE parent_id IS NULL
  UNION ALL
  SELECT c.id, c.name, c.parent_id, ct.level + 1
  FROM categories c JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree ORDER BY level, name;

-- Window Functions (MySQL 8.0+)
SELECT name, total_spent,
  RANK() OVER (ORDER BY total_spent DESC) AS ranking,
  ROW_NUMBER() OVER (ORDER BY total_spent DESC) AS row_num
FROM (
  SELECT c.name, SUM(o.total_amount) AS total_spent
  FROM customers c JOIN orders o ON o.customer_id = c.id
  GROUP BY c.id, c.name
) ranked;
```

## UUID as Primary Key

```sql
-- Gerar UUID no MySQL
CREATE TABLE sessions (
  id        CHAR(36) NOT NULL DEFAULT (UUID()) PRIMARY KEY,
  user_id   INT NOT NULL,
  token     VARCHAR(255),
  INDEX idx_session_user (user_id)
) ENGINE=InnoDB;

-- MySQL 8.0+: UUID() como DEFAULT
-- MySQL < 8.0: gerar no Delphi e enviar como parâmetro
```

**In Delphi:**

```pascal
uses
  System.SysUtils;

{ Gerar UUID no Delphi para MySQL < 8.0 }
LQuery.ParamByName('id').AsString := TGUID.NewGuid.ToString;
```
