
## 2. Adatbázisok kezelése

|Cél|SQL parancs|
|---|---|
|Adatbázisok listázása|`SHOW DATABASES;`|
|Adatbázis létrehozása|`CREATE DATABASE gyakorlas;`|
|Adatbázis kiválasztása|`USE gyakorlas;`|
|Aktuális adatbázis megnézése|`SELECT DATABASE();`|
|Adatbázis törlése|`DROP DATABASE gyakorlas;`|

Példa:

```sql
CREATE DATABASE gyakorlas;
USE gyakorlas;
```

---

## 3. Táblák kezelése

|Cél|SQL parancs|
|---|---|
|Táblák listázása|`SHOW TABLES;`|
|Tábla szerkezetének megnézése|`DESCRIBE users;`|
|Alternatív szerkezetnézés|`SHOW COLUMNS FROM users;`|
|Tábla létrehozása|`CREATE TABLE ...`|
|Tábla törlése|`DROP TABLE users;`|
|Tábla kiürítése|`TRUNCATE TABLE users;`|
|Tábla átnevezése|`RENAME TABLE users TO customers;`|

Példa:

```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(150),
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 4. Gyakori adattípusok

|Típus|Mire való?|Példa|
|---|---|---|
|`INT`|egész szám|`age INT`|
|`BIGINT`|nagy egész szám|`views BIGINT`|
|`VARCHAR(n)`|rövid szöveg|`name VARCHAR(100)`|
|`TEXT`|hosszabb szöveg|`description TEXT`|
|`DATE`|dátum|`birth_date DATE`|
|`DATETIME`|dátum + idő|`created_at DATETIME`|
|`TIMESTAMP`|időbélyeg|`created_at TIMESTAMP`|
|`DECIMAL(10,2)`|pontos tizedes szám|`price DECIMAL(10,2)`|
|`BOOLEAN`|igaz/hamis|`is_active BOOLEAN`|

---

## 5. Sorok beszúrása — INSERT

Egy sor beszúrása:

```sql
INSERT INTO users (name, email, age)
VALUES ('Krisz', 'krisz@example.com', 29);
```

Több sor beszúrása:

```sql
INSERT INTO users (name, email, age)
VALUES
  ('Anna', 'anna@example.com', 24),
  ('Béla', 'bela@example.com', 31),
  ('Csilla', 'csilla@example.com', 27);
```

---

## 6. Lekérdezés — SELECT

Minden oszlop lekérése:

```sql
SELECT * FROM users;
```

Konkrét oszlopok lekérése:

```sql
SELECT name, email FROM users;
```

Alias használata:

```sql
SELECT name AS nev, email AS email_cim
FROM users;
```

---

## 7. Szűrés — WHERE

```sql
SELECT * FROM users
WHERE age > 25;
```

```sql
SELECT * FROM users
WHERE name = 'Krisz';
```

```sql
SELECT * FROM users
WHERE age >= 18 AND age <= 30;
```

```sql
SELECT * FROM users
WHERE age < 18 OR age > 65;
```

```sql
SELECT * FROM users
WHERE email IS NOT NULL;
```

---

## 8. Rendezés — ORDER BY

Növekvő sorrend:

```sql
SELECT * FROM users
ORDER BY age ASC;
```

Csökkenő sorrend:

```sql
SELECT * FROM users
ORDER BY age DESC;
```

Több mező szerint:

```sql
SELECT * FROM users
ORDER BY age DESC, name ASC;
```

---

## 9. Limitálás — LIMIT

Első 5 sor:

```sql
SELECT * FROM users
LIMIT 5;
```

Lapozás:

```sql
SELECT * FROM users
LIMIT 10 OFFSET 20;
```

Ez 10 sort ad vissza, de az első 20-at kihagyja.

---

## 10. Keresés szövegben — LIKE

Név, ami `K` betűvel kezdődik:

```sql
SELECT * FROM users
WHERE name LIKE 'K%';
```

Név, amiben van `ri`:

```sql
SELECT * FROM users
WHERE name LIKE '%ri%';
```

Név, ami `z` betűvel végződik:

```sql
SELECT * FROM users
WHERE name LIKE '%z';
```

---

## 11. IN, BETWEEN, NULL

Több lehetséges érték:

```sql
SELECT * FROM users
WHERE age IN (20, 25, 30);
```

Tartomány:

```sql
SELECT * FROM users
WHERE age BETWEEN 18 AND 30;
```

NULL érték keresése:

```sql
SELECT * FROM users
WHERE email IS NULL;
```

Nem NULL:

```sql
SELECT * FROM users
WHERE email IS NOT NULL;
```

---

## 12. Adatok módosítása — UPDATE

```sql
UPDATE users
SET age = 30
WHERE name = 'Krisz';
```

Több mező módosítása:

```sql
UPDATE users
SET name = 'Krisztián',
    age = 30
WHERE id = 1;
```

Vigyázat:

```sql
UPDATE users
SET age = 30;
```

Ez minden sorban átírja az életkort. Ez az SQL-es “hoppá bazdmeg” pillanat.

---

## 13. Adatok törlése — DELETE

Egy sor törlése:

```sql
DELETE FROM users
WHERE id = 1;
```

Minden sor törlése:

```sql
DELETE FROM users;
```

Gyors teljes ürítés:

```sql
TRUNCATE TABLE users;
```

---

## 14. Oszlop hozzáadása, módosítása, törlése — ALTER TABLE

Oszlop hozzáadása:

```sql
ALTER TABLE users
ADD COLUMN phone VARCHAR(30);
```

Oszlop módosítása:

```sql
ALTER TABLE users
MODIFY COLUMN phone VARCHAR(50);
```

Oszlop átnevezése:

```sql
ALTER TABLE users
RENAME COLUMN phone TO phone_number;
```

Oszlop törlése:

```sql
ALTER TABLE users
DROP COLUMN phone_number;
```

---

## 15. Primary key

A primary key egyedi azonosító.

```sql
CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  price DECIMAL(10,2)
);
```

---

## 16. Unique constraint

Egyedi érték kikényszerítése:

```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(150) UNIQUE
);
```

Utólag hozzáadva:

```sql
ALTER TABLE users
ADD CONSTRAINT unique_email UNIQUE (email);
```

---

## 17. Foreign key

Kapcsolat két tábla között.

```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE orders (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  total DECIMAL(10,2),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 18. JOIN-ok

### INNER JOIN

Csak azokat adja vissza, ahol mindkét táblában van egyezés.

```sql
SELECT users.name, orders.total
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

### LEFT JOIN

Minden usert visszaad, akkor is, ha nincs rendelése.

```sql
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

### RIGHT JOIN

Minden rendelést visszaad, akkor is, ha nincs hozzá user.

```sql
SELECT users.name, orders.total
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

---

## 19. Aggregálás

|Cél|SQL|
|---|---|
|Darabszám|`COUNT(*)`|
|Összeg|`SUM(total)`|
|Átlag|`AVG(total)`|
|Minimum|`MIN(total)`|
|Maximum|`MAX(total)`|

Példák:

```sql
SELECT COUNT(*) FROM users;
```

```sql
SELECT AVG(age) FROM users;
```

```sql
SELECT SUM(total) FROM orders;
```

---

## 20. GROUP BY

Rendelések száma userenként:

```sql
SELECT user_id, COUNT(*) AS order_count
FROM orders
GROUP BY user_id;
```

Összes költés userenként:

```sql
SELECT user_id, SUM(total) AS total_spent
FROM orders
GROUP BY user_id;
```

---

## 21. HAVING

A `WHERE` sorokra szűr, a `HAVING` csoportokra.

```sql
SELECT user_id, SUM(total) AS total_spent
FROM orders
GROUP BY user_id
HAVING total_spent > 10000;
```

---

## 22. Subquery

```sql
SELECT *
FROM users
WHERE id IN (
  SELECT user_id
  FROM orders
  WHERE total > 10000
);
```

---

## 23. CASE

Feltételes érték:

```sql
SELECT name, age,
  CASE
    WHEN age < 18 THEN 'kiskorú'
    WHEN age >= 18 AND age < 65 THEN 'felnőtt'
    ELSE 'idős'
  END AS korcsoport
FROM users;
```

---

## 24. Index

Index létrehozása:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Index törlése:

```sql
DROP INDEX idx_users_email ON users;
```

Az index gyorsítja a keresést, de lassíthatja az írást. Nem varázspor, hanem adatbázis-fegyver: jól használva hasznos, vakon szórva hülyeség.

---

## 25. View

View létrehozása:

```sql
CREATE VIEW user_orders AS
SELECT users.name, orders.total
FROM users
JOIN orders ON users.id = orders.user_id;
```

View használata:

```sql
SELECT * FROM user_orders;
```

View törlése:

```sql
DROP VIEW user_orders;
```

---

## 26. Tranzakciók

```sql
START TRANSACTION;

UPDATE users
SET age = 30
WHERE id = 1;

COMMIT;
```

Visszavonás:

```sql
START TRANSACTION;

DELETE FROM users
WHERE id = 1;

ROLLBACK;
```

---

## 27. DDL, DML, DQL, DCL, TCL

|Kategória|Jelentés|Példák|
|---|---|---|
|DDL|Data Definition Language|`CREATE`, `ALTER`, `DROP`, `TRUNCATE`|
|DML|Data Manipulation Language|`INSERT`, `UPDATE`, `DELETE`|
|DQL|Data Query Language|`SELECT`|
|DCL|Data Control Language|`GRANT`, `REVOKE`|
|TCL|Transaction Control Language|`COMMIT`, `ROLLBACK`|

---

## 28. Gyakorló mini adatbázis

```sql
CREATE DATABASE webshop;
USE webshop;

CREATE TABLE customers (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(150) UNIQUE
);

CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  price DECIMAL(10,2)
);

CREATE TABLE orders (
  id INT AUTO_INCREMENT PRIMARY KEY,
  customer_id INT,
  order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE order_items (
  id INT AUTO_INCREMENT PRIMARY KEY,
  order_id INT,
  product_id INT,
  quantity INT,
  FOREIGN KEY (order_id) REFERENCES orders(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

Tesztadatok:

```sql
INSERT INTO customers (name, email)
VALUES
  ('Krisz', 'krisz@example.com'),
  ('Anna', 'anna@example.com');

INSERT INTO products (name, price)
VALUES
  ('Laptop', 350000),
  ('Egér', 8000),
  ('Billentyűzet', 18000);

INSERT INTO orders (customer_id)
VALUES
  (1),
  (1),
  (2);

INSERT INTO order_items (order_id, product_id, quantity)
VALUES
  (1, 1, 1),
  (1, 2, 2),
  (2, 3, 1),
  (3, 2, 1);
```

Hasznos lekérdezés:

```sql
SELECT 
  customers.name,
  products.name AS product,
  order_items.quantity,
  products.price,
  order_items.quantity * products.price AS total
FROM order_items
JOIN orders ON order_items.order_id = orders.id
JOIN customers ON orders.customer_id = customers.id
JOIN products ON order_items.product_id = products.id;
```

---

## 29. Legfontosabb parancsok röviden

```sql
SHOW DATABASES;
CREATE DATABASE gyakorlas;
USE gyakorlas;
SHOW TABLES;
DESCRIBE users;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(150)
);

INSERT INTO users (name, email)
VALUES ('Krisz', 'krisz@example.com');

SELECT * FROM users;

UPDATE users
SET name = 'Krisztián'
WHERE id = 1;

DELETE FROM users
WHERE id = 1;

DROP TABLE users;
DROP DATABASE gyakorlas;
```

---

## 30. Fontos gondolkodási szabály

SQL-ben mindig kérdezd meg magadtól:

1. Melyik táblából kérek adatot?
    
2. Milyen oszlopok kellenek?
    
3. Kell-e szűrés?
    
4. Kell-e rendezés?
    
5. Kell-e több tábla összekapcsolása?
    
6. Kell-e csoportosítás?
    
7. Kell-e limit?
    

Alap lekérdezési sorrend fejben:

```sql
SELECT oszlopok
FROM tabla
JOIN masik_tabla ON kapcsolat
WHERE feltetel
GROUP BY csoport
HAVING csoport_feltetel
ORDER BY rendezes
LIMIT darab;
```