---
tags:
  - matematika
  - geometria
  - kör
aliases:
  - A kör alapjai
  - Kör és körcikk
---
## 1. Mi a kör?

A **körvonal** a sík azon pontjainak halmaza, amelyek egy adott ponttól ugyanakkora távolságra vannak.

- Az adott pont a **középpont**, jele általában $O$.
- Az állandó távolság a **sugár**, jele $r$.

Ha $P$ a körvonal egyik pontja:

$$
OP=r
$$

A **körlap** a körvonalból és a belsejéből áll. Pontjainak távolsága a középponttól legfeljebb $r$.

> [!important] Körvonal és körlap
> A **kerület** a körvonal hosszát méri.
>
> A **terület** a körlap méretét méri.
>
> A „kör” szót gyakran mindkettőre használjuk; a szövegkörnyezet dönti el, melyikről van szó.

## 2. Sugár és átmérő

### Sugár

A középpontot a körvonal valamely pontjával összekötő szakasz, illetve ennek hossza.

Egy adott kör minden sugara ugyanolyan hosszú.

### Átmérő

A kör középpontján átmenő szakasz, amelynek mindkét végpontja a körvonalon van.

Két sugárból áll:

$$
\boxed{d=2r}
$$

Ezért:

$$
\boxed{r=\frac{d}{2}}
$$

> [!warning] Sugár vagy átmérő?
> A képletekbe azt a mennyiséget helyettesítsd, amelyet a képlet kér.
>
> Ha az átmérőt sugárként használod a területképletben, négyszer akkora eredményt kapsz.

## 3. Húr és körív

### Húr

Két körvonali pontot összekötő szakasz.

- Az átmérő is húr.
- Az átmérő a kör leghosszabb húrja.
- A többi húr rövidebb az átmérőnél.

### Körív

A körvonal két pont közötti része.

Két különböző pont a körvonalat két ívre osztja. Ezért meg kell határozni, melyik ívről beszélünk.

> [!tip] Húr és ív
> A **húr egyenes szakasz**, az **ív a körvonal mentén halad**.
>
> Ugyanazon két különböző végpont között az ív hosszabb a húrnál.

### Fontos húrkapcsolatok

Ugyanabban a körben:

- Egyenlő hosszúságú húrok egyenlő hosszúságú kisebb íveket határoznak meg.
- A középpontból egy húrra állított merőleges felezi a húrt.
- A középponthoz közelebb fekvő húr hosszabb.

## 4. Egyenes és kör kölcsönös helyzete

| Egyenes típusa | Közös pontok száma a körvonallal |
|---|---:|
| Külső egyenes | 0 |
| Érintő | 1 |
| Metsző egyenes | 2 |

### Érintő

Az érintő egyetlen pontban találkozik a körvonallal. Ez az **érintési pont**.

Az érintési pontba húzott sugár merőleges az érintőre:

$$
\boxed{OP\perp e}
$$

ahol $P$ az érintési pont, $e$ az érintő.

Egy körön kívüli pontból húzott két érintőszakasz egyenlő hosszúságú.

> [!important] Mit jelent az érintő iránya?
> Az érintő a körvonal helyi irányát mutatja az érintési pontban.
>
> Körmozgásnál a pillanatnyi sebesség érintőirányú, ezért merőleges az adott pontba mutató sugárra.

## 5. Mi a π?

A kör kerületének és átmérőjének aránya minden körnél ugyanaz:

$$
\boxed{\pi=\frac{K}{d}}
$$

Közelítő értéke:

$$
\pi\approx3{,}14159
$$

A $\pi$ dimenzió nélküli szám: két hosszúság hányadosa.

> [!tip] Intuíció
> Ha egy kört kétszeresére nagyítasz, az átmérője és a kerülete is kétszeres lesz.
>
> Az arányuk nem változik. Ez az állandó arány a $\pi$.

## 6. A kör kerülete

Mivel $K/d=\pi$:

$$
\boxed{K=\pi d}
$$

A $d=2r$ kapcsolatot behelyettesítve:

$$
\boxed{K=2\pi r}
$$

A kerület hosszúság, ezért mértékegysége például:

$$
\text{cm},\quad\text{m}
$$

### A sugár kifejezése

Ha a kerület ismert:

$$
K=2\pi r
$$

Mindkét oldalt $2\pi$-vel osztjuk:

$$
\boxed{r=\frac{K}{2\pi}}
$$

## 7. A kör területe

$$
\boxed{T=\pi r^2}
$$

Átmérővel kifejezve:

$$
T=\pi\left(\frac{d}{2}\right)^2
$$

$$
\boxed{T=\frac{\pi d^2}{4}}
$$

A terület mértékegysége például:

$$
\text{cm}^2,\quad\text{m}^2
$$

### Miért szerepel benne a sugár négyzete?

A terület két térbeli irányban mért kiterjedést fejez ki.

Ha minden hosszúságot kétszeresére növelünk, a terület:

$$
2^2=4
$$

szeresére nő.

Általánosan, ha a sugár $k$-szorosára változik:

$$
K_{\text{új}}=kK
$$

$$
T_{\text{új}}=k^2T
$$

### Honnan érthető meg a területképlet?

Képzeld el, hogy a körlapot sok keskeny körcikkre vágod, és felváltva egymás mellé rendezed őket.

Egy egyre inkább téglalaphoz hasonló alakzatot kapsz, amelynek:

- magassága közel $r$;
- alapja közel a kerület fele, vagyis $\pi r$.

Egyre finomabb felosztásnál a terület:

$$
T=(\pi r)\cdot r=\pi r^2
$$

## 8. Középponti szög

A középponti szög csúcsa a kör középpontja, szárai sugarak.

A szög kijelöl egy körívet és egy körcikket.

A teljes körhöz:

$$
360^\circ
$$

tartozik.

Ezért az $\alpha$ középponti szöghöz tartozó rész a teljes kör:

$$
\frac{\alpha}{360^\circ}
$$

része.

> [!tip] Arányos gondolkodás
> Ugyanabban a körben kétszer akkora középponti szöghöz kétszer akkora ívhossz és körcikkterület tartozik.

## 9. Ívhossz

Ha $\alpha$ fokban adott, az ívhossz:

$$
\boxed{s=\frac{\alpha}{360^\circ}\cdot2\pi r}
$$

### Miért?

A teljes körvonal hossza $2\pi r$.

Ha a szög a teljes fordulat negyede, akkor az ív is a kerület negyede. Ugyanez az arányosság bármely középponti szögnél működik.

## 10. Körcikk és körszelet

### Körcikk

A körlap két sugár és a hozzájuk tartozó körív által határolt része.

A területe, ha $\alpha$ fokban adott:

$$
\boxed{T_{\text{körcikk}}
=\frac{\alpha}{360^\circ}\cdot\pi r^2}
$$

### Körszelet

A körlap egy **húr és a hozzá tartozó körív** által határolt része.

> [!warning] Nem ugyanaz
> **Körcikk:** két sugár és egy ív határolja.
>
> **Körszelet:** egy húr és egy ív határolja.
>
> A körcikk területképlete nem alkalmazható közvetlenül a körszeletre.

## 11. Radián: szögmérés ívhosszal

A radián a szög olyan mértékegysége, amelyet az ívhossz és a sugár arányával definiálunk:

$$
\boxed{\theta=\frac{s}{r}}
$$

**Egy radián** az a középponti szög, amelyhez a sugárral azonos hosszúságú ív tartozik:

$$
s=r\quad\Rightarrow\quad\theta=1\text{ rad}
$$

Teljes körnél $s=2\pi r$, ezért:

$$
\theta=\frac{2\pi r}{r}=2\pi
$$

Tehát:

$$
\boxed{360^\circ=2\pi\text{ rad}}
$$

$$
\boxed{180^\circ=\pi\text{ rad}}
$$

### Gyakori átváltások

| Fok | Radián |
|---:|---:|
| $30^\circ$ | $\pi/6$ |
| $45^\circ$ | $\pi/4$ |
| $60^\circ$ | $\pi/3$ |
| $90^\circ$ | $\pi/2$ |
| $180^\circ$ | $\pi$ |
| $360^\circ$ | $2\pi$ |

### Fokból radiánba

$$
\boxed{\theta_{\text{rad}}
=\frac{\alpha_{\text{fok}}}{180}\pi}
$$

### Radiánból fokba

$$
\boxed{\alpha_{\text{fok}}
=\frac{\theta_{\text{rad}}}{\pi}\cdot180}
$$

> [!important] Miért hasznos a radián?
> Közvetlenül összekapcsolja a szöget és a megtett ívhosszt.
>
> Radiánban az ívhossz képlete egyszerűen:
>
> $$
> \boxed{s=r\theta}
> $$
>
> A körcikk területe pedig:
>
> $$
> \boxed{T_{\text{körcikk}}=\frac12r^2\theta}
> $$
>
> Ezekbe a képletekbe a szöget **radiánban** kell behelyettesíteni.

## 12. Kerületi szög

A kerületi szög:

- csúcsa a körvonalon van;
- szárai a körvonalat további két pontban metszik.

A szög ahhoz az ívhez tartozik, amely a szárak végpontjai között van, és **nem tartalmazza a szög csúcsát**.

Az ugyanahhoz az ívhez tartozó kerületi szög a középponti szög fele:

$$
\boxed{\beta=\frac{\alpha}{2}}
$$

Ezért az ugyanahhoz az ívhez tartozó kerületi szögek egyenlők.

> [!warning] Melyik ív?
> Két körvonali pont két ívet határoz meg.
>
> Mindig ellenőrizd, hogy a középponti és a kerületi szög valóban ugyanahhoz az ívhez tartozik-e.

## 13. Thalész tétele

Ha egy kör átmérőjének két végpontját összekötjük a körvonal egy harmadik pontjával, derékszögű háromszöget kapunk.

**Az átmérővel szemközti szög $90^\circ$.**

### Miért?

Az átmérőhöz tartozó félkör középponti szöge:

$$
180^\circ
$$

Az ugyanahhoz az ívhez tartozó kerületi szög ennek fele:

$$
\boxed{\frac{180^\circ}{2}=90^\circ}
$$

A megfordítás is igaz: egy derékszögű háromszög derékszögű csúcsa az átfogó mint átmérő fölé rajzolt körön helyezkedik el.

## 14. Egy kidolgozott példa

Egy kör sugara $6\text{ cm}$. A kijelölt körcikk középponti szöge $60^\circ$.

### A teljes kerület

$$
K=2\pi r=12\pi\text{ cm}
$$

### A kijelölt ív hossza

A $60^\circ$ a teljes fordulat hatoda:

$$
s=\frac{60^\circ}{360^\circ}\cdot12\pi
$$

$$
\boxed{s=2\pi\text{ cm}}
$$

### A körcikk területe

$$
T_{\text{körcikk}}
=\frac{60^\circ}{360^\circ}\cdot\pi\cdot6^2
$$

$$
\boxed{T_{\text{körcikk}}=6\pi\text{ cm}^2}
$$

### Ugyanez radiánnal

$$
60^\circ=\frac{\pi}{3}\text{ rad}
$$

$$
s=r\theta=6\cdot\frac{\pi}{3}=2\pi\text{ cm}
$$

**Ugyanazt a geometriai kapcsolatot kétféle szögmértékkel számoltuk ki.**

## Gyors képletáttekintés

| Mennyiség | Képlet |
|---|---|
| Átmérő | $d=2r$ |
| Kerület | $K=2\pi r=\pi d$ |
| Körlap területe | $T=\pi r^2$ |
| Ívhossz, fokban | $s=\frac{\alpha}{360^\circ}\,2\pi r$ |
| Körcikk területe, fokban | $T_{\text{körcikk}}=\frac{\alpha}{360^\circ}\,\pi r^2$ |
| Szög, radiánban | $\theta=s/r$ |
| Ívhossz, radiánban | $s=r\theta$ |
| Körcikk területe, radiánban | $T_{\text{körcikk}}=\frac12r^2\theta$ |

> [!tip] Ellenőrzés számolás után
> - Sugárral vagy átmérővel számoltál?
> - A szög fokban vagy radiánban van?
> - Hosszúságot vagy területet keresel?
> - A kiválasztott ív hossza legfeljebb a teljes kerület?
> - A körcikk területe legfeljebb a teljes körlap területe?