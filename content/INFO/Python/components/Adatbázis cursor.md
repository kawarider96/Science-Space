# Python adatbázis-cursor

Az adatbázis-cursor egy olyan objektum, amelyen keresztül **SQL-utasításokat küldhetünk az adatbázisnak**, és lekérhetjük az adatbázis által visszaadott eredményeket.

Egyszerű példa:

```python
with self.conn.cursor() as cursor:
    cursor.execute("SELECT * FROM users")
    rows = cursor.fetchall()
```

> [!info]  
> A kapcsolatot a connection tartja fenn az adatbázissal, a cursor pedig ezen a kapcsolaton keresztül hajtja végre az SQL-utasításokat.

---

# Connection és cursor közötti különbség

A connection és a cursor nem ugyanazt jelenti.

|Objektum|Feladata|
|---|---|
|`connection`|Fenntartja a kapcsolatot a Python-program és az adatbázis között|
|`cursor`|SQL-utasításokat hajt végre a kapcsolaton keresztül|
|`cursor.execute()`|Elküldi az SQL-utasítást az adatbázisnak|
|`cursor.fetchone()`|Lekéri a következő eredménysort|
|`cursor.fetchall()`|Lekéri az összes eredménysort|
|`connection.commit()`|Véglegesíti a módosításokat|
|`connection.rollback()`|Visszavonja a még nem véglegesített módosításokat|

Mentális modell:

```text
Python-program
      ↓
connection
      ↓
cursor
      ↓
SQL-utasítás
      ↓
adatbázis
      ↓
eredmény
      ↓
cursor
      ↓
Python-program
```

> [!tip]  
> A connection olyan, mint egy telefonvonal az adatbázishoz.
> 
> A cursor pedig az, aki ezen a vonalon konkrét utasításokat mond az adatbázisnak.

---

# Cursor létrehozása

A cursort egy létező adatbázis-kapcsolatból hozzuk létre:

```python
cursor = connection.cursor()
```

OOP esetén:

```python
cursor = self.conn.cursor()
```

Itt:

- `self.conn` az adatbázis-kapcsolat;
    
- a `cursor()` metódus létrehozza a cursort;
    
- az eredmény a `cursor` változóba kerül.
    

---

# Cursor használata `with` segítségével

A biztonságosabb megoldás:

```python
with self.conn.cursor() as cursor:
    cursor.execute("SELECT * FROM users")
```

A `with` blokk:

1. létrehozza a cursort;
    
2. átadja a `cursor` változónak;
    
3. lehetővé teszi az SQL-utasítások végrehajtását;
    
4. a blokk végén automatikusan bezárja a cursort.
    

> [!info]  
> A cursor bezárása nem feltétlenül zárja be az adatbázis-kapcsolatot.
> 
> A `self.conn` kapcsolat továbbra is használható maradhat újabb cursorok létrehozására.

---

# Cursor használata `with` nélkül

A cursort manuálisan is létrehozhatjuk:

```python
cursor = self.conn.cursor()

cursor.execute("SELECT * FROM users")

rows = cursor.fetchall()

cursor.close()
```

Ebben az esetben nekünk kell bezárni:

```python
cursor.close()
```

Ha közben kivétel történik, ez a sor esetleg nem fut le:

```python
cursor = self.conn.cursor()

cursor.execute("HIBÁS SQL")

cursor.close()
```

Ezért célszerűbb a `with` használata:

```python
with self.conn.cursor() as cursor:
    cursor.execute("HIBÁS SQL")
```

A cursor akkor is bezáródik, ha az SQL végrehajtása közben kivétel történik.

---

# SQL végrehajtása `execute()` segítségével

A cursor `execute()` metódusa SQL-utasítást küld az adatbázisnak.

```python
with self.conn.cursor() as cursor:
    cursor.execute("SELECT * FROM users")
```

Az `execute()` paramétere egy string:

```python
sql = "SELECT * FROM users"

with self.conn.cursor() as cursor:
    cursor.execute(sql)
```

Az SQL külön fájlból is beolvasható:

```python
with open(
    "query.sql",
    "r",
    encoding="utf-8"
) as file:
    sql = file.read()

with self.conn.cursor() as cursor:
    cursor.execute(sql)
```

> [!warning]  
> A `cursor.execute()` hajtja végre az SQL-t.
> 
> A `file.read()` csak egyszerű szövegként olvassa be az SQL-fájl tartalmát.

---

# `SELECT` lekérdezés végrehajtása

Például legyen egy `users` táblánk:

|id|name|age|
|--:|---|--:|
|1|Krisz|30|
|2|Rebi|28|

Lekérdezés:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name, age FROM users"
    )

    rows = cursor.fetchall()
```

A `rows` eredménye általában sorokból álló lista:

```python
[
    (1, "Krisz", 30),
    (2, "Rebi", 28)
]
```

A pontos eredménytípus az adatbázis-drivertől és a cursor beállításaitól függhet.

---

# `fetchone()`

A `fetchone()` a következő eredménysort adja vissza:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name FROM users"
    )

    row = cursor.fetchone()
```

Eredmény:

```python
(1, "Krisz")
```

Ha nincs több eredménysor:

```python
row = cursor.fetchone()
```

az eredmény általában:

```python
None
```

---

# `fetchall()`

A `fetchall()` az összes még elérhető eredménysort lekéri:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name FROM users"
    )

    rows = cursor.fetchall()
```

Eredmény:

```python
[
    (1, "Krisz"),
    (2, "Rebi")
]
```

> [!warning]  
> Nagyon sok rekordnál a `fetchall()` jelentős memóriát használhat, mert egyszerre próbálja lekérni az összes eredményt.

---

# `fetchmany()`

A `fetchmany()` meghatározott számú eredménysort kér le:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name FROM users"
    )

    rows = cursor.fetchmany(10)
```

Ez legfeljebb tíz sort kér le.

Nagyobb eredményhalmazok feldolgozásánál hasznos lehet.

---

# Cursor bejárása ciklussal

Sok adatbázis-drivernél a cursor közvetlenül bejárható:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name FROM users"
    )

    for row in cursor:
        print(row)
```

Ez soronként dolgozza fel az eredményt:

```text
(1, 'Krisz')
(2, 'Rebi')
```

---

# Eredmények kiírása

>[!info]
> Itt a fetchall() -al elmentjük az egész cursor eredményét egy rows változóba ez memóriaigényesebb mint a fenti ahol közvetlen a cursorból olvassuk ki az eredményeket viszont az csak addig elérhető amíg a cursor nyitva van ezzel a fetchall()-os módszerrel pedig a cursor zárása után is megmaradnak az eredmények.

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name, age FROM users"
    )

    rows = cursor.fetchall()

for row in rows:
    print(row)
```

Eredmény:

```text
(1, 'Krisz', 30)
(2, 'Rebi', 28)
```

A tuple elemeit index alapján is elérhetjük:

```python
for row in rows:
    user_id = row[0]
    name = row[1]
    age = row[2]

    print(user_id, name, age)
```

---

# Oszlopnevek lekérése

A cursor `description` tulajdonsága információt tartalmazhat a lekérdezés eredményének oszlopairól:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name, age FROM users"
    )

    column_names = [
        column.name
        for column in cursor.description
    ]

    rows = cursor.fetchall()
```

A `column_names` értéke:

```python
["id", "name", "age"]
```

> [!info]  
> A `cursor.description` által visszaadott objektum pontos formája az adatbázis-drivertől függhet.
> 
> Egyes drivereknél az oszlopnév így érhető el:
> 
> ```python
> column[0]
> ```

Általánosabb megoldás:

```python
column_names = [
    column[0]
    for column in cursor.description
]
```

---

# Eredmény átalakítása dictionaryvé

Az oszlopnevek és az eredménysorok összekapcsolhatók:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT id, name, age FROM users"
    )

    column_names = [
        column[0]
        for column in cursor.description
    ]

    rows = cursor.fetchall()
```

```python
result = [
    dict(zip(column_names, row))
    for row in rows
]
```

Eredmény:

```python
[
    {
        "id": 1,
        "name": "Krisz",
        "age": 30
    },
    {
        "id": 2,
        "name": "Rebi",
        "age": 28
    }
]
```

A `zip()` összepárosítja az oszlopneveket az adott sor értékeivel:

```text
id   → 1
name → Krisz
age  → 30
```

---

## Dictionary cursor

Egyes adatbázis-driverek képesek az eredményeket közvetlenül dictionaryszerű objektumként visszaadni.

Psycopg esetén például használható a `dict_row`:

```python
from psycopg.rows import dict_row
```

```python
with self.conn.cursor(
    row_factory=dict_row
) as cursor:
    cursor.execute(
        "SELECT id, name, age FROM users"
    )

    rows = cursor.fetchall()
```

Eredmény:

```python
[
    {
        "id": 1,
        "name": "Krisz",
        "age": 30
    },
    {
        "id": 2,
        "name": "Rebi",
        "age": 28
    }
]
```

> [!tip]  
> Dictionaryszerű eredménynél a mezők név alapján érhetők el:
> 
> ```python
> row["name"]
> ```
> 
> Ez gyakran olvashatóbb, mint:
> 
> ```python
> row[1]
> ```

---

# Adatok beszúrása

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO users (name, age)
        VALUES (%s, %s)
        """,
        ("Krisz", 30)
    )

self.conn.commit()
```

A paraméterek külön tuple-ben szerepelnek:

```python
("Krisz", 30)
```

A driver biztonságosan behelyettesíti őket az SQL-utasításba.

> [!warning]  
> Az SQL-paramétereket ne f-stringgel vagy string-összefűzéssel helyettesítsük be!
> 
> Rossz:
> 
> ```python
> sql = f"""
> INSERT INTO users (name)
> VALUES ('{name}')
> """
> ```
> 
> Ez SQL injection sebezhetőséget okozhat.

Helyes:

```python
cursor.execute(
    """
    INSERT INTO users (name)
    VALUES (%s)
    """,
    (name,)
)
```

> [!info]  
> Az SQL-paraméterek jelölése adatbázis-driverenként eltérhet.
> 
> Psycopg esetén általában `%s` helyőrzőt használunk.

---

# Miért van vessző az `(name,)` végén?

Egyetlen elemű tuple létrehozásához vessző szükséges:

```python
(name,)
```

Ez tuple:

```python
type((name,))
```

Eredmény:

```text
<class 'tuple'>
```

Ez viszont csak egy zárójelbe tett érték:

```python
(name)
```

A típusa ugyanaz marad, mint a `name` változóé.

---

# Adatok módosítása

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        UPDATE users
        SET age = %s
        WHERE id = %s
        """,
        (31, 1)
    )

self.conn.commit()
```

A cursor végrehajtja az `UPDATE` utasítást, a connection pedig véglegesíti a módosítást:

```python
self.conn.commit()
```

---

# Adatok törlése

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        DELETE FROM users
        WHERE id = %s
        """,
        (1,)
    )

self.conn.commit()
```

> [!warning]  
> Ha a `DELETE` utasításból kimarad a `WHERE`, akkor az összes rekord törlődhet:
> 
> ```sql
> DELETE FROM users;
> ```
> 
> Ez nem „majd visszacsináljuk valahogy” kategória, ha már megtörtént a `commit()`.

---

# `commit()`

Az adatbázist módosító SQL-utasításokat általában véglegesíteni kell:

```python
self.conn.commit()
```

Tipikus módosító utasítások:

- `INSERT`
    
- `UPDATE`
    
- `DELETE`
    
- `CREATE TABLE`
    
- `ALTER TABLE`
    
- `DROP TABLE`
    

Példa:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO users (name, age)
        VALUES (%s, %s)
        """,
        ("Krisz", 30)
    )

self.conn.commit()
```

> [!info]  
> A `commit()` a connection metódusa, nem a cursoré.
> 
> Helyes:
> 
> ```python
> self.conn.commit()
> ```
> 
> Nem helyes:
> 
> ```python
> cursor.commit()
> ```

---

# `rollback()`

Ha az SQL végrehajtása közben hiba történik, a tranzakció módosításait visszavonhatjuk:

```python
try:
    with self.conn.cursor() as cursor:
        cursor.execute(sql)

    self.conn.commit()

except Exception:
    self.conn.rollback()
    raise
```

A folyamat:

```text
SQL végrehajtása
        ↓
   sikeres volt?
    ↙        ↘
  igen       nem
   ↓          ↓
commit     rollback
               ↓
             raise
```

> [!info]  
> A `rollback()` is a connection metódusa, mert a tranzakció a kapcsolathoz tartozik.

---

# `SELECT` esetén kell `commit()`?

Egy egyszerű `SELECT` nem módosítja az adatokat:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT * FROM users"
    )

    rows = cursor.fetchall()
```

Ezért az eredmény eltárolásához általában nincs szükség `commit()` műveletre.

A `commit()` elsősorban az adatbázist módosító tranzakciók véglegesítésére szolgál.

---

# `cursor.rowcount`

A `rowcount` megmutathatja, hány rekordot érintett az utolsó SQL-utasítás:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        UPDATE users
        SET active = %s
        WHERE age < %s
        """,
        (False, 18)
    )

    affected_rows = cursor.rowcount

self.conn.commit()
```

Kiírás:

```python
print(
    f"{affected_rows} rekord módosult."
)
```

> [!warning]  
> A `rowcount` viselkedése driverenként és utasítástípusonként eltérhet.
> 
> `SELECT` esetén nem minden driver tudja előre a teljes eredménysorok számát.

---

# Automatikusan generált ID lekérése

PostgreSQL esetén használhatjuk a `RETURNING` kulcsszót:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO users (name, age)
        VALUES (%s, %s)
        RETURNING id
        """,
        ("Krisz", 30)
    )

    new_user_id = cursor.fetchone()[0]

self.conn.commit()
```

A `RETURNING id` visszaadja az új rekord azonosítóját.

```python
print(new_user_id)
```

---

# Több rekord beszúrása

Több hasonló rekordhoz használható az `executemany()`:

```python
users = [
    ("Krisz", 30),
    ("Rebi", 28),
    ("Anna", 35)
]
```

```python
with self.conn.cursor() as cursor:
    cursor.executemany(
        """
        INSERT INTO users (name, age)
        VALUES (%s, %s)
        """,
        users
    )

self.conn.commit()
```

Az SQL minden tuple-re végrehajtódik.

---

# A cursor élettartama

A cursort általában csak addig tartjuk nyitva, ameddig szükségünk van rá:

```python
with self.conn.cursor() as cursor:
    cursor.execute(sql)
    rows = cursor.fetchall()
```

A blokk végén:

- a cursor bezáródik;
    
- a `rows` változóban lévő adatok megmaradnak;
    
- a connection továbbra is használható lehet.
    

```python
print(rows)
```

Ez a cursor bezárása után is működik, mert az eredményeket már eltároltuk a memóriában.

---

# Több cursor egy kapcsolaton

Egy connectionből több cursort is létrehozhatunk:

```python
with self.conn.cursor() as first_cursor:
    first_cursor.execute(
        "SELECT * FROM users"
    )

    users = first_cursor.fetchall()

with self.conn.cursor() as second_cursor:
    second_cursor.execute(
        "SELECT * FROM projects"
    )

    projects = second_cursor.fetchall()
```

Mindkét cursor ugyanazt a kapcsolatot használja, de külön SQL-műveleteket végez.

> [!warning]  
> Attól, hogy több cursort hozhatunk létre, ugyanaz a connection nem feltétlenül használható biztonságosan több szálból egyszerre.
> 
> Ez az adatbázis-drivertől és a connection kezelésétől függ.

---

# Hibakezelés cursor használatakor

```python
try:
    with self.conn.cursor() as cursor:
        cursor.execute(sql)

        if cursor.description is not None:
            result = cursor.fetchall()
        else:
            result = None

    self.conn.commit()

    return result

except Exception:
    self.conn.rollback()
    raise
```

A `cursor.description`:

- `SELECT` és más eredményt visszaadó utasításoknál általában nem `None`;
    
- eredményt nem visszaadó utasításoknál általában `None`.
    

---

# Egy általános `execute()` metódus

```python
class Database:

    def __init__(self):
        self.conn = None

    def execute(
        self,
        sql,
        parameters=None
    ):
        if self.conn is None:
            raise dbConnectionNotExistsError(
                "Nincs adatbázis-kapcsolat."
            )

        try:
            with self.conn.cursor() as cursor:
                cursor.execute(
                    sql,
                    parameters
                )

                if cursor.description is not None:
                    result = cursor.fetchall()
                else:
                    result = None

            self.conn.commit()

            return result

        except Exception:
            self.conn.rollback()
            raise
```

Használata `SELECT` esetén:

```python
users = database.execute(
    "SELECT * FROM users"
)

print(users)
```

Használata paraméterekkel:

```python
user = database.execute(
    """
    SELECT *
    FROM users
    WHERE id = %s
    """,
    (1,)
)
```

---

# SQL-fájl végrehajtása cursorral

```python
def execute_file(self, file_path):
    if self.conn is None:
        raise dbConnectionNotExistsError(
            "Nincs adatbázis-kapcsolat."
        )

    with open(
        file_path,
        "r",
        encoding="utf-8"
    ) as file:
        sql = file.read()

    try:
        with self.conn.cursor() as cursor:
            cursor.execute(sql)

            if cursor.description is not None:
                result = cursor.fetchall()
            else:
                result = None

        self.conn.commit()

        return result

    except Exception:
        self.conn.rollback()
        raise
```

A működés:

```text
SQL-fájl megnyitása
        ↓
SQL beolvasása stringként
        ↓
cursor létrehozása
        ↓
cursor.execute(sql)
        ↓
van visszaadott eredmény?
     ↙              ↘
   igen             nem
    ↓                ↓
fetchall()         None
     ↘              ↙
          commit
             ↓
          return
```

---

# Gyakori hibák

## Nincs adatbázis-kapcsolat

```python
self.conn = None
```

Ebben az esetben nem lehet cursort létrehozni:

```python
self.conn.cursor()
```

Ezért előtte ellenőrizzük:

```python
if self.conn is None:
    raise dbConnectionNotExistsError(
        "Nincs adatbázis-kapcsolat."
    )
```

---

## Elmarad a `fetch`

Ez csak végrehajtja a lekérdezést:

```python
cursor.execute(
    "SELECT * FROM users"
)
```

De az eredményt még nem helyezi automatikusan egy saját változóba.

Ehhez szükséges például:

```python
rows = cursor.fetchall()
```

---

## Elmarad a `commit()`

```python
cursor.execute(
    """
    INSERT INTO users (name)
    VALUES (%s)
    """,
    ("Krisz",)
)
```

Ha a tranzakciót nem véglegesítjük:

```python
self.conn.commit()
```

a módosítás később elveszhet vagy visszavonódhat.

---

## Elmarad a `rollback()`

Ha egy tranzakcióban hiba történik, a kapcsolat hibás tranzakciós állapotban maradhat.

```python
except Exception:
    self.conn.rollback()
    raise
```

A `rollback()` visszaállítja a kapcsolatot használható tranzakciós állapotba.

---

## Bezárt cursor használata

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT * FROM users"
    )

cursor.fetchall()
```

A `with` blokk után a cursor már zárva van, ezért ez hibát okozhat.

Helyesen:

```python
with self.conn.cursor() as cursor:
    cursor.execute(
        "SELECT * FROM users"
    )

    rows = cursor.fetchall()

print(rows)
```

---

# Összefoglalás

> [!summary]  
> A cursor egy adatbázis-műveleteket végrehajtó objektum.
> 
> A connectionből hozzuk létre:
> 
> ```python
> with self.conn.cursor() as cursor:
>     ...
> ```
> 
> SQL végrehajtása:
> 
> ```python
> cursor.execute(sql)
> ```
> 
> Eredmények lekérése:
> 
> ```python
> cursor.fetchone()
> cursor.fetchmany(10)
> cursor.fetchall()
> ```
> 
> Módosítások véglegesítése:
> 
> ```python
> self.conn.commit()
> ```
> 
> Módosítások visszavonása:
> 
> ```python
> self.conn.rollback()
> ```

---

## Mentális modell

```text
connection
    ↓
cursor létrehozása
    ↓
execute(SQL)
    ↓
adatbázis végrehajtja
    ↓
┌───────────────────────┐
│ SELECT                │
│      ↓                │
│ fetchone/fetchall     │
├───────────────────────┤
│ INSERT/UPDATE/DELETE  │
│      ↓                │
│ commit                │
└───────────────────────┘
    ↓
cursor bezárása
```

> [!tip]  
> A legegyszerűbb megfogalmazás:
> 
> **A connection összeköt az adatbázissal, a cursor pedig dolgozik rajta.**