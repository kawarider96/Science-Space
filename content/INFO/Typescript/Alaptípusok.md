## 1️⃣ Mi ez?

Az **alaptípusok** a TypeScript legkisebb építőkövei.  
Ezekből épül fel **minden összetettebb típus**, ezért ha itt félreértesz valamit, az **később megsokszorozódik**.

> [!important]  
> Nem az a kérdés, _mit lehet leírni_, hanem az, _mit szabad_.

---

## 2️⃣ `string`

### Mi ez?

Szöveges adat.

```ts
let name: string = "Krisz";
```

> [!info]  
> `string` **nem** azt jelenti, hogy „bármi, ami idézőjelben van”.  
> Hanem azt, hogy **szövegként kezelhető**.

### Tipikus hiba

```ts
let ageText: string = 27;
// ❌ number nem string
```

---

## 3️⃣ `number`

### Mi ez?

Minden szám: egész, tört, negatív.

```ts
let age: number = 27;
let price: number = 19.99;
```

> [!warning]  
> A TypeScriptben **nincs külön int / float**.

### Mentális modell

> Szám = amivel matematikát akarsz csinálni.

---

## 4️⃣ `boolean`

### Mi ez?

Logikai érték: igaz vagy hamis.

```ts
let isAdmin: boolean = true;
```

> [!important]  
> `boolean` ≠ truthy / falsy

```ts
let x: boolean = "igen"; // ❌
```

---

## 5️⃣ `null` és `undefined`

Ez az egyik **legfontosabb és leggyakrabban elrontott rész**.

### `undefined`

> [!info]  
> Azt jelenti: **nincs érték hozzárendelve**.

```ts
let value: number;
// value === undefined
```

### `null`

> [!info]  
> Azt jelenti: **szándékosan üres**.

```ts
let selectedUser: User | null = null;
```

### Kritikus különbség

> [!important]  
> `undefined` = nincs még  
> `null` = volt / lehetne, de most nincs

---

## 6️⃣ Miért veszélyes az `any`?

### Mi az `any`?

```ts
let data: any;
```

> [!danger]  
> `any` azt mondja a TypeScriptnek:  
> **"Ne gondolkodj, majd én tudom."**

### Mit enged meg?

```ts
data.foo.bar.baz(); // ✅ fordul
```

> [!danger]  
> Ez **pont olyan**, mintha sima JavaScriptet írnál.

### Mikor szokás használni (rosszul)?

- gyors hack
- időhiány
- „majd később javítom” (spoiler: nem)

---

## 7️⃣ Miért jobb az `unknown`?

### Mi az `unknown`?

```ts
let data: unknown;
```

> [!success]  
> `unknown` = **nem tudom mi ez, ezért óvatos vagyok**

### Mit csinál másképp?

```ts
data.foo; // ❌ nem engedi
```

Csak ellenőrzés után:

```ts
if (typeof data === "string") {
  data.toUpperCase(); // ✅
}
```

> [!important]  
> `unknown` **kényszerít a gondolkodásra**.

---

## 8️⃣ `any` vs `unknown` – fejlesztői döntés

> [!warning]  
> Ha `any`-t használsz, a TypeScript **kiszáll a beszélgetésből**.

> [!success]  
> Ha `unknown`-t használsz, a TypeScript **partner marad**.

**Ökölszabály:**

> [!tip]  
> Ha nem tudod, mi az adat → `unknown`  
> Ha tudod → pontos típus

---

## 9️⃣ Gyakori félreértések

> [!danger]  
> ❌ `any` = gyorsabb fejlesztés  
> ❌ `null` és `undefined` ugyanaz  
> ❌ `boolean` = truthy

> [!success]  
> ✅ `unknown` = biztonság  
> ✅ explicit típus = kevesebb bug

---

## 🔟 Egy mondatos összefoglaló

> [!quote]  
> **Az alaptípusok nem korlátoznak – kereteket adnak, hogy ne lődd szét a saját kódodat.**

---

🧠 Gondolkodj el rajta:

- Hol használsz ma `any`-t megszokásból?
- Hol lenne jobb az `unknown`?
- Mely változóid lehetnek valójában `null`-ok?