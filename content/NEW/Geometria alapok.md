---
tags:
  - matematika
  - geometria
  - alapok
aliases:
  - Geometriai alapfogalmak
---
## 1. Pont, egyenes, félegyenes, szakasz

Ezekkel írjuk le az alakzatok helyét és felépítését.

| Fogalom        | Jelentés                                                      | Jelölés                                |
| -------------- | ------------------------------------------------------------- | -------------------------------------- |
| **Pont**       | Egy helyet jelöl; nincs hosszúsága, szélessége vagy magassága | $A$, $B$, $P$                          |
| **Egyenes**    | Mindkét irányban végtelen; nincs vastagsága                   | $e$, $f$                               |
| **Félegyenes** | Van kezdőpontja, és egy irányban végtelen                     | $A$-ból $B$ irányába induló félegyenes |
| **Szakasz**    | Egy egyenes két pont közötti része; két végpontja van         | $\overline{AB}$                        |

Az $AB$ szakasz hosszát például így írhatjuk:

$$
|\overline{AB}|=5\text{ cm}
$$

> [!important] A rajz és a matematikai objektum
> A papíron a pontnak mérete, az egyenesnek vastagsága van. Ezek csak az ábrázoláshoz kellenek: a matematikai pontnak nincs kiterjedése, az egyenesnek nincs vastagsága.

### Alapvető kapcsolatok

- Két különböző pont pontosan egy egyenest határoz meg.
- Ha több pont ugyanazon az egyenesen helyezkedik el, **kollineárisak**.
- Ha $B$ az $A$ és $C$ pont között van, akkor:

$$
|\overline{AC}|=|\overline{AB}|+|\overline{BC}|
$$

Ez a hosszúságok összeadásának geometriai megfelelője.

## 2. Egyenesek kölcsönös helyzete a síkban

Két különböző egyenes a síkban vagy metszi egymást, vagy párhuzamos.

### Metsző egyenesek

Pontosan egy közös pontjuk van: a **metszéspont**.

### Párhuzamos egyenesek

Ugyanabban a síkban vannak, és nincs közös pontjuk.

Jelölés:

$$
e\parallel f
$$

### Merőleges egyenesek

Olyan metsző egyenesek, amelyek derékszöget zárnak be.

Jelölés:

$$
e\perp f
$$

> [!tip] Merőlegesség felismerése
> Ha két metsző egyenes egyik szöge $90^\circ$, akkor mind a négy szögük $90^\circ$.

> [!warning] Sík és tér
> Térben két egyenes lehet **kitérő** is: nem metszik egymást, és nem párhuzamosak. Nem helyezkednek el közös síkban.

## 3. Távolság

### Két pont távolsága

A két pontot összekötő szakasz hossza.

### Pont és egyenes távolsága

A pontból az egyenesre állított **merőleges szakasz hossza**.

Miért a merőleges?

Mert ez a legrövidebb út a ponttól az egyenesig. Egy ferdén húzott szakasz hosszabb lenne.

### Párhuzamos egyenesek távolsága

A két egyenest összekötő merőleges szakasz hossza. Ez mindenhol ugyanakkora.

## 4. Mi a szög?

A szöget két, közös kezdőpontból induló félegyenes alkotja.

- A közös kezdőpont a **szög csúcsa**.
- A félegyenesek a **szög szárai**.
- A szög nagysága azt fejezi ki, mekkora elfordulás visz az egyik szártól a másikig a kijelölt irányban.

Jelölés:

$$
\angle ABC
$$

**A középső betű a csúcs**, tehát itt $B$. A szárak a $B$-ből $A$ és $C$ felé induló félegyenesek.

A szögeket gyakran görög betűkkel jelöljük:

$$
\alpha,\quad\beta,\quad\gamma
$$

> [!important] A szög nem a szárak hosszától függ
> Ha a rajzon hosszabbra húzod a szárakat, a szög nem változik.
>
> A szög az irányok közötti elfordulást méri, nem a vonalak hosszát.

## 5. Szögmérés fokban

Egy teljes fordulat:

$$
360^\circ
$$

Ebből:

| Elfordulás | Szög |
|---|---:|
| Negyed fordulat | $90^\circ$ |
| Fél fordulat | $180^\circ$ |
| Teljes fordulat | $360^\circ$ |

A fok kisebb egységei:

$$
1^\circ=60'
$$

$$
1'=60''
$$

A $'$ a **szögperc**, a $''$ a **szögmásodperc** jele. Ezek szögegységek, nem időegységek.

## 6. Szögtípusok

| Típus | Nagyság |
|---|---|
| Nullszög | $\alpha=0^\circ$ |
| Hegyesszög | $0^\circ<\alpha<90^\circ$ |
| Derékszög | $\alpha=90^\circ$ |
| Tompaszög | $90^\circ<\alpha<180^\circ$ |
| Egyenesszög | $\alpha=180^\circ$ |
| Homorúszög | $180^\circ<\alpha<360^\circ$ |
| Teljesszög | $\alpha=360^\circ$ |

## 7. Szögek összeadása és szögfelezés

Ha egy szög belsejében húzott félegyenes két kisebb szögre bontja az eredetit, akkor:

$$
\alpha=\beta+\gamma
$$

A **szögfelező** olyan félegyenes, amely a szög csúcsából indul, és két egyenlő szögre osztja a szöget:

$$
\beta=\gamma=\frac{\alpha}{2}
$$

> [!tip] Intuíció
> Az elfordulások összeadhatók: ha először $\beta$, majd ugyanabba az irányba további $\gamma$ szöggel fordulsz, a teljes elfordulás $\beta+\gamma$.

## 8. Pótszögek és kiegészítő szögek

### Pótszögek

Összegük $90^\circ$:

$$
\alpha+\beta=90^\circ
$$

### Kiegészítő szögek

Összegük $180^\circ$:

$$
\alpha+\beta=180^\circ
$$

Nem kell egymás mellett elhelyezkedniük: ezek az elnevezések a **nagyságuk összegét** írják le.

## 9. Mellékszögek

Két szög mellékszög, ha:

- közös a csúcsuk és az egyik száruk;
- a másik két száruk egy egyenes két ellentétes irányába mutat.

Együtt egyenesszöget alkotnak, ezért:

$$
\boxed{\alpha+\beta=180^\circ}
$$

Ha az egyik ismert, a másik:

$$
\beta=180^\circ-\alpha
$$

> [!important] Kapcsolat
> A mellékszögek mindig kiegészítő szögek. Két kiegészítő szög viszont nem feltétlenül mellékszög, mert lehetnek az ábra különböző helyein.

## 10. Csúcsszögek

Két metsző egyenesnél az egymással szemben fekvő szögek **csúcsszögek**.

Nagyságuk egyenlő:

$$
\boxed{\alpha=\gamma}
$$

### Miért egyenlők?

Mindkettő ugyanannak a szomszédos $\beta$ szögnek a kiegészítő szöge:

$$
\alpha+\beta=180^\circ
$$

$$
\gamma+\beta=180^\circ
$$

Ezért:

$$
\alpha=180^\circ-\beta=\gamma
$$

**Az egyenlőségük a mellékszögek kapcsolatából következik.**

## 11. Párhuzamos egyeneseket metsző egyenes

Ha két párhuzamos egyenest egy harmadik egyenes metsz, a két metszéspontban keletkező szögek között szabályos kapcsolatok vannak.

### Egyállású szögek

A két metszéspontnál **azonos relatív helyzetben** vannak: például mindkettő az adott párhuzamos fölött és a metsző egyenes jobb oldalán.

Párhuzamos egyenesek esetén:

$$
\boxed{\alpha=\beta}
$$

### Belső váltószögek

- A két párhuzamos közötti sávban vannak.
- A metsző egyenes ellentétes oldalain helyezkednek el.
- Különböző metszéspontokhoz tartoznak.

Párhuzamos egyenesek esetén:

$$
\boxed{\alpha=\beta}
$$

A külső váltószögek szintén egyenlők.

### Belső egyoldali szögek

- A két párhuzamos közötti sávban vannak.
- A metsző egyenes ugyanazon oldalán helyezkednek el.
- Különböző metszéspontokhoz tartoznak.

Összegük:

$$
\boxed{\alpha+\beta=180^\circ}
$$

> [!warning] A párhuzamosság feltétel
> Ezeket a szabályokat akkor használhatod, ha tudod vagy már bizonyítottad, hogy a két egyenes párhuzamos.
>
> Az, hogy a rajzon párhuzamosnak látszanak, önmagában nem bizonyíték.

A megfelelő szögkapcsolatok megfordítva a párhuzamosság bizonyítására is használhatók: például egyenlő egyállású szögekből következik a két egyenes párhuzamossága.

## 12. Szögek egy pont körül

Ha egy pont körüli teljes fordulatot átfedés és kihagyás nélkül szögekre bontunk, az összegük:

$$
\boxed{\alpha+\beta+\gamma+\dots=360^\circ}
$$

Ugyanígy egy egyenesszög felosztásakor:

$$
\alpha+\beta+\gamma+\dots=180^\circ
$$

## 13. Hogyan olvass egy geometriai ábrát?

1. **Keresd a megadott adatokat és jelöléseket.**
   A derékszöget kis négyzet, a párhuzamosságot gyakran egyforma nyilak, az egyenlő szakaszokat egyforma vonalkák jelölik.
2. **Ne feltételezz kapcsolatot pusztán a látvány alapján.**
   Egy ábra nem feltétlenül méretarányos.
3. **Nevezd meg a használható szabályt.**
   Például: „Ez a két szög csúcsszög, ezért egyenlő.”
4. **Írd fel az összefüggést, majd számolj.**
   A geometriai kapcsolatból lesz az egyenlet.

> [!important] A gondolkodás sorrendje
> **Geometriai kapcsolat → egyenlet → számítás.**
>
> Először azt kell felismerni, miért egyenlő két szög, vagy miért $180^\circ$ az összegük.

## Gyors áttekintés

| Kapcsolat | Következmény |
|---|---|
| Merőleges egyenesek | $90^\circ$-os szögek |
| Szögfelező | Két egyenlő részszög |
| Mellékszögek | Összegük $180^\circ$ |
| Csúcsszögek | Egyenlők |
| Egyállású szögek párhuzamosoknál | Egyenlők |
| Váltószögek párhuzamosoknál | Egyenlők |
| Belső egyoldali szögek párhuzamosoknál | Összegük $180^\circ$ |
| Egy pont körüli teljes szögfelosztás | Összegük $360^\circ$ |