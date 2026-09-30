# Python `with` utasítás

A `with` utasítás Pythonban olyan erőforrások biztonságos kezelésére szolgál, amelyeket használat után megfelelően le kell zárni vagy fel kell szabadítani.

Gyakran használjuk:

- fájlok megnyitásakor;
    
- adatbázis-kapcsolatoknál;
    
- adatbázis-tranzakcióknál;
    
- hálózati kapcsolatoknál;
    
- zárolásoknál;
    
- ideiglenes erőforrásoknál.
    

Alapvető példa:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
```

> [!info]  
> A `with` gondoskodik arról, hogy az erőforrás használat után megfelelően le legyen zárva – akkor is, ha közben kivétel történik.

---

## Alapvető szintaxis

```python
with context_manager as variable:
    # Az erőforrás használata
```

Fájlkezelésnél:

```python
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

A részek jelentése:

|Rész|Jelentése|
|---|---|
|`with`|Context manager használatának kezdete|
|`open(...)`|Létrehozza és megnyitja a fájlobjektumot|
|`as file`|A megnyitott fájlobjektumot a `file` változóhoz rendeli|
|Behúzott blokk|Itt használhatjuk az erőforrást|
|Blokk vége|A fájl automatikusan bezáródik|

---

# Mit csinál a `with`?

A következő kód:

```python
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

lényegében ezt végzi el:

```python
file = open("example.txt", "r", encoding="utf-8")

try:
    content = file.read()
finally:
    file.close()
```

A `finally` blokk akkor is lefut, ha a `try` blokkon belül hiba történik.

Ezért a `with` használata biztonságosabb és rövidebb.

> [!info]  
> A `with` nem egyszerűen egy rövidített `try-finally`, de fájlkezelésnél a működését így a legegyszerűbb elképzelni.

---

# Mi az a context manager?

A **context manager** egy olyan objektum, amely meghatározza:

1. mi történjen egy blokkba történő belépéskor;
    
2. mi történjen a blokkból való kilépéskor.
    

Magyarul olyan, mint egy automatikus gondnok:

```text
belépés
   ↓
erőforrás előkészítése
   ↓
kód végrehajtása
   ↓
erőforrás felszabadítása
```

Az `open()` által visszaadott fájlobjektum context managerként használható.

```python
with open("example.txt", "r") as file:
    content = file.read()
```

Ebben az esetben:

- belépéskor megkapjuk a megnyitott fájlt;
    
- kilépéskor a fájl automatikusan bezáródik.
    

---

# Miért jobb a `with`?

## Fájlkezelés `with` nélkül

```python
file = open("example.txt", "r", encoding="utf-8")

content = file.read()

file.close()
```

Ez első ránézésre működik, de hiba esetén problémás lehet:

```python
file = open("example.txt", "r", encoding="utf-8")

content = file.read()

result = 10 / 0

file.close()
```

A nullával való osztás miatt a program kivételt dob:

```text
ZeroDivisionError
```

Emiatt ez a sor már nem fut le:

```python
file.close()
```

A fájl nyitva maradhat.

---

## Fájlkezelés `with` használatával

```python
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()

    result = 10 / 0
```

A `ZeroDivisionError` itt is bekövetkezik, de a fájl a blokkból való kilépéskor bezáródik.

> [!tip]  
> A `with` legnagyobb előnye nem az, hogy rövidebb a kód, hanem az, hogy biztonságosan elvégzi a szükséges takarítást.

---

# A változó a `with` blokk után

A `with` segítségével létrehozott változó a blokkon kívül is elérhető lehet:

```python
with open("example.txt", "r") as file:
    content = file.read()

print(file)
```

Viszont a fájl ekkor már zárva van:

```python
print(file.closed)
```

Eredmény:

```text
True
```

Ezért ez már hibát okoz:

```python
with open("example.txt", "r") as file:
    content = file.read()

file.read()
```

Eredmény:

```text
ValueError: I/O operation on closed file
```

A beolvasott tartalom azonban továbbra is használható:

```python
with open("example.txt", "r") as file:
    content = file.read()

print(content)
```

> [!info]  
> A fájlobjektum bezáródik, de a már beolvasott adat az adott változóban megmarad.

---

# A saját példánk

```python
def execute_file(self, file_path):
    if self.conn is None:
        raise dbConnectionNotExistsError(
            "Nincs adatbázis kapcsolat."
        )

    with open(file_path, "r", encoding="utf-8") as file:
        sql = file.read()
```

A `with` blokk:

```python
with open(file_path, "r", encoding="utf-8") as file:
    sql = file.read()
```

a következőket végzi el:

1. megnyitja a `file_path` által megadott fájlt;
    
2. olvasási módot használ;
    
3. UTF-8 karakterkódolást használ;
    
4. a fájlobjektumot a `file` változóhoz rendeli;
    
5. beolvassa a fájl teljes tartalmát;
    
6. az eredményt az `sql` változóban tárolja;
    
7. a blokk végén automatikusan bezárja a fájlt.
    

---

# A `with` és a behúzás

A `with` csak a behúzott kódblokkra vonatkozik:

```python
with open("query.sql", "r", encoding="utf-8") as file:
    sql = file.read()
    print(file.closed)

print(file.closed)
```

Eredmény:

```text
False
True
```

A blokkon belül:

```python
print(file.closed)
```

eredménye:

```text
False
```

A blokk után:

```python
print(file.closed)
```

eredménye:

```text
True
```

> [!warning]  
> A behúzás határozza meg, meddig használható nyitott állapotban az erőforrás.

---

# Kivétel történik a `with` blokkon belül

```python
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()
    raise ValueError("Valami elbaszódott.")
```

A következő történik:

1. a fájl megnyílik;
    
2. a tartalma beolvasódik;
    
3. létrejön a `ValueError`;
    
4. a `with` bezárja a fájlt;
    
5. a kivétel továbbterjed.
    

> [!info]  
> A `with` nem feltétlenül nyeli el a kivételt.
> 
> A fájl bezárása után a kivétel ugyanúgy továbbterjed, hacsak valahol nem kapjuk el `try-except` segítségével.

---

# `with` és `try-except` együtt

A `with` gondoskodik az erőforrás bezárásáról, a `try-except` pedig kezelheti a hibákat.

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
```

A feladatok elkülönülnek:

|Eszköz|Feladata|
|---|---|
|`with`|Erőforrás biztonságos kezelése|
|`try`|Hibára hajlamos kód kijelölése|
|`except`|A bekövetkező kivétel kezelése|
|`finally`|Mindenképpen végrehajtandó kód|

---

# Több erőforrás kezelése egyszerre

Egyetlen `with` utasítással több erőforrást is kezelhetünk:

```python
with open(
    "source.txt",
    "r",
    encoding="utf-8"
) as source_file, open(
    "destination.txt",
    "w",
    encoding="utf-8"
) as destination_file:
    content = source_file.read()
    destination_file.write(content)
```

Ez:

1. megnyitja a forrásfájlt olvasásra;
    
2. megnyitja a célfájlt írásra;
    
3. átmásolja a tartalmat;
    
4. mindkét fájlt automatikusan bezárja.
    

Olvashatóbb zárójeles formában:

```python
with (
    open(
        "source.txt",
        "r",
        encoding="utf-8"
    ) as source_file,
    open(
        "destination.txt",
        "w",
        encoding="utf-8"
    ) as destination_file,
):
    content = source_file.read()
    destination_file.write(content)
```

---

# A context manager belső működése

A context managerek két speciális metódust használnak:

```python
__enter__()
```

és:

```python
__exit__()
```

|Metódus|Feladata|
|---|---|
|`__enter__()`|Lefut a `with` blokkba való belépéskor|
|`__exit__()`|Lefut a `with` blokkból való kilépéskor|

A következő kód:

```python
with object as value:
    do_something()
```

leegyszerűsítve ezt jelenti:

```python
value = object.__enter__()

try:
    do_something()
finally:
    object.__exit__(...)
```

---

# Saját context manager készítése

Saját osztályt is alkalmassá tehetünk a `with` használatára:

```python
class DatabaseConnection:

    def __enter__(self):
        print("Kapcsolat megnyitása")
        return self

    def __exit__(
        self,
        exception_type,
        exception_value,
        traceback
    ):
        print("Kapcsolat bezárása")
```

Használata:

```python
with DatabaseConnection() as database:
    print("Adatbázis használata")
```

Eredmény:

```text
Kapcsolat megnyitása
Adatbázis használata
Kapcsolat bezárása
```

A működés sorrendje:

```text
DatabaseConnection létrehozása
            ↓
      __enter__()
            ↓
    with blokk kódja
            ↓
       __exit__()
```

---

# Mit kap meg a `__exit__()`?

A `__exit__()` három kivételhez kapcsolódó adatot kap:

```python
def __exit__(
    self,
    exception_type,
    exception_value,
    traceback
):
    ...
```

|Paraméter|Jelentése|
|---|---|
|`exception_type`|A kivétel típusa|
|`exception_value`|A konkrét kivételobjektum|
|`traceback`|A hiba keletkezésének helyét tartalmazó traceback|

Ha nem történik kivétel, ezek értéke `None`.

---

# Elnyelheti-e a context manager a kivételt?

Igen. Ha a `__exit__()` `True` értékkel tér vissza, akkor a kivétel nem terjed tovább.

```python
class ErrorHandler:

    def __enter__(self):
        return self

    def __exit__(
        self,
        exception_type,
        exception_value,
        traceback
    ):
        print("Hiba kezelve")
        return True
```

```python
with ErrorHandler():
    raise ValueError("Valami elromlott")

print("Program folytatódik")
```

Eredmény:

```text
Hiba kezelve
Program folytatódik
```

Ha a `__exit__()` `False` vagy `None` értéket ad vissza, akkor a kivétel továbbterjed.

> [!warning]  
> Kivételt csak indokolt esetben nyeleljünk el. Ellenkező esetben a program úgy tesz, mintha minden rendben lenne, miközben a háttérben már ég a pajta.

---

# Adatbázisos használat

Adatbázis-kurzorokat gyakran használhatunk `with` segítségével:

```python
with self.conn.cursor() as cursor:
    cursor.execute(sql)
    result = cursor.fetchall()
```

Ebben az esetben a `with` blokk végén a cursor automatikusan bezáródik.

Az adatbázis-driver viselkedésétől függően maga a kapcsolat is használható context managerként:

```python
with connection:
    with connection.cursor() as cursor:
        cursor.execute(sql)
```

> [!warning]  
> Az adatbázis-kapcsolat context managerének pontos működése driverenként eltérhet.
> 
> Egyes driverek automatikusan `commit()` vagy `rollback()` műveletet végeznek, mások pedig csak az erőforrás bezárását kezelik.

---

# `with` használata zárolásnál

Többszálú programozásnál egy zárolást is kezelhetünk vele:

```python
from threading import Lock

lock = Lock()

with lock:
    # Egyszerre csak egy szál futhat itt.
    update_shared_data()
```

A `with` automatikusan:

1. megszerzi a zárolást;
    
2. végrehajtja a blokkot;
    
3. feloldja a zárolást.
    

---

# Mikor használjunk `with` utasítást?

Használjuk, amikor egy erőforrást:

- meg kell nyitni és később be kell zárni;
    
- le kell foglalni és később fel kell szabadítani;
    
- hiba esetén is megfelelően le kell kezelni;
    
- csak egy meghatározott kódblokkon belül akarunk használni.
    

Tipikus példák:

```python
with open(...) as file:
    ...
```

```python
with connection.cursor() as cursor:
    ...
```

```python
with lock:
    ...
```

---

# `with` vs `try-except`

A `with` és a `try-except` nem egymás alternatívái.

## `with`

Az erőforrás életciklusát kezeli:

```python
with open("example.txt", "r") as file:
    content = file.read()
```

## `try-except`

A kivételeket kezeli:

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Nullával nem lehet osztani.")
```

## Együtt

```python
try:
    with open(
        "example.txt",
        "r",
        encoding="utf-8"
    ) as file:
        content = file.read()

except FileNotFoundError:
    print("A fájl nem található.")
```

---

# Összefoglalás

> [!summary]  
> A `with` utasítás context managerek használatára szolgál.
> 
> Alapvető forma:
> 
> ```python
> with context_manager as variable:
>     # erőforrás használata
> ```
> 
> Fájlkezelésnél:
> 
> ```python
> with open(
>     "example.txt",
>     "r",
>     encoding="utf-8"
> ) as file:
>     content = file.read()
> ```
> 
> A `with`:
> 
> - előkészíti az erőforrást;
>     
> - átadja egy változónak;
>     
> - végrehajtja a blokkot;
>     
> - végül biztonságosan felszabadítja az erőforrást.
>     

---

## Mentális modell

```text
with
  ↓
context manager
  ↓
__enter__()
  ↓
erőforrás előkészítése
  ↓
with blokk végrehajtása
  ↓
hiba történt?
 ↙           ↘
nem          igen
 ↓             ↓
__exit__()   __exit__()
  ↓             ↓
takarítás    takarítás
                ↓
        kivétel továbbterjedhet
```

> [!tip]  
> A `with` utasítást így érdemes fejben olvasni:
> 
> **„Használd ezt az erőforrást ebben a blokkban, majd a végén mindenképpen takaríts el magad után.”**