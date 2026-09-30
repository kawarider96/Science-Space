## 1️⃣ Mi ez?

A **generikus függvény** olyan függvény, amely **nem egy konkrét típusra van drótozva**, mégis **megőrzi a típusinformációt**.

```ts
function identity<T>(value: T): T {
  return value;
}
```

> [!important]  
> A generikus függvény **ugyanazt a típust adja vissza**, amit kap.

---

## 2️⃣ Mentális modell – típus bejön, típus kimegy

Gondolj rá így:

```text
T  ──▶  [ FÜGGVÉNY LOGIKA ]  ──▶  T
```

> [!info]  
> A függvény **nem tudja**, mi az a `T`, de **tiszteletben tartja**.

Pont úgy, mint:

```ts
function double(x: number): number {
  return x * 2;
}
```

Csak itt a típus **paraméter**.

---

## 3️⃣ Klasszikus példa – `identity`

```ts
function identity<T>(value: T): T {
  return value;
}
```

Használat:

```ts
const a = identity(10);        // number
const b = identity("alma");   // string
```

> [!success]  
> A TypeScript **nem veszti el a típust**.

---

## 4️⃣ Mi lenne generikus nélkül?

### `any`-vel (rossz)

```ts
function identity(value: any) {
  return value;
}
```

```ts
const x = identity(10);
x.toUpperCase(); // ❌ runtime error
```

> [!danger]  
> `any` = kikapcsolt TypeScript.

---

## 5️⃣ Miért jobb a generikus, mint az `any`?

> [!important]  
> A generikus **megtartja a kapcsolatot** a bemenet és a kimenet között.

|Megoldás|Típusmegőrzés|Biztonság|TS segítség|
|---|---|---|---|
|`any`|❌ nincs|❌|❌|
|generikus|✅ van|✅|✅|

---

## 6️⃣ Generikus + constraint (első ízelítő)

```ts
function logLength<T extends { length: number }>(value: T): T {
  console.log(value.length);
  return value;
}
```

> [!info]  
> Itt a generikus **nem teljesen szabad**.

Ez azt mondja:

- bármi jöhet
- **ami rendelkezik `length`-tel**

---

## 7️⃣ Mikor érdemes generikus függvényt írni?

> [!tip]  
> Akkor, amikor:
> 
> - a logika független a típustól
>     
> - a típusinformáció fontos
>     
> - nem akarsz overload poklot
>     

Példák:

- `map`, `filter`, `first`
- API wrapper helper
- form utility-k

---

## 8️⃣ Gyakori hibák

> [!danger]  
> ❌ generikus mindenre  
> ❌ értelmetlen `T` név  
> ❌ `any` használata generikus helyett

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A generikus függvény nem a típust rejti el, hanem megőrzi – és ez óriási különbség.**

---

🧠 Gondolkodj el rajta:

- Mely függvényeid vesztik el ma a típusinformációt?
- Hol használsz `any`-t lustaságból?
- Mely util függvényed lenne tisztább generikussal?