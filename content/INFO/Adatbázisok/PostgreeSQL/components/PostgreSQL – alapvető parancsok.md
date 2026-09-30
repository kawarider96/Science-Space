---
tags:
  - PostgreSQL
  - SQL
  - adatbázis
---

# PostgreSQL – alapvető parancsok

## 1. Mi a PostgreSQL?

A **PostgreSQL** egy relációs adatbázis-kezelő rendszer. Az adatokat táblákban tárolja, a táblák között pedig kapcsolatokat hozhatunk létre.

A PostgreSQL kezelésére az **SQL** nyelvet használjuk.

> [!info] PostgreSQL vagy SQL?
> 
> - **SQL**: az adatbázisok kezelésére használt nyelv.
>     
> - **PostgreSQL**: egy konkrét adatbázis-kezelő rendszer, amely az SQL nyelvet használja.
>     
> 
> A PostgreSQL az SQL mellett több saját funkcióval is rendelkezik, például `ILIKE`, `RETURNING` vagy `JSONB`.

---

# 2. SQL-parancsok fő csoportjai

|Csoport|Jelentés|Példák|
|---|---|---|
|DDL|Adatbázis-szerkezet kezelése|`CREATE`, `ALTER`, `DROP`, `TRUNCATE`|
|DML|Adatok módosítása|`INSERT`, `UPDATE`, `DELETE`|
|DQL|Adatok lekérdezése|`SELECT`|
|TCL|Tranzakciók kezelése|`BEGIN`, `COMMIT`, `ROLLBACK`|

> [!tip]  
> Az SQL-parancsokat általában pontosvesszővel zárjuk:
> 
> ```sql
> SELECT * FROM users;
> ```

Az SQL kulcsszavai nem érzékenyek a kis- és nagybetűkre, de az átláthatóság érdekében ajánlott őket nagybetűvel írni:

```sql
SELECT name FROM users;
```

---

# 3. Adatbázis létrehozása és törlése

## Adatbázis létrehozása

```sql
CREATE DATABASE project_manager;
```

## Adatbázis törlése

```sql
DROP DATABASE project_manager;
```

> [!danger]  
> A `DROP DATABASE` az egész adatbázist törli, az összes táblával és adattal együtt.

## Csatlakozás adatbázishoz `psql` használatával

```text
\c project_manager
```

> [!note]  
> A `\c`, `\dt` és hasonló parancsok nem SQL-parancsok, hanem a PostgreSQL `psql` terminál saját parancsai. Ezek után nem kell pontosvessző.

---

# 4. Hasznos `psql` parancsok

|Parancs|Jelentés|
|---|---|
|`\l`|Adatbázisok listázása|
|`\c adatbazis`|Csatlakozás egy adatbázishoz|
|`\dt`|Táblák listázása|
|`\d users`|Egy tábla szerkezetének megjelenítése|
|`\dn`|Sémák listázása|
|`\du`|Felhasználók és szerepkörök listázása|
|`\q`|Kilépés a `psql` programból|

Példa:

```text
\c project_manager
\dt
\d users
```

---

# 5. Tábla létrehozása – `CREATE TABLE`

## Egyszerű tábla

```sql
CREATE TABLE users (
    id INTEGER,
    name VARCHAR(100),
    email VARCHAR(255),
    age INTEGER
);
```

A fenti parancs létrehoz egy `users` nevű táblát négy oszloppal.

## Gyakori adattípusok

|Adattípus|Jelentés|Példa|
|---|---|---|
|`INTEGER`|Egész szám|`25`|
|`BIGINT`|Nagy egész szám|`9000000000`|
|`NUMERIC(10,2)`|Pontos tizedes szám|`1999.99`|
|`REAL`|Lebegőpontos szám|`12.5`|
|`VARCHAR(100)`|Legfeljebb 100 karakteres szöveg|`'Krisz'`|
|`TEXT`|Tetszőleges hosszúságú szöveg|`'Hosszabb leírás'`|
|`BOOLEAN`|Logikai érték|`TRUE`, `FALSE`|
|`DATE`|Dátum|`'2026-07-30'`|
|`TIME`|Időpont dátum nélkül|`'14:30:00'`|
|`TIMESTAMP`|Dátum és idő|`'2026-07-30 14:30:00'`|
|`TIMESTAMPTZ`|Dátum és idő időzónával|`CURRENT_TIMESTAMP`|
|`UUID`|Egyedi azonosító|UUID érték|
|`JSONB`|JSON-adatok hatékony tárolása|`'{"theme":"dark"}'`|

> [!tip]  
> Pénz és más pontos tizedes adatok tárolására inkább `NUMERIC` típust használjunk. A `REAL` és `DOUBLE PRECISION` típusoknál előfordulhatnak lebegőpontos kerekítési eltérések.

---

# 6. Elsődleges kulcs és automatikus azonosító

Az **elsődleges kulcs**, vagyis `PRIMARY KEY`, egyedileg azonosít minden sort.

## Modern megoldás: `IDENTITY`

```sql
CREATE TABLE users (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    age INTEGER,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

Ebben a táblában:

- az `id` automatikusan növekszik;
    
- a `name` és az `email` kötelező;
    
- az `email` nem ismétlődhet;
    
- az `is_active` alapértelmezetten `TRUE`;
    
- a `created_at` automatikusan megkapja az aktuális időpontot.
    

## Régebbi, de gyakori megoldás: `SERIAL`

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

> [!info]  
> A `SERIAL` PostgreSQL-specifikus, régebbi megoldás. Új tábláknál általában a szabványosabb `GENERATED ... AS IDENTITY` használata ajánlott.

---

# 7. Fontos megszorítások

A megszorítások szabályokat adnak meg az oszlopokhoz.

|Megszorítás|Jelentés|
|---|---|
|`PRIMARY KEY`|Egyedi, kötelező azonosító|
|`FOREIGN KEY`|Kapcsolat egy másik táblával|
|`NOT NULL`|Az érték nem lehet `NULL`|
|`UNIQUE`|Az érték nem ismétlődhet|
|`DEFAULT`|Alapértelmezett érték|
|`CHECK`|Saját ellenőrzési feltétel|

## `CHECK` használata

```sql
CREATE TABLE products (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(150) NOT NULL,
    price NUMERIC(10, 2) NOT NULL CHECK (price >= 0),
    stock INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0)
);
```

Az adatbázis nem enged negatív árat vagy készletet megadni.

---

# 8. Kapcsolat létrehozása táblák között

Tegyük fel, hogy minden projekt egy felhasználóhoz tartozik.

```sql
CREATE TABLE projects (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id INTEGER NOT NULL,
    name VARCHAR(150) NOT NULL,
    description TEXT,
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_project_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT valid_project_dates
        CHECK (end_date IS NULL OR start_date IS NULL OR end_date >= start_date)
);
```

A kapcsolat:

```text
users.id ← projects.user_id
```

Az `ON DELETE CASCADE` jelentése: ha egy felhasználót törlünk, akkor az összes hozzá tartozó projekt is automatikusan törlődik.

> [!warning]  
> Az `ON DELETE CASCADE` kényelmes, de veszélyes is lehet. Egyetlen szülőrekord törlése sok kapcsolódó adatot törölhet.

---

# 9. Tábla módosítása – `ALTER TABLE`

## Új oszlop hozzáadása

```sql
ALTER TABLE users
ADD COLUMN phone VARCHAR(30);
```

## Több oszlop hozzáadása

```sql
ALTER TABLE users
ADD COLUMN city VARCHAR(100),
ADD COLUMN birth_date DATE;
```

## Oszlop átnevezése

```sql
ALTER TABLE users
RENAME COLUMN name TO full_name;
```

## Tábla átnevezése

```sql
ALTER TABLE users
RENAME TO app_users;
```

## Oszlop adattípusának módosítása

```sql
ALTER TABLE users
ALTER COLUMN age TYPE SMALLINT;
```

Ha átalakításra is szükség van:

```sql
ALTER TABLE users
ALTER COLUMN age TYPE INTEGER
USING age::INTEGER;
```

## `NOT NULL` hozzáadása

```sql
ALTER TABLE users
ALTER COLUMN email SET NOT NULL;
```

## `NOT NULL` eltávolítása

```sql
ALTER TABLE users
ALTER COLUMN email DROP NOT NULL;
```

## Alapértelmezett érték megadása

```sql
ALTER TABLE users
ALTER COLUMN is_active SET DEFAULT TRUE;
```

## Oszlop törlése

```sql
ALTER TABLE users
DROP COLUMN phone;
```

---

# 10. Adatok beszúrása – `INSERT INTO`

## Egy sor beszúrása

```sql
INSERT INTO users (name, email, age)
VALUES ('Krisz', 'krisz@example.com', 30);
```

Az `id`, az `is_active` és a `created_at` értékét nem kell megadni, mert az adatbázis automatikusan létrehozza őket.

## Több sor beszúrása

```sql
INSERT INTO users (name, email, age)
VALUES
    ('Anna', 'anna@example.com', 25),
    ('Béla', 'bela@example.com', 32),
    ('Csilla', 'csilla@example.com', 28);
```

## Beszúrt sor visszaadása

```sql
INSERT INTO users (name, email, age)
VALUES ('Dávid', 'david@example.com', 35)
RETURNING *;
```

Csak az új azonosító visszaadása:

```sql
INSERT INTO users (name, email)
VALUES ('Edit', 'edit@example.com')
RETURNING id;
```

> [!tip]  
> A `RETURNING` különösen hasznos backend fejlesztésnél, mert rögtön visszakaphatjuk a létrehozott rekordot vagy annak `id` értékét.

---

# 11. Adatok lekérdezése – `SELECT`

## Minden oszlop lekérdezése

```sql
SELECT *
FROM users;
```

## Meghatározott oszlopok lekérdezése

```sql
SELECT id, name, email
FROM users;
```

## Oszlop átnevezése az eredményben

```sql
SELECT
    name AS felhasznalo_neve,
    email AS email_cim
FROM users;
```

Az `AS` csak a lekérdezés eredményében nevezi át az oszlopot, a táblában nem.

## Ismétlődő értékek kiszűrése

```sql
SELECT DISTINCT city
FROM users;
```

---

# 12. Szűrés – `WHERE`

## Egyenlőség

```sql
SELECT *
FROM users
WHERE age = 30;
```

## Összehasonlító operátorok

|Operátor|Jelentés|
|---|---|
|`=`|Egyenlő|
|`<>` vagy `!=`|Nem egyenlő|
|`>`|Nagyobb|
|`<`|Kisebb|
|`>=`|Nagyobb vagy egyenlő|
|`<=`|Kisebb vagy egyenlő|

Példa:

```sql
SELECT *
FROM users
WHERE age >= 18;
```

## Több feltétel

```sql
SELECT *
FROM users
WHERE age >= 18
  AND is_active = TRUE;
```

```sql
SELECT *
FROM users
WHERE city = 'Pécs'
   OR city = 'Budapest';
```

## Feltétel tagadása

```sql
SELECT *
FROM users
WHERE NOT is_active;
```

---

# 13. Szöveges keresés – `LIKE` és `ILIKE`

## `LIKE`

A `LIKE` kis- és nagybetűérzékeny keresést végez.

```sql
SELECT *
FROM users
WHERE name LIKE 'K%';
```

Ez minden olyan nevet visszaad, amely `K` betűvel kezdődik.

## Helyettesítő karakterek

|Karakter|Jelentés|
|---|---|
|`%`|Tetszőleges számú karakter|
|`_`|Pontosan egy karakter|

Példák:

```sql
-- K betűvel kezdődik
SELECT * FROM users
WHERE name LIKE 'K%';
```

```sql
-- Tartalmazza az "ris" szöveget
SELECT * FROM users
WHERE name LIKE '%ris%';
```

```sql
-- Pontosan négy karakter hosszú
SELECT * FROM users
WHERE name LIKE '____';
```

## `ILIKE`

Az `ILIKE` nem különbözteti meg a kis- és nagybetűket.

```sql
SELECT *
FROM users
WHERE name ILIKE '%krisz%';
```

> [!info]  
> Az `ILIKE` PostgreSQL-specifikus parancs.

---

# 14. `IN`, `BETWEEN` és `NULL`

## Több lehetséges érték – `IN`

```sql
SELECT *
FROM users
WHERE city IN ('Pécs', 'Budapest', 'Szeged');
```

Ez rövidebb, mint:

```sql
SELECT *
FROM users
WHERE city = 'Pécs'
   OR city = 'Budapest'
   OR city = 'Szeged';
```

## Értéktartomány – `BETWEEN`

```sql
SELECT *
FROM users
WHERE age BETWEEN 18 AND 30;
```

A két szélső érték is beletartozik a tartományba.

## Hiányzó érték – `NULL`

```sql
SELECT *
FROM users
WHERE phone IS NULL;
```

```sql
SELECT *
FROM users
WHERE phone IS NOT NULL;
```

> [!danger]  
> A `NULL` értéket nem szabad `= NULL` formában vizsgálni.
> 
> Hibás:
> 
> ```sql
> WHERE phone = NULL
> ```
> 
> Helyes:
> 
> ```sql
> WHERE phone IS NULL
> ```

A `NULL` nem üres szöveget és nem nullát jelent, hanem azt, hogy az érték **ismeretlen vagy nincs megadva**.

---

# 15. Rendezés – `ORDER BY`

## Növekvő sorrend

```sql
SELECT *
FROM users
ORDER BY age ASC;
```

Az `ASC` az alapértelmezett, ezért elhagyható:

```sql
SELECT *
FROM users
ORDER BY age;
```

## Csökkenő sorrend

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

## Rendezés több oszlop alapján

```sql
SELECT *
FROM users
ORDER BY city ASC, name ASC;
```

Először város, azon belül név alapján rendez.

---

# 16. Találatok korlátozása – `LIMIT` és `OFFSET`

## Első öt rekord

```sql
SELECT *
FROM users
ORDER BY id
LIMIT 5;
```

## Lapozás

```sql
SELECT *
FROM users
ORDER BY id
LIMIT 10
OFFSET 20;
```

Ez kihagyja az első 20 rekordot, majd visszaadja a következő 10-et.

A hagyományos lapozás képlete:

```text
OFFSET = (oldalszám - 1) × oldalméret
```

Például a harmadik oldal tízes oldalmérettel:

```text
OFFSET = (3 - 1) × 10 = 20
```

---

# 17. Adatok módosítása – `UPDATE`

## Egy felhasználó módosítása

```sql
UPDATE users
SET age = 31
WHERE id = 1;
```

## Több oszlop módosítása

```sql
UPDATE users
SET
    name = 'Király Krisztián',
    age = 31,
    is_active = TRUE
WHERE id = 1;
```

## Módosított sor visszaadása

```sql
UPDATE users
SET age = age + 1
WHERE id = 1
RETURNING *;
```

> [!danger]  
> Ha elhagyod a `WHERE` feltételt, az összes sor módosul:
> 
> ```sql
> UPDATE users
> SET is_active = FALSE;
> ```
> 
> Ez nem SQL-hiba. Az adatbázis pontosan végrehajtja a kérést, aztán neked marad a halk káromkodás.

---

# 18. Adatok törlése – `DELETE`

## Egy rekord törlése

```sql
DELETE FROM users
WHERE id = 5;
```

## Több rekord törlése

```sql
DELETE FROM users
WHERE is_active = FALSE;
```

## Törölt rekord visszaadása

```sql
DELETE FROM users
WHERE id = 5
RETURNING *;
```

## Minden rekord törlése

```sql
DELETE FROM users;
```

> [!danger]  
> A `DELETE` parancsnál mindig ellenőrizd a `WHERE` feltételt. Biztonságos módszer, ha először ugyanazzal a feltétellel futtatsz egy `SELECT` lekérdezést:
> 
> ```sql
> SELECT *
> FROM users
> WHERE is_active = FALSE;
> ```
> 
> Ha valóban ezeket akarod törölni:
> 
> ```sql
> DELETE FROM users
> WHERE is_active = FALSE;
> ```

---

# 19. Összesítő függvények

## Sorok megszámolása – `COUNT`

```sql
SELECT COUNT(*)
FROM users;
```

Csak az aktív felhasználók száma:

```sql
SELECT COUNT(*) AS active_user_count
FROM users
WHERE is_active = TRUE;
```

## Összegzés – `SUM`

```sql
SELECT SUM(price) AS total_price
FROM products;
```

## Átlag – `AVG`

```sql
SELECT AVG(age) AS average_age
FROM users;
```

## Minimum – `MIN`

```sql
SELECT MIN(age) AS youngest_age
FROM users;
```

## Maximum – `MAX`

```sql
SELECT MAX(age) AS oldest_age
FROM users;
```

---

# 20. Csoportosítás – `GROUP BY`

A `GROUP BY` az azonos értékekkel rendelkező sorokat csoportosítja.

## Felhasználók száma városonként

```sql
SELECT
    city,
    COUNT(*) AS user_count
FROM users
GROUP BY city;
```

## Csoportok szűrése – `HAVING`

```sql
SELECT
    city,
    COUNT(*) AS user_count
FROM users
GROUP BY city
HAVING COUNT(*) >= 5;
```

Ez csak azokat a városokat adja vissza, amelyekhez legalább öt felhasználó tartozik.

> [!info] `WHERE` és `HAVING`
> 
> - A `WHERE` a csoportosítás előtt, az egyes sorokat szűri.
>     
> - A `HAVING` a csoportosítás után, a létrejött csoportokat szűri.
>     

Példa mindkettő használatára:

```sql
SELECT
    city,
    COUNT(*) AS active_user_count
FROM users
WHERE is_active = TRUE
GROUP BY city
HAVING COUNT(*) >= 5;
```

---

# 21. Táblák összekapcsolása – `JOIN`

A `JOIN` segítségével több kapcsolódó táblából kérhetünk le adatokat.

Legyen két táblánk:

```text
users.id ← projects.user_id
```

## `INNER JOIN`

Csak azokat a sorokat adja vissza, amelyek mindkét táblában rendelkeznek kapcsolódó rekorddal.

```sql
SELECT
    users.name AS user_name,
    projects.name AS project_name
FROM users
INNER JOIN projects
    ON projects.user_id = users.id;
```

Rövidebb, aliasokat használó változat:

```sql
SELECT
    u.name AS user_name,
    p.name AS project_name
FROM users AS u
INNER JOIN projects AS p
    ON p.user_id = u.id;
```

## `LEFT JOIN`

Minden felhasználót visszaad, akkor is, ha nincs projektje.

```sql
SELECT
    u.name AS user_name,
    p.name AS project_name
FROM users AS u
LEFT JOIN projects AS p
    ON p.user_id = u.id;
```

Ha egy felhasználónak nincs projektje, akkor a projekt oszlopai `NULL` értékűek lesznek.

## Projekttel nem rendelkező felhasználók

```sql
SELECT
    u.id,
    u.name
FROM users AS u
LEFT JOIN projects AS p
    ON p.user_id = u.id
WHERE p.id IS NULL;
```

## További `JOIN` típusok

|Típus|Eredmény|
|---|---|
|`INNER JOIN`|Csak a mindkét oldalon kapcsolódó sorok|
|`LEFT JOIN`|A bal tábla minden sora|
|`RIGHT JOIN`|A jobb tábla minden sora|
|`FULL JOIN`|Mindkét tábla minden sora|
|`CROSS JOIN`|Minden sort minden sorral összepárosít|

> [!tip]  
> A gyakorlatban az `INNER JOIN` és a `LEFT JOIN` messze a leggyakoribb.

---

# 22. Allekérdezések

Egy lekérdezés egy másik lekérdezésen belül is használható.

## Átlagéletkornál idősebb felhasználók

```sql
SELECT *
FROM users
WHERE age > (
    SELECT AVG(age)
    FROM users
);
```

## Olyan felhasználók, akiknek van projektjük

```sql
SELECT *
FROM users
WHERE id IN (
    SELECT user_id
    FROM projects
);
```

Ugyanez gyakran `EXISTS` használatával hatékonyabban és érthetőbben írható le:

```sql
SELECT *
FROM users AS u
WHERE EXISTS (
    SELECT 1
    FROM projects AS p
    WHERE p.user_id = u.id
);
```

---

# 23. Indexek

Az index felgyorsíthatja a keresést, rendezést és összekapcsolást.

## Index létrehozása

```sql
CREATE INDEX idx_users_name
ON users(name);
```

## Több oszlopos index

```sql
CREATE INDEX idx_users_city_name
ON users(city, name);
```

## Egyedi index

```sql
CREATE UNIQUE INDEX idx_users_email
ON users(email);
```

## Index törlése

```sql
DROP INDEX idx_users_name;
```

> [!note]  
> A `PRIMARY KEY` és `UNIQUE` megszorításokhoz a PostgreSQL automatikusan létrehozza a szükséges egyedi indexet.

> [!warning]  
> Az indexek gyorsítják az olvasást, de:
> 
> - tárhelyet foglalnak;
>     
> - lassíthatják az `INSERT`, `UPDATE` és `DELETE` műveleteket;
>     
> - felesleges minden oszlopra indexet készíteni.
>     
> 
> Tipikusan azokra az oszlopokra érdemes indexet tenni, amelyek gyakran szerepelnek `WHERE`, `JOIN` vagy `ORDER BY` műveletekben.

A külső kulcs oszlopára például érdemes lehet indexet tenni:

```sql
CREATE INDEX idx_projects_user_id
ON projects(user_id);
```

---

# 24. Tábla kiürítése – `TRUNCATE`

```sql
TRUNCATE TABLE users;
```

A `TRUNCATE` gyorsan eltávolítja a tábla összes sorát.

Az automatikus azonosító újraindítása:

```sql
TRUNCATE TABLE users
RESTART IDENTITY;
```

Kapcsolódó táblák kiürítése:

```sql
TRUNCATE TABLE users
RESTART IDENTITY
CASCADE;
```

> [!danger]  
> A `CASCADE` a kapcsolódó táblák adatait is törölheti. Ezt csak akkor használd, ha pontosan tudod, mely táblák kapcsolódnak egymáshoz.

## `DELETE` és `TRUNCATE` különbsége

|`DELETE`|`TRUNCATE`|
|---|---|
|Használhat `WHERE` feltételt|Nem használhat `WHERE` feltételt|
|Kiválasztott sorokat is törölhet|Az összes sort törli|
|Soronként dolgozik|Általában gyorsabb|
|Nem indítja újra automatikusan az azonosítót|`RESTART IDENTITY` használható|

---

# 25. Tábla törlése – `DROP TABLE`

```sql
DROP TABLE users;
```

Csak akkor törölje, ha létezik:

```sql
DROP TABLE IF EXISTS users;
```

Kapcsolódó objektumokkal együtt:

```sql
DROP TABLE users CASCADE;
```

> [!danger]  
> A `DROP TABLE` nemcsak a tábla adatait, hanem magát a táblát és annak szerkezetét is törli.

---

# 26. Tranzakciók

A tranzakció több adatbázis-műveletet egyetlen egységként kezel.

## Tranzakció indítása

```sql
BEGIN;
```

## Módosítások véglegesítése

```sql
COMMIT;
```

## Módosítások visszavonása

```sql
ROLLBACK;
```

## Példa pénzátutalásra

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 10000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 10000
WHERE id = 2;

COMMIT;
```

Ha valami probléma történik:

```sql
ROLLBACK;
```

> [!info]  
> A tranzakció lényege: vagy minden művelet sikeresen megtörténik, vagy egyik sem marad végleges.

## Mentési pont – `SAVEPOINT`

```sql
BEGIN;

UPDATE users
SET is_active = FALSE
WHERE id = 1;

SAVEPOINT first_update;

UPDATE users
SET is_active = FALSE
WHERE id = 2;

ROLLBACK TO first_update;

COMMIT;
```

Ebben az esetben a második módosítás visszavonódik, az első viszont megmarad.

---

# 27. Nézetek – `VIEW`

A nézet egy elmentett lekérdezés, amelyet táblához hasonlóan használhatunk.

## Nézet létrehozása

```sql
CREATE VIEW active_users AS
SELECT
    id,
    name,
    email
FROM users
WHERE is_active = TRUE;
```

## Nézet lekérdezése

```sql
SELECT *
FROM active_users;
```

## Nézet törlése

```sql
DROP VIEW active_users;
```

> [!info]  
> Egy hagyományos `VIEW` általában nem tárol külön adatmásolatot. Lekérdezéskor az alapjául szolgáló SQL-lekérdezés fut le.

---

# 28. Dátum- és időkezelés

## Aktuális dátum

```sql
SELECT CURRENT_DATE;
```

## Aktuális dátum és idő

```sql
SELECT CURRENT_TIMESTAMP;
```

## Dátumhoz idő hozzáadása

```sql
SELECT CURRENT_DATE + INTERVAL '7 days';
```

## Egy hónappal ezelőtti rekordok

```sql
SELECT *
FROM users
WHERE created_at >= CURRENT_TIMESTAMP - INTERVAL '1 month';
```

## Dátum részeinek lekérése

```sql
SELECT
    EXTRACT(YEAR FROM created_at) AS year,
    EXTRACT(MONTH FROM created_at) AS month
FROM users;
```

---

# 29. `CASE` feltételes kifejezés

A `CASE` feltétel alapján különböző értékeket adhat vissza.

```sql
SELECT
    name,
    age,
    CASE
        WHEN age < 18 THEN 'Kiskorú'
        WHEN age < 65 THEN 'Felnőtt'
        ELSE 'Nyugdíjas korú'
    END AS age_category
FROM users;
```

A `CASE` nem módosítja az adatokat, csak kiszámít egy értéket a lekérdezés eredményéhez.

---

# 30. `NULL` érték helyettesítése – `COALESCE`

A `COALESCE` az első nem `NULL` értéket adja vissza.

```sql
SELECT
    name,
    COALESCE(phone, 'Nincs megadva') AS phone
FROM users;
```

Több értékkel:

```sql
SELECT
    COALESCE(nickname, name, email, 'Ismeretlen')
FROM users;
```

---

# 31. Upsert – `ON CONFLICT`

Az upsert jelentése:

- ha még nincs ilyen rekord, szúrja be;
    
- ha már létezik, módosítsa vagy hagyja figyelmen kívül.
    

Ehhez szükség van egy `UNIQUE` vagy `PRIMARY KEY` megszorításra.

## Ütközés figyelmen kívül hagyása

```sql
INSERT INTO users (name, email)
VALUES ('Krisz', 'krisz@example.com')
ON CONFLICT (email) DO NOTHING;
```

## Meglévő rekord módosítása

```sql
INSERT INTO users (name, email, age)
VALUES ('Krisz', 'krisz@example.com', 31)
ON CONFLICT (email)
DO UPDATE SET
    name = EXCLUDED.name,
    age = EXCLUDED.age
RETURNING *;
```

Az `EXCLUDED` a beszúrni próbált új rekordot jelenti.

---

# 32. Teljes gyakorlati példa

## 1. Felhasználók táblája

```sql
CREATE TABLE users (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## 2. Projektek táblája

```sql
CREATE TABLE projects (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id INTEGER NOT NULL,
    name VARCHAR(150) NOT NULL,
    description TEXT,
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_projects_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT chk_project_dates
        CHECK (
            end_date IS NULL
            OR start_date IS NULL
            OR end_date >= start_date
        )
);
```

## 3. Feladatok táblája

```sql
CREATE TABLE tasks (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    project_id INTEGER NOT NULL,
    name VARCHAR(150) NOT NULL,
    description TEXT,
    is_completed BOOLEAN NOT NULL DEFAULT FALSE,
    due_date DATE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_tasks_project
        FOREIGN KEY (project_id)
        REFERENCES projects(id)
        ON DELETE CASCADE
);
```

## 4. Indexek

```sql
CREATE INDEX idx_projects_user_id
ON projects(user_id);

CREATE INDEX idx_tasks_project_id
ON tasks(project_id);

CREATE INDEX idx_tasks_is_completed
ON tasks(is_completed);
```

## 5. Felhasználó beszúrása

```sql
INSERT INTO users (name, email)
VALUES ('Krisz', 'krisz@example.com')
RETURNING *;
```

Tegyük fel, hogy az új felhasználó `id` értéke `1`.

## 6. Projekt beszúrása

```sql
INSERT INTO projects (
    user_id,
    name,
    description,
    start_date
)
VALUES (
    1,
    'Projektmenedzser',
    'FastAPI és Next.js alapú projektmenedzser',
    CURRENT_DATE
)
RETURNING *;
```

Tegyük fel, hogy a projekt `id` értéke `1`.

## 7. Feladatok beszúrása

```sql
INSERT INTO tasks (
    project_id,
    name,
    due_date
)
VALUES
    (1, 'Adatbázis megtervezése', CURRENT_DATE + INTERVAL '3 days'),
    (1, 'FastAPI végpontok elkészítése', CURRENT_DATE + INTERVAL '7 days'),
    (1, 'Next.js frontend elkészítése', CURRENT_DATE + INTERVAL '14 days');
```

## 8. Projekt lekérdezése a feladatokkal együtt

```sql
SELECT
    p.id AS project_id,
    p.name AS project_name,
    u.name AS owner_name,
    t.id AS task_id,
    t.name AS task_name,
    t.is_completed
FROM projects AS p
INNER JOIN users AS u
    ON u.id = p.user_id
LEFT JOIN tasks AS t
    ON t.project_id = p.id
WHERE p.id = 1
ORDER BY t.id;
```

## 9. Feladat készre állítása

```sql
UPDATE tasks
SET is_completed = TRUE
WHERE id = 1
RETURNING *;
```

## 10. Elkészült feladatok számának lekérdezése

```sql
SELECT
    project_id,
    COUNT(*) AS completed_task_count
FROM tasks
WHERE is_completed = TRUE
GROUP BY project_id;
```

## 11. Projektek állapotának összesítése

```sql
SELECT
    p.id,
    p.name,
    COUNT(t.id) AS total_tasks,
    COUNT(t.id) FILTER (WHERE t.is_completed = TRUE) AS completed_tasks
FROM projects AS p
LEFT JOIN tasks AS t
    ON t.project_id = p.id
GROUP BY p.id, p.name
ORDER BY p.id;
```

> [!tip]  
> A `FILTER` PostgreSQLben nagyon kényelmes, amikor ugyanabban a lekérdezésben több különböző feltétel alapján akarunk összesíteni.

---

# 33. SQL-parancsok ajánlott sorrendje

Egy összetettebb `SELECT` lekérdezés általános sorrendje:

```sql
SELECT
    oszlopok
FROM tabla
JOIN masik_tabla
    ON kapcsolati_feltetel
WHERE sor_feltetel
GROUP BY csoportositas
HAVING csoport_feltetel
ORDER BY rendezes
LIMIT darabszam
OFFSET kihagyott_sorok;
```

Példa:

```sql
SELECT
    u.city,
    COUNT(p.id) AS project_count
FROM users AS u
LEFT JOIN projects AS p
    ON p.user_id = u.id
WHERE u.is_active = TRUE
GROUP BY u.city
HAVING COUNT(p.id) >= 2
ORDER BY project_count DESC
LIMIT 10;
```

---

# 34. Gyors puska

```sql
-- Adatbázis létrehozása
CREATE DATABASE adatbazis_neve;

-- Tábla létrehozása
CREATE TABLE tabla_neve (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- Oszlop hozzáadása
ALTER TABLE tabla_neve
ADD COLUMN description TEXT;

-- Adat beszúrása
INSERT INTO tabla_neve (name)
VALUES ('Példa');

-- Adatok lekérdezése
SELECT *
FROM tabla_neve;

-- Szűrt lekérdezés
SELECT *
FROM tabla_neve
WHERE id = 1;

-- Adat módosítása
UPDATE tabla_neve
SET name = 'Új név'
WHERE id = 1;

-- Adat törlése
DELETE FROM tabla_neve
WHERE id = 1;

-- Sorok megszámolása
SELECT COUNT(*)
FROM tabla_neve;

-- Rendezés
SELECT *
FROM tabla_neve
ORDER BY id DESC;

-- Első tíz rekord
SELECT *
FROM tabla_neve
LIMIT 10;

-- Tábla kiürítése
TRUNCATE TABLE tabla_neve
RESTART IDENTITY;

-- Tábla törlése
DROP TABLE IF EXISTS tabla_neve;
```

---

# 35. Legfontosabb biztonsági szabályok

> [!danger] `UPDATE` és `DELETE`  
> Módosítás vagy törlés előtt mindig ellenőrizd a `WHERE` feltételt egy `SELECT` lekérdezéssel.

> [!warning] Éles adatbázis  
> Éles környezetben szerkezeti módosításokat lehetőleg migrációkkal végezz, ne kézzel összevissza futtatott SQL-parancsokkal.

> [!tip] Tranzakció  
> Kockázatos módosításnál használj tranzakciót:
> 
> ```sql
> BEGIN;
> 
> UPDATE users
> SET is_active = FALSE
> WHERE id = 10;
> 
> -- Ellenőrzés
> SELECT *
> FROM users
> WHERE id = 10;
> 
> -- Ha minden jó:
> COMMIT;
> 
> -- Ha valami rossz:
> -- ROLLBACK;
> ```

> [!tip] Elnevezések  
> PostgreSQLben célszerű kisbetűs `snake_case` neveket használni:
> 
> ```text
> users
> project_tasks
> created_at
> user_id
> ```
> 
> Így elkerülhető az idézőjjeles azonosítók okozta felesleges szívás.