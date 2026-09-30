# Python `open()` függvény

Az `open()` egy Pythonba beépített függvény, amellyel **fájlokat nyithatunk meg olvasásra, írásra vagy módosításra**.

Az általunk vizsgált kódrészlet:

```python
def execute_file(self, file_path):
    if self.conn is None:
        raise dbConnectionNotExistsError(
            "Nincs adatbázis kapcsolat."
        )

    with open(file_path, "r", encoding="utf-8") as file:
        sql = file.read()
```

Ebben a kódban az `open()` megnyitja a `file_path` változóban megadott fájlt, majd annak teljes tartalmát beolvassa az `sql` változóba.

> [!info]  
> Az `open()` a Python beépített függvénye, ezért nem kell külön importálni.

---

## Az `open()` alapvető szintaxisa

```python
open(file, mode, encoding)
```

Példa:

```python
file = open("query.sql", "r", encoding="utf-8")
```

A paraméterek jelentése:

|Paraméter|Jelentése|
|---|---|
|`file`|A megnyitandó fájl neve vagy elérési útja|
|`mode`|Megadja, hogy milyen módban nyitjuk meg a fájlt|
|`encoding`|Megadja a fájl karakterkódolását|

## Python `open()` módok
| Mód    | Jelentése                   | Fájlnak léteznie kell? | Korábbi tartalom | Pozíció    |
| ------ | --------------------------- | ---------------------: | ---------------- | ---------- |
| `"r"`  | Olvasás                     |                   Igen | Megmarad         | Fájl eleje |
| `"w"`  | Írás                        |                    Nem | **Törlődik**     | Fájl eleje |
| `"a"`  | Hozzáfűzés                  |                    Nem | Megmarad         | Fájl vége  |
| `"x"`  | Kizárólagos létrehozás      |           Nem létezhet | Új fájl          | Fájl eleje |
| `"r+"` | Olvasás és írás             |                   Igen | Megmarad         | Fájl eleje |
| `"w+"` | Írás és olvasás             |                    Nem | **Törlődik**     | Fájl eleje |
| `"a+"` | Hozzáfűzés és olvasás       |                    Nem | Megmarad         | Fájl vége  |
| `"x+"` | Létrehozás, olvasás és írás |           Nem létezhet | Új fájl          | Fájl eleje |
## Kiegészítő módok
Ezeket az előző módok valamelyikével kombináljuk:

|Jelölés|Jelentése|Példa|
|---|---|---|
|`"t"`|Szöveges mód, ez az alapértelmezett|`"rt"`, `"wt"`|
|`"b"`|Bináris mód|`"rb"`, `"wb"`|
|`"+"`|Olvasás és írás engedélyezése|`"r+"`, `"w+"`|


---

# A fájl elérési útja

Az `open()` első paramétere a fájl elérési útja.

```python
open("query.sql", "r")
```

Ebben az esetben a Python az aktuális munkakönyvtárban keresi a `query.sql` fájlt.

Megadhatunk relatív elérési utat:

```python
open("sql/query.sql", "r")
```

Vagy abszolút elérési utat:

```python
open(
    "C:/Users/Krisz/Documents/Aiven/query.sql",
    "r"
)
```

> [!warning]  
> A relatív elérési út nem feltétlenül ahhoz a mappához képest értendő, ahol a Python-fájl található.
> 
> Alapértelmezetten az aktuális munkakönyvtárhoz képest értelmezi a Python.

---

# Mit jelent az `"r"`?

Az `"r"` a fájl megnyitási módja.

Az `r` az angol **read**, vagyis „olvasás” szó rövidítése.

```python
open("query.sql", "r")
```

Ez azt jelenti:

> Nyisd meg a `query.sql` fájlt olvasásra.

Ebben a módban:

- a fájl tartalmát elolvashatjuk;
    
- a fájlba nem írhatunk;
    
- a fájlnak már léteznie kell.
    

Ha a fájl nem létezik, a Python `FileNotFoundError` kivételt dob.

```python
open("nem_letezik.sql", "r")
```

Eredmény:

```text
FileNotFoundError
```

---

# Gyakori fájlmegnyitási módok

|Mód|Jelentése|
|---|---|
|`"r"`|Fájl megnyitása olvasásra|
|`"w"`|Fájl megnyitása írásra, a korábbi tartalom törlésével|
|`"a"`|Írás a fájl végére|
|`"x"`|Új fájl létrehozása, ha még nem létezik|
|`"b"`|Bináris mód|
|`"t"`|Szöveges mód|
|`"r+"`|Olvasás és írás|
|`"w+"`|Írás és olvasás, a korábbi tartalom törlésével|
|`"a+"`|Olvasás és hozzáfűzés|

---

## `"w"` – írás

A `w` az angol **write**, vagyis „írás” rövidítése.

```python
with open("example.txt", "w", encoding="utf-8") as file:
    file.write("Hello")
```

> [!warning]  
> Ha a fájl már létezik, a `"w"` mód törli annak teljes korábbi tartalmát.
> 
> Ez a kis rohadék nem kér megerősítést.

Ha a fájl nem létezik, a Python létrehozza.

---

## `"a"` – hozzáfűzés

Az `a` az angol **append**, vagyis „hozzáfűzés” rövidítése.

```python
with open("example.txt", "a", encoding="utf-8") as file:
    file.write("\nÚj sor")
```

Ebben az esetben a meglévő tartalom nem törlődik. Az új szöveg a fájl végére kerül.

---

## `"x"` – új fájl létrehozása

```python
with open("example.txt", "x", encoding="utf-8") as file:
    file.write("Új fájl")
```

Ha a fájl már létezik, a Python `FileExistsError` kivételt dob.

---

# Mit jelent az `encoding="utf-8"`?

Az `encoding` megadja, hogy a Python milyen karakterkódolással olvassa be a fájlt.

```python
encoding="utf-8"
```

A UTF-8 képes megfelelően kezelni például:

- a magyar ékezetes karaktereket;
    
- különböző nyelvek betűit;
    
- speciális karaktereket;
    
- emojikat.
    

Példa:

```python
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

> [!tip]  
> Szöveges fájloknál célszerű explicit módon megadni az `encoding="utf-8"` paramétert.
> 
> Enélkül a Python az operációs rendszer alapértelmezett karakterkódolását használhatja, ami különböző számítógépeken eltérő eredményt okozhat.

---

# Mit jelent a `with`?

A `with` segítségével egy erőforrást – ebben az esetben egy fájlt – **biztonságosan használhatunk**.

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
```

A `with` blokk:

1. megnyitja a fájlt;
    
2. átadja azt a `file` változónak;
    
3. végrehajtja a behúzott kódot;
    
4. automatikusan bezárja a fájlt.
    

> [!info]  
> A `with` nem az `open()` része.
> 
> A `with` egy külön Python-utasítás, amely egy úgynevezett **context managert** használ.

Az `open()` által visszaadott fájlobjektum context managerként használható.

---

# Mi az az `as file`?

Ebben a kódban:

```python
with open("query.sql", "r", encoding="utf-8") as file:
```

az `open()` által megnyitott fájlobjektumot a `file` nevű változóban tároljuk.

Ezután ezen keresztül használhatjuk a fájlt:

```python
file.read()
```

A `file` név nem kötelező. Lehetne például:

```python
with open("query.sql", "r", encoding="utf-8") as sql_file:
    sql = sql_file.read()
```

Ez sok esetben még beszédesebb elnevezés.

---

# Miért kell bezárni a fájlt?

A megnyitott fájl egy operációs rendszer által kezelt erőforrást használ.

Ha nem zárjuk be megfelelően:

- feleslegesen foglalhat erőforrást;
    
- az írási műveletek nem feltétlenül fejeződnek be megfelelően;
    
- problémák lehetnek a fájl későbbi használatával;
    
- túl sok nyitott fájl esetén hibát kaphatunk.
    

A fájlt manuálisan is bezárhatjuk:

```python
file = open("query.sql", "r", encoding="utf-8")

sql = file.read()

file.close()
```

Ez azonban problémás lehet:

```python
file = open("query.sql", "r", encoding="utf-8")

sql = file.read()

# Ha itt hiba történik, lehet, hogy ez már nem fut le.
file.close()
```

Ezért jobb a `with`:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
```

A fájl akkor is bezáródik, ha a `with` blokkon belül kivétel történik.

> [!tip]  
> Fájlkezelésnél szinte mindig a `with open(...)` forma a megfelelő megoldás.

---

# Mit csinál a `file.read()`?

A `read()` beolvassa a fájl teljes tartalmát, és egyetlen stringként adja vissza.

Tegyük fel, hogy a `query.sql` tartalma:

```sql
SELECT *
FROM users;
```

Python-kód:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
```

Ezután az `sql` változó értéke:

```python
"SELECT *\nFROM users;"
```

A változó típusa:

```python
print(type(sql))
```

Eredmény:

```text
<class 'str'>
```

Tehát a `read()` nem hajtja végre az SQL-utasítást. Csak **szövegként beolvassa** azt.

> [!warning]  
> Az `open()` és a `read()` nem tudja, hogy a fájl SQL-kódot tartalmaz.
> 
> A Python számára ez egyszerű szöveg. Az SQL végrehajtását később a `cursor.execute()` végzi el.

---

# Fájl beolvasási lehetőségek

## `read()`

A teljes fájlt egyetlen stringként olvassa be.

```python
content = file.read()
```

Ez SQL-fájloknál általában megfelelő:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
```

---

## `readline()`

Egyetlen sort olvas be.

```python
first_line = file.readline()
```

Minden újabb hívás a következő sort olvassa be:

```python
first_line = file.readline()
second_line = file.readline()
```

---

## `readlines()`

A fájl sorait listaként adja vissza.

```python
lines = file.readlines()
```

Például:

```python
[
    "SELECT *\n",
    "FROM users;\n"
]
```

---

## Bejárás `for` ciklussal

A fájl soronként közvetlenül is bejárható:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    for line in file:
        print(line)
```

Ez nagy fájloknál memóriahatékonyabb, mert nem feltétlenül tölti be egyszerre a teljes fájlt.

---

# A megadott metódus működése

```python
def execute_file(self, file_path):
    if self.conn is None:
        raise dbConnectionNotExistsError(
            "Nincs adatbázis kapcsolat."
        )

    with open(file_path, "r", encoding="utf-8") as file:
        sql = file.read()
```

## `file_path`

A `file_path` paraméter tartalmazza a megnyitandó fájl elérési útját.

Például:

```python
database.execute_file("sql/create_tables.sql")
```

Ekkor:

```python
file_path = "sql/create_tables.sql"
```

Az `open()` valójában ezt kapja:

```python
open(
    "sql/create_tables.sql",
    "r",
    encoding="utf-8"
)
```

---

# Hibakezelés fájlmegnyitásnál

Az `open()` különböző kivételeket dobhat.

|Kivétel|Mikor történhet?|
|---|---|
|`FileNotFoundError`|A fájl nem található|
|`PermissionError`|Nincs jogosultságunk a fájl megnyitásához|
|`IsADirectoryError`|Fájl helyett egy mappát próbálunk megnyitni|
|`UnicodeDecodeError`|A fájl karakterkódolása nem megfelelő|
|`OSError`|Egyéb operációs rendszerhez kapcsolódó fájlhiba|

Példa:

```python
try:
    with open(
        "query.sql",
        "r",
        encoding="utf-8"
    ) as file:
        sql = file.read()

except FileNotFoundError:
    print("Az SQL-fájl nem található.")

except PermissionError:
    print("Nincs jogosultság a fájl megnyitásához.")
```

---

# `open()` és `try-except`

Az `open()` által dobott kivételeket elkaphatjuk:

```python
def read_sql_file(file_path):
    try:
        with open(
            file_path,
            "r",
            encoding="utf-8"
        ) as file:
            return file.read()

    except FileNotFoundError:
        print("A fájl nem található.")
```

Ez azonban nem mindig a legjobb megoldás, mert a hiba eltűnhet a hívó fél elől.

Jobb megoldás lehet saját kivételt dobni:

```python
class SqlFileNotFoundError(Exception):
    pass
```

```python
def read_sql_file(file_path):
    try:
        with open(
            file_path,
            "r",
            encoding="utf-8"
        ) as file:
            return file.read()

    except FileNotFoundError as error:
        raise SqlFileNotFoundError(
            f"Az SQL-fájl nem található: {file_path}"
        ) from error
```

> [!info]  
> A `raise ... from error` megőrzi az eredeti kivételt is.
> 
> Így látható marad, hogy a saját kivétel mögött eredetileg egy `FileNotFoundError` történt.

---

# Biztonságosabb útvonalkezelés `pathlib` segítségével

A fájlútvonalakat string helyett `Path` objektummal is kezelhetjük:

```python
from pathlib import Path
```

```python
def read_sql_file(file_path):
    path = Path(file_path)

    with path.open("r", encoding="utf-8") as file:
        return file.read()
```

Még egyszerűbben:

```python
from pathlib import Path


def read_sql_file(file_path):
    return Path(file_path).read_text(
        encoding="utf-8"
    )
```

Ez ugyanúgy beolvassa a teljes fájlt stringként.

> [!tip]  
> Egyszerű fájlbeolvasásnál a `Path.read_text()` rövidebb lehet.
> 
> A `with open(...)` viszont fontos alapminta, ezért először ezt érdemes rendesen megérteni.

---

# Egy fontos SQL-es korlátozás

Ez a megoldás:

```python
cursor.execute(sql)
```

nem minden adatbázis-kliensnél képes több, pontosvesszővel elválasztott SQL-utasítást egyszerre végrehajtani.

Például egy fájl tartalma lehet:

```sql
CREATE TABLE users (...);

INSERT INTO users (...);

SELECT * FROM users;
```

Egyes driverek ezt egyetlen `execute()` hívással nem fogadják el.

> [!warning]  
> Az `open()` gond nélkül beolvassa a teljes fájlt.
> 
> A probléma ilyenkor nem a fájlbeolvasásnál, hanem az adatbázis-driver SQL-végrehajtási szabályainál jelentkezik.

---

# `open()` vs `Path.read_text()`

## `open()`

```python
with open(
    "query.sql",
    "r",
    encoding="utf-8"
) as file:
    sql = file.read()
```

Előnyei:

- jól látható a fájl megnyitása;
    
- hozzáférünk a fájlobjektumhoz;
    
- használhatunk `read()`, `readline()` és `readlines()` metódusokat;
    
- nagy fájlokat soronként is feldolgozhatunk.
    

---

## `Path.read_text()`

```python
from pathlib import Path

sql = Path("query.sql").read_text(
    encoding="utf-8"
)
```

Előnyei:

- rövidebb;
    
- egyszerű;
    
- jól olvasható;
    
- útvonalkezeléshez kényelmes.
    

---

# Összefoglalás

> [!summary]  
> Az `open()` egy beépített Python-függvény, amellyel fájlokat nyithatunk meg.
> 
> A vizsgált forma:
> 
> ```python
> with open(
>     file_path,
>     "r",
>     encoding="utf-8"
> ) as file:
>     sql = file.read()
> ```
> 
> jelentése:
> 
> 1. nyisd meg a `file_path` fájlt;
>     
> 2. olvasási módban nyisd meg;
>     
> 3. UTF-8 karakterkódolást használj;
>     
> 4. a fájlobjektum neve legyen `file`;
>     
> 5. olvasd be a teljes tartalmat az `sql` változóba;
>     
> 6. a blokk végén automatikusan zárd be a fájlt.
>     

---

## Mentális modell

```text
file_path
    ↓
open(file_path, "r", encoding="utf-8")
    ↓
fájl megnyitása olvasásra
    ↓
as file
    ↓
file.read()
    ↓
teljes tartalom egy stringben
    ↓
sql változó
    ↓
with blokk vége
    ↓
fájl automatikus bezárása
```

> [!tip]  
> A teljes kódrészletet fejben így olvasd:
> 
> **„Ha nincs adatbázis-kapcsolat, dobj hibát. Egyébként nyisd meg az SQL-fájlt olvasásra, olvasd be a teljes tartalmát, majd automatikusan zárd be a fájlt.”**