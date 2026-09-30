# Python `raise` utasítás

A `raise` utasítás Pythonban arra szolgál, hogy **szándékosan kivételt (`exception`) dobjunk**.

> [!info]  
> A `raise` lényegében ezt jelenti:  
> **„Itt hibás állapot történt, állítsuk meg a normál futást, és jelezzük a hibát.”**

---

## Alap szintaxis

```python
raise ExceptionType("Hibaüzenet")
```

Példa:

```python
age = -5

if age < 0:
    raise ValueError("Az életkor nem lehet negatív.")
```

Eredmény:

```text
ValueError: Az életkor nem lehet negatív.
```

---

## Miért használunk `raise`-t?

A `raise` akkor hasznos, amikor a program olyan állapotba kerül, amelyet **nem akarunk elfogadni**.

Például:

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Nullával nem lehet osztani.")

    return a / b
```

Normál eset:

```python
result = divide(10, 2)

print(result)
```

Eredmény:

```text
5.0
```

Hibás eset:

```python
divide(10, 0)
```

Eredmény:

```text
ValueError: Nullával nem lehet osztani.
```

> [!warning]  
> Amikor egy `raise` lefut, a normál programfutás azon a ponton megszakad, hacsak a kivételt valahol nem kezeljük `try-except` segítségével.

---

## `raise` és `try-except`

A `raise` által dobott kivételt el lehet kapni.

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Nullával nem lehet osztani.")

    return a / b


try:
    result = divide(10, 0)
    print(result)

except ValueError as e:
    print("Hiba történt:", e)
```

Eredmény:

```text
Hiba történt: Nullával nem lehet osztani.
```

> [!info]  
> A `raise` **eldobja** a kivételt.  
> Az `except` **elkapja** a kivételt.

---

## Gyakori exception típusok

Pythonban több beépített kivételtípus létezik.

## `ValueError`

Akkor célszerű használni, amikor az érték típusa megfelelő, de maga az érték hibás.

```python
def set_age(age):
    if age < 0:
        raise ValueError("Az életkor nem lehet negatív.")
```

---

## `TypeError`

Akkor használjuk, amikor nem megfelelő típusú adatot kapunk.

```python
def greet(name):
    if not isinstance(name, str):
        raise TypeError("A névnek stringnek kell lennie.")

    print("Hello", name)
```

Hibás használat:

```python
greet(42)
```

Eredmény:

```text
TypeError: A névnek stringnek kell lennie.
```

---

## `RuntimeError`

Olyan hibákra használható, amelyek futás közben jelentkeznek, és nincs rájuk pontosabb beépített exception.

```python
if database_connection is None:
    raise RuntimeError("Nincs adatbázis-kapcsolat.")
```

---

# `raise` saját függvényben

Példa felhasználó létrehozására:

```python
def create_user(username, age):

    if username == "":
        raise ValueError("A username nem lehet üres.")

    if age < 18:
        raise ValueError("A felhasználónak legalább 18 évesnek kell lennie.")

    print("Felhasználó létrehozva.")
```

Használat:

```python
create_user("Krisz", 30)
```

Ez működik.

Viszont:

```python
create_user("", 30)
```

hiba:

```text
ValueError: A username nem lehet üres.
```

---

# `raise` classban

OOP esetén különösen gyakori.

```python
class BankAccount:

    def __init__(self, balance):
        if balance < 0:
            raise ValueError("A kezdőegyenleg nem lehet negatív.")

        self.balance = balance
```

Használat:

```python
account = BankAccount(1000)
```

Ez működik.

Viszont:

```python
account = BankAccount(-500)
```

hiba:

```text
ValueError: A kezdőegyenleg nem lehet negatív.
```

> [!tip]  
> OOP-ban a `raise` nagyon hasznos arra, hogy megakadályozzuk hibás objektumok létrejöttét.

---

# Paraméterellenőrzés `raise` segítségével

Gyakori minta:

```python
def withdraw(amount):

    if amount <= 0:
        raise ValueError("Az összegnek pozitívnak kell lennie.")

    # további műveletek
```

Ez jobb, mint például:

```python
def withdraw(amount):

    if amount <= 0:
        print("Hibás összeg")
        return
```

Az első megoldás azért jobb, mert a hívó fél eldöntheti, hogyan akarja kezelni a hibát.

---

# `raise` továbbdobása

Az egyik fontos forma:

```python
raise
```

Ilyenkor nem adunk meg exception típust.

Ez csak akkor használható értelmesen, amikor már egy `except` blokkban vagyunk.

Példa:

```python
	def divide():
	    try:
	        return 10 / 0
	    except ZeroDivisionError:
	        print("Nullával próbáltál osztani")
	        raise
	
	
	divide()
	
	print("Program vége")
```
 
Itt:

1. Python dob egy `ZeroDivisionError` hibát.
2. Az `except` elkapja.
3. Kiírjuk a hibát.
4. A `raise` újra továbbdobja ugyanazt a hibát.

**Eredmény:** 
```bash
Nullával próbáltál osztani
Traceback ...
ZeroDivisionError: division by zero
```

Ez már ==**NEM FUT LE!**==
```python 
print("Program vége")
``` 

Viszont `raise` nélkül
**Eredmény:** 
```bash
Nullával próbáltál osztani 
Program vége
```

Ez így le fog futni!
```python 
print("Program vége")
```
 
> [!info]  
> A sima `raise` jelentése:
> 
> **„Dobd tovább ugyanazt a kivételt.”**

---

# Adatbázisos példa

Adatbázis-műveleteknél ez nagyon gyakori:

```python
try:
    cursor.execute(sql)
    conn.commit()

except Exception:
    conn.rollback()
    raise
```

Ez azt jelenti:

```text
SQL futtatása
      ↓
sikerült?
   ↙      ↘
 igen      nem
 ↓          ↓
commit    rollback
             ↓
           raise
```

> [!example]  
> Ha az SQL utasítás elbaszódik:
> 
> - a módosításokat visszavonjuk `rollback()` segítségével,
>     
> - majd a hibát továbbdobjuk `raise` segítségével.
>     

Ez azért jó, mert a hiba **nem tűnik el csendben**.

---

# Saját exception létrehozása

Saját exception típust is készíthetünk.

```python
class InsufficientBalanceError(Exception):
    pass
```

Ezután:

```python
def withdraw(balance, amount):

    if amount > balance:
        raise InsufficientBalanceError(
            "Nincs elegendő pénz a számlán."
        )

    return balance - amount
```

Használat:

```python
try:
    withdraw(1000, 5000)

except InsufficientBalanceError as e:
    print(e)
```

> [!tip]  
> Nagyobb alkalmazásokban a saját exceptionök sokkal átláthatóbb hibakezelést tesznek lehetővé.

---

# `raise` vs `return`

A kettő teljesen más.

## `return`

Normális programfutás esetén visszaad egy értéket.

```python
def divide(a, b):
    return a / b
```

---

## `raise`

Hibás állapotot jelez.

```python
def divide(a, b):

    if b == 0:
        raise ValueError("Nullával nem lehet osztani.")

    return a / b
```

Tehát:

```text
return
```

jelentése:

```text
A függvény sikeresen végzett.
```

Míg:

```text
raise
```

jelentése:

```text
A függvény nem tudta normálisan végrehajtani a feladatát.
```

---

# Összefoglalás

> [!summary]  
> A `raise` utasítással Pythonban **kivételt dobunk**.
> 
> Alap forma:
> 
> ```python
> raise ValueError("Hiba történt")
> ```
> 
> A kivételt `try-except` segítségével elkaphatjuk:
> 
> ```python
> try:
>     ...
> except ValueError:
>     ...
> ```
> 
> A sima:
> 
> ```python
> raise
> ```
> 
> egy már elkapott kivételt dob tovább.

## Mentális modell

```text
raise
   ↓
exception keletkezik
   ↓
Python keres egy megfelelő except blokkot
   ↓
talál?
 ↙    ↘
igen   nem
 ↓      ↓
kezelés program leáll
```

> [!tip]  
> Jó szabály:
> 
> Ha egy függvény olyan adatot vagy állapotot kap, amely mellett **nem tudja helyesen elvégezni a feladatát**, akkor gyakran `raise` a megfelelő megoldás.


## Raise típusai

| Exception             | Mikor használjuk?                                |
| --------------------- | ------------------------------------------------ |
| `ValueError`          | Jó típus, de hibás érték                         |
| `TypeError`           | Rossz adattípus                                  |
| `KeyError`            | Nincs ilyen kulcs egy dictionary-ben             |
| `IndexError`          | Nincs ilyen index egy listában                   |
| `AttributeError`      | Egy objektumnak nincs ilyen attribútuma/metódusa |
| `FileNotFoundError`   | A fájl nem található                             |
| `PermissionError`     | Nincs jogosultság                                |
| `ZeroDivisionError`   | Nullával osztás                                  |
| `RuntimeError`        | Általános futási hiba                            |
| `NotImplementedError` | Egy funkció még nincs implementálva              |
| `ConnectionError`     | Kapcsolati probléma                              |
| `TimeoutError`        | Időtúllépés                                      |
| `ImportError`         | Importálási probléma                             |
| `ModuleNotFoundError` | Modul nem található                              |
| `AssertionError`      | Assertion hibázott                               |
