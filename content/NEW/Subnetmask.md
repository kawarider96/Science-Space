---
tags:
  - hálózatok
  - IPv4
  - subnet
  - IT
  - informatika
aliases:
  - Alhálózati maszk
  - Subnet mask
---

# Subnet mask és alhálózat-számítás

## 1. Mire való a subnet mask?

Az IPv4-cím egy **32 bites szám**, amelyet négy, egyenként 8 bites részben írunk le:

```text
192.168.1.70
```

Ezeket a részeket **oktetteknek** nevezzük. Egy oktett értéke 0–255 lehet: ez összesen 256 különböző érték.

Az IP-cím két részből áll:

- **Hálózati rész:** melyik alhálózathoz tartozik a cím?
- **Hostrész:** azon belül melyik hálózati interfészt azonosítja?

**A subnet mask mutatja meg, hol van a kettő közötti határ.**

> [!important] Alapötlet
> Az IP-cím önmagában nem mondja meg az alhálózat méretét. Ehhez a maszk is kell.

## 2. A maszk és a CIDR-jelölés

A maszk binárisan összefüggő 1-esekből, majd 0-kból áll:

- **1:** hálózati bit.
- **0:** hostbit.

Például:

```text
Maszk:     255.255.255.192
Binárisan: 11111111.11111111.11111111.11000000
```

Ebben 26 darab 1-es szerepel, ezért a rövid jelölése:

```text
/26
```
Ezt hívjuk CIDR-nek.

Egy cím maszkkal együtt:

```text
192.168.1.70/26
```

A `/26` jelentése:

$$
\underbrace{26}_{\text{hálózati bit}}
+
\underbrace{6}_{\text{hostbit}}
=
32
$$

**A CIDR és a pontozott maszk ugyanazt az információt írja le.**

## 3. Hány cím fér el az alhálózatban?

Ha a prefix hossza $p$, a hostbitek száma:

$$
h=32-p
$$
Ha $p=26$ akkor $h=32-26=6$

Minden hostbit kétféle lehet: 0 vagy 1. Ezért az összes lehetséges cím:

$$
N=2^h=2^{32-p}
$$

Szokásos IPv4-alhálózatban két cím különleges:

- **Hálózati cím:** a használható címtartomány első címe
- **Broadcast cím:** a használható címtartomány utolsó címe

Ezért a kiosztható hostcímek száma:

$$
H=2^h-2
$$

Például `/26` esetén:

$$
h=32-26=6
$$

$$
N=2^6=64
$$

$$
H=64-2=\boxed{62}
$$

> [!warning] Kivételek
> A „mínusz kettő” szabály a szokásos, legfeljebb `/30` prefixű alhálózatokra vonatkozik. A `/31` pont–pont kapcsolaton két végpont címzésére használható; a `/32` egyetlen címet jelöl.

## 4. Mekkora alhálózat kell adott számú eszközhöz?

Olyan hostbitszámot keresünk, amelyre:

$$
2^h-2\geq H_{\text{szükséges}}
$$

**A legkisebb megfelelő $h$ adja a legkisebb elegendő alhálózatot.**

Ha 62 kiosztható cím kell:

$$
2^5-2=30 \qquad \text{kevés}
$$

$$
2^6-2=62 \qquad \text{elegendő}
$$

Tehát:

$$
h=6
$$

$$
p=32-6=\boxed{26}
$$

A szükséges maszk:

```text
/26 = 255.255.255.192
```

### Logaritmussal

Ugyanez közvetlenül:

$$
h=\left\lceil\log_2(H_{\text{szükséges}}+2)\right\rceil
$$

A $\lceil \ \rceil$ **felfelé kerekítést** jelent.

Ha a számológépen csak `log` van:

$$
\log_2(x)=\frac{\log(x)}{\log(2)}
$$

> [!tip] Miért kell hozzáadni kettőt?
> A szükséges hostcímeken felül a hálózati és a broadcast címnek is el kell férnie.
>
> A szükséges címekbe az átjáró IP-címét és a többi címzett hálózati eszközt is számold bele.

## 5. CIDR átalakítása pontozott maszkká

Egy oktett bitjeinek értékei:

| Bit helye | 1. | 2. | 3. | 4. | 5. | 6. | 7. | 8. |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Érték | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

A maszkban balról kezdve kapcsoljuk be a biteket:

| Hálózati bitek az oktettben | Binárisan  | Decimálisan |
| :-------------------------: | ---------- | :---------: |
|              0              | `00000000` |      0      |
|              1              | `10000000` |     128     |
|              2              | `11000000` |     192     |
|              3              | `11100000` |     224     |
|              4              | `11110000` |     240     |
|              5              | `11111000` |     248     |
|              6              | `11111100` |     252     |
|              7              | `11111110` |     254     |
|              8              | `11111111` |     255     |

A `/26` esetén:

- Az első 24 bit három teljes oktett: `255.255.255`.
- A maradék 2 hálózati bit: $128+64=192$.

Így:

$$
/26 \longrightarrow 255.255.255.192
$$

## 6. Gyakori maszkok

| CIDR | Subnet mask | Összes cím | Kiosztható hostcím |
|---|---|---:|---:|
| /16 | 255.255.0.0 | 65 536 | 65 534 |
| /23 | 255.255.254.0 | 512 | 510 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

> [!important] Intuíció
> Ha a prefixet eggyel növeled, egy hostbitet hálózati bitté alakítasz. Emiatt az alhálózat összes címeinek száma megfeleződik.
>
> **Nagyobb prefix → kisebb alhálózat.**

## 7. Hálózati cím és címtartomány kiszámítása

Nézzük ezt a címet:

```text
192.168.1.70/26
```

A maszk:

```text
255.255.255.192
```

### 1. Blokkméret

A határt tartalmazó oktettben:

$$
B=256-\text{maszkoktett}
$$

Itt:

$$
B=256-192=64
$$

Ezért a hálózatok az utolsó oktettben 64-es lépésekben kezdődnek:

```text
0, 64, 128, 192
```

A tartományok:

```text
0–63
64–127
128–191
192–255
```

### 2. Melyik blokkban van a cím?

A 70 a **64–127** tartományba esik.

Ezért:

| Tulajdonság | Cím |
|---|---|
| Hálózati cím | 192.168.1.64 |
| Első kiosztható cím | 192.168.1.65 |
| Utolsó kiosztható cím | 192.168.1.126 |
| Broadcast cím | 192.168.1.127 |
| Következő hálózat kezdete | 192.168.1.128 |

> [!tip] Gyors módszer
> Keresd meg az IP-címhez tartozó blokk kezdetét.
>
> - Hálózati cím: a blokk első címe.
> - Broadcast: a következő blokk kezdete mínusz 1.
> - Hosttartomány: a hálózati cím utáni címtől a broadcast előtti címig.

**A blokkmódszer nem mindig az utolsó oktettben működik.** Például `/23` esetén a harmadik oktettben számolunk: $256-254=2$, tehát ott a blokkok kezdete 0, 2, 4, 6…

## 8. Mi történik valójában? Bitenkénti ÉS

A hálózati cím pontos számítása:

$$
\text{hálózati cím}=\text{IP-cím AND subnet mask}
$$

Az `AND` eredménye csak akkor 1, ha mindkét bemeneti bit 1.

A példánk utolsó oktettje:

```text
IP:       01000110   = 70
Maszk:    11000000   = 192
          --------
AND:      01000000   = 64
```

**A maszk megőrzi a hálózati biteket, és lenullázza a hostbiteket.**

Két cím akkor tartozik ugyanahhoz az alhálózathoz egy adott maszk szerint, ha az `AND` művelet ugyanazt a hálózati címet adja.

## 9. Több alhálózat létrehozása

Ha egy `/24` hálózatot `/26` hálózatokra osztunk, két további bitet használunk a hálózatok megkülönböztetésére:

$$
26-24=2
$$

A létrejövő alhálózatok száma:

$$
2^2=4
$$

Például a `192.168.1.0/24` felosztása:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Mindegyikben 62 kiosztható hostcím van.

**Több alhálózat használhatja ugyanazt a maszkot.** Az alhálózatok számából nem következik, hogy ugyanannyi különböző maszk kell.

Eltérő méretű alhálózatok kiosztását **VLSM-nek** nevezzük. Ilyenkor általában a legnagyobb címigényű alhálózattal kezdünk; minden hálózatnak megfelelő blokkhatáron kell kezdődnie, átfedés nélkül.

## 10. Gyakori félreértések

- **A `/26` nem 26 eszközt jelent:** 26 hálózati bitet jelent.
- **A 0–255 tartomány 256 érték:** a nullát is számoljuk.
- **A hálózati cím nem feltétlenül `.0`:** lehet például `.64`.
- **A broadcast nem feltétlenül `.255`:** lehet például `.127`.
- **Az átjáró nem kötelezően az első hostcím:** ez beállítási szokás.
- **A subnet és a VLAN külön fogalom:** a subnet IP-szintű felosztás, a VLAN Ethernet-szintű elkülönítés. Gyakran egy VLANhoz egy subnetet rendelnek.
