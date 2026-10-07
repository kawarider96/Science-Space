---
tags:
  - matematika
  - geometria
  - háromszögek
aliases:
  - Háromszögek alapjai
---
## 1. Mi a háromszög?

Három, nem egy egyenesre eső pontot szakaszokkal összekötve háromszöget kapunk.

A háromszögnek van:

- **3 csúcsa**
- **3 oldala**
- **3 belső szöge**

A csúcsokat általában $A$, $B$, $C$ betűkkel jelöljük.

**Az oldalt annak a csúcsnak a kisbetűjével jelöljük, amellyel szemben van.**

| Csúcs | Belső szög | Szemközti oldal   |
| ----- | ---------- | ----------------- |
| $A$   | $\alpha$   | $a=\overline{BC}$ |
| $B$   | $\beta$    | $b=\overline{AC}$ |
| $C$   | $\gamma$   | $c=\overline{AB}$ |

> [!tip] Jelölések értelmezése
> Az $a$ oldal az $A$ csúccsal szemben van. Az $\alpha$ szög az $A$ csúcsnál található.
>
> Így az $a$ oldal és az $\alpha$ szög egymással szemben helyezkedik el.

## 2. A belső szögek összege

A síkbeli, euklideszi háromszög belső szögeinek összege:

$$
\boxed{\alpha+\beta+\gamma=180^\circ}
$$

Ha két szöget ismerünk, a harmadik:

$$
\gamma=180^\circ-\alpha-\beta
$$

### Miért éppen 180°?

Húzzunk az egyik csúcson keresztül a szemközti oldallal párhuzamos egyenest.

A másik két belső szöget a váltószögek egyenlősége miatt „megtaláljuk” ennél a csúcsnál is. A három szög itt együtt egyenesszöget alkot:

$$
180^\circ
$$

> [!important] Kapcsolat az előző jegyzettel
> A háromszög szögösszege a párhuzamos egyenesek szögkapcsolataiból következik.

## 3. Külső szög

Ha az egyik oldalt meghosszabbítjuk egy csúcson túl, az ottani belső szög mellett **külső szög** keletkezik.

A belső szög és ez a külső szög mellékszögek:

$$
\gamma+\gamma_{\text{külső}}=180^\circ
$$

Ezért:

$$
\gamma_{\text{külső}}=180^\circ-\gamma
$$

Mivel:

$$
\alpha+\beta+\gamma=180^\circ
$$

következik, hogy:

$$
\boxed{\gamma_{\text{külső}}=\alpha+\beta}
$$

**A külső szög egyenlő a két nem mellette fekvő belső szög összegével.**

## 4. Háromszögek csoportosítása oldalak szerint

### Különböző oldalú háromszög

Mindhárom oldal hossza különböző.

Mindhárom belső szöge is különböző.

### Egyenlő szárú háromszög

Két oldala egyenlő hosszúságú. Ezek a **szárak**, a harmadik oldal az **alap**.

Az alapon fekvő szögek egyenlők.

Ha:

$$
b=c
$$

akkor:

$$
\beta=\gamma
$$

A megfordítás is igaz: két egyenlő szöggel szemben egyenlő hosszúságú oldalak vannak.

### Egyenlő oldalú háromszög

Mindhárom oldala egyenlő:

$$
a=b=c
$$

Mindhárom szöge egyenlő, ezért:

$$
\alpha=\beta=\gamma=\frac{180^\circ}{3}
$$

$$
\boxed{\alpha=\beta=\gamma=60^\circ}
$$

> [!note] Fogalmi kapcsolat
> Az egyenlő oldalú háromszög az egyenlő szárú háromszög speciális esete: annak is van két egyenlő oldala.

## 5. Háromszögek csoportosítása szögek szerint

| Típus | Tulajdonság |
|---|---|
| **Hegyesszögű** | Mindhárom belső szöge kisebb $90^\circ$-nál |
| **Derékszögű** | Egy belső szöge pontosan $90^\circ$ |
| **Tompaszögű** | Egy belső szöge nagyobb $90^\circ$-nál |

Egy háromszögnek legfeljebb egy derék- vagy tompaszöge lehet, mert a belső szögek összege $180^\circ$.

### A derékszögű háromszög oldalai

- A derékszöget közrefogó két oldal a **befogó**.
- A derékszöggel szemközti oldal az **átfogó**.
- Az átfogó a háromszög leghosszabb oldala.

> [!important] Kétféle besorolás egyszerre
> Az oldalak és a szögek szerinti csoportosítás külön szempont.
>
> Egy háromszög lehet például **egyenlő szárú és derékszögű** egyszerre. Ekkor a szögei $45^\circ$, $45^\circ$, $90^\circ$.

## 6. Az oldalak és a szögek kapcsolata

Egy háromszögben:

- **Nagyobb oldallal szemben nagyobb szög van.**
- **Egyenlő oldalakkal szemben egyenlő szögek vannak.**

Például:

$$
a>b \quad\Longleftrightarrow\quad \alpha>\beta
$$

> [!tip] Intuíció
> Gondolj két, egy csúcsból induló szárra, amelyek hosszát rögzíted. Ha jobban szétnyitod őket, a végpontjaik közötti szemközti oldal hosszabb lesz.

Ezzel számolás nélkül is ellenőrizheted az ábrát: a legnagyobb szögnek a leghosszabb oldallal szemben kell lennie.

## 7. Háromszög-egyenlőtlenség

Három pozitív hosszúságból csak akkor készíthető háromszög, ha **bármely két oldal összege nagyobb a harmadiknál**:

$$
a+b>c
$$

$$
a+c>b
$$

$$
b+c>a
$$

### Miért?

Két oldalnak „körbe kell érnie” a harmadik oldal két végpontja között.

Ha a két oldal összege:

- **kisebb a harmadiknál:** nem érnek össze;
- **egyenlő a harmadikkal:** egy egyenesre lapulnak;
- **nagyobb a harmadiknál:** létrejöhet valódi háromszög.

> [!tip] Gyors ellenőrzés
> Pozitív oldalhosszak esetén elegendő ellenőrizni, hogy a két rövidebb oldal összege nagyobb-e a leghosszabbnál.

Ha két oldal $a$ és $b$ ismert, a harmadik oldal lehetséges tartománya:

$$
\boxed{|a-b|<c<a+b}
$$

## 8. Magasságvonal és magasság

Egy csúcsból a szemközti oldal **egyenesére merőleges** egyenest húzunk. Ez a csúcshoz tartozó **magasságvonal**.

A csúcs és a merőleges talppontja közötti szakasz hossza a **magasság**.

Az $a$ oldalhoz tartozó magasság jelölése:

$$
m_a
$$

### Hol található a magasság?

- Hegyesszögű háromszögnél mindhárom magasságszakasz belül van.
- Derékszögű háromszögnél a két befogó egyben magasságszakasz is.
- Tompaszögű háromszögnél két magasság talppontja az oldalak meghosszabbítására esik.

Ezért fontos, hogy a magasságot az oldal **egyenesére**, ne feltétlenül az oldalszakaszra állítsuk.

A három magasságvonal egy pontban találkozik: ez a **magasságpont**.

## 9. Súlyvonal

A súlyvonal egy csúcsot köt össze a szemközti oldal **felezőpontjával**.

Minden háromszögnek három súlyvonala van.

Ezek egy pontban találkoznak: a **súlypontban**, amelyet gyakran $G$-vel jelölünk.

A súlypont minden súlyvonalat **2:1 arányban oszt**, a csúcstól mérve.

Ha $F$ a szemközti oldal felezőpontja:

$$
\boxed{AG:GF=2:1}
$$

Tehát:

$$
AG=\frac{2}{3}AF
$$

$$
GF=\frac{1}{3}AF
$$

> [!tip] Fizikai jelentés
> Egy egyenletes vastagságú és sűrűségű háromszöglap tömegközéppontja a geometriai súlypontban van. Itt alátámasztva egyensúlyozható.

## 10. Belső szögfelező

A belső szögfelező egy csúcs belső szögét két egyenlő szögre osztja.

Ha az $A$ csúcs szöge $\alpha$, a szögfelező két oldalán:

$$
\frac{\alpha}{2}
$$

nagyságú szögek vannak.

A három belső szögfelező egy pontban találkozik. Ez a **beírt kör középpontja**.

Ez a pont mindhárom oldalegyenestől azonos távolságra van, ezért kör rajzolható köré, amely mindhárom oldalt érinti.

> [!warning] Mit felez?
> A szögfelező a **szöget** felezi. Általában nem felezi a szemközti oldalt.

## 11. Oldalfelező merőleges

Egy oldal oldalfelező merőlegese:

- átmegy az oldal felezőpontján;
- merőleges az oldalra.

**Minden pontja egyenlő távol van az oldal két végpontjától.**

A három oldalfelező merőleges egy pontban találkozik. Ez a **körülírt kör középpontja**.

Ez a pont mindhárom csúcstól azonos távolságra van, ezért kör rajzolható köré, amely mindhárom csúcson átmegy.

> [!important] Két különböző kör
> **Beírt kör:** mindhárom oldalt érinti.
>
> **Körülírt kör:** mindhárom csúcson átmegy.

## 12. A nevezetes vonalak összehasonlítása

| Vonal | Mi határozza meg? | Mit garantál? | Közös metszéspont |
|---|---|---|---|
| **Magasságvonal** | Csúcsból merőleges a szemközti oldal egyenesére | Merőlegesség | Magasságpont |
| **Súlyvonal** | Csúcs és a szemközti oldal felezőpontja | Az oldalt felezi | Súlypont |
| **Belső szögfelező** | Csúcsból a belső szöget felezi | Két egyenlő szög | Beírt kör középpontja |
| **Oldalfelező merőleges** | Oldalfelező ponton át, merőlegesen | Egyenlő távolság a végpontoktól | Körülírt kör középpontja |

> [!warning] Ne keverd össze!
> A súlyvonal általában nem merőleges az oldalra.
>
> A magasságvonal általában nem felezi az oldalt.
>
> Az oldalfelező merőleges általában nem megy át a szemközti csúcson.

### Mikor esnek egybe?

Egyenlő szárú háromszögben a szárak közötti csúcsból az alaphoz húzott:

- magasságvonal,
- súlyvonal,
- belső szögfelező

és az alap oldalfelező merőlegese **ugyanarra az egyenesre esik**.

Egyenlő oldalú háromszögben ez mindhárom csúcsnál teljesül, és a négy nevezetes középpont is egybeesik.

## 13. Kerület és terület

### Kerület

A határoló oldalak hosszának összege:

$$
\boxed{K=a+b+c}
$$

### Terület

Egy oldal és a hozzá tartozó magasság segítségével:

$$
\boxed{T=\frac{a\,m_a}{2}}
$$

Bármelyik oldalt választhatjuk alapnak:

$$
T=\frac{a\,m_a}{2}
=\frac{b\,m_b}{2}
=\frac{c\,m_c}{2}
$$

**Mindig a kiválasztott oldalhoz tartozó, rá merőleges magasságot kell használni.**

### Miért osztunk kettővel?

Két egybevágó háromszögből olyan paralelogramma állítható össze, amelynek alapja $a$, magassága $m_a$.

A paralelogramma területe $a\,m_a$, így egy háromszög ennek a fele.

> [!tip] Mértékegység-ellenőrzés
> Kerület: például $\text{cm}$.
>
> Terület: például $\text{cm}^2$, mert két hosszúságot szorzunk össze.
