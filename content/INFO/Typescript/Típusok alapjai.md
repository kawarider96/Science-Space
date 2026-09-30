## 1️⃣ Mi ez? (egy mondatban)

Egy **típus** nem adat, hanem egy **ígéret / szerződés** arról, hogy _milyen alakú és viselkedésű adat_ mozoghat a kódban.

> [!important]  
> **A típus nem azt mondja meg, mi VAN.**  
> **Azt mondja meg, mi LEHET.**

---

## 2️⃣ Miért kell erről külön jegyzet?

A legtöbb félreértés innen jön:

> „A változóban most ez az érték van, akkor ez a típusa.”

❌ Nem.

> [!warning]  
> A TypeScript **nem az aktuális értéket**, hanem a **megengedett tartományt** írja le.

Ha ezt nem érted meg:

- a TypeScript idegesítő lesz
- elkezdesz `any`-zni
- és visszacsúszol JS-be

---

## 3️⃣ Mentális modell: típus = szerződés

Képzeld el úgy, mintha minden függvény és objektum egy **katonai parancs** lenne:

> „ÉN EZT VÁROM, ÉS EZT ADOM VISSZA.”

Példa:

```ts
function square(x: number): number {
  return x * x;
}
```

> [!info]  
> Ez nem azt jelenti, hogy `x` most éppen 4.  
> Azt jelenti, hogy **bármikor, amikor meghívod**, csak szám lehet.

---

## 4️⃣ Típus vs érték (kritikus különbség)

### Érték (runtime)

```ts
let x = 5;
```

- futás közben létezik
- változhat
- számolunk vele

### Típus (compile time)

```ts
let x: number = 5;
```

- csak fordításkor létezik
- nem fut le
- szabályokat ír le

> [!important]  
> **Runtime-ban NINCS típusinformáció.**

---

## 5️⃣ Mi történik, ha megszeged a szerződést?

```ts
let age: number;

age = 25;      // ✅ oké
age = "huszon"; // ❌ nem oké
```

> [!success]  
> A TypeScript **megállít**, mielőtt hülyeséget csinálnál.

Ez nem szigor.  
Ez **védelem**.

---

## 6️⃣ Miért nem elég a "józan ész"?

> [!warning]  
> „Én tudom, hogy ide szám jön.” – minden fejlesztő, minden bug előtt.

A probléma:

- más is nyúl a kódhoz
- te is elfelejted 3 hét múlva
- refaktoráláskor szétesik

A típus:

- **nem felejt**
- **nem ért félre**
- **nem alkuszik**

---

## 7️⃣ Típus mint dokumentáció

```ts
type LoginPayload = {
  username: string;
  password: string;
};
```

> [!info]  
> Ez jobb dokumentáció, mint egy komment:
> 
> - mindig friss
>     
> - nem hazudik
>     
> - a fordító ellenőrzi
>     

---

## 8️⃣ Gyakori félreértések

> [!danger]  
> ❌ „A típus az adat”  
> ❌ „A TypeScript futás közben is véd”  
> ❌ „Ha egyszer jó volt, mindig jó lesz”

> [!success]  
> ✅ A típus **korlátokat állít fel**  
> ✅ A típus **gondolkodásra kényszerít**

---

## 9️⃣ Egy nagyon fontos mondat

> [!quote]  
> **A TypeScript nem azért létezik, hogy kevesebbet tudj, hanem hogy kevesebbet kelljen fejben tartanod.**

---

## 10️⃣ Előkészítés a következő jegyzethez

A következő logikus kérdés:

> „Oké, de milyen típusok vannak, és mikor melyiket használjam?”

Ez jön:  
👉 **Alaptípusok – `string`, `number`, `boolean`, `any`, `unknown`**

---

🧠 Gondolkodj el rajta:

- Hol kezeled ma fejben a szabályokat típus helyett?
- Mely függvényed lenne tisztább egy explicit szerződéssel?
- Hol mondtad már azt: „ezt most csak én használom”?