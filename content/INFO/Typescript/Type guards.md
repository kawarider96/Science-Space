## 1️⃣ Mi ez?

A **type guard** olyan kódrészlet, amivel **bizonyítod a TypeScriptnek**, hogy egy adott ponton **egy konkrét típusú értékkel dolgozol**.

> [!important]  
> A TypeScript **nem hisz neked bemondásra**.  
> Bizonyítékot kér.

---

## 2️⃣ Miért nem elég egy sima `if`?

```ts
function print(value: string | number) {
  if (value) {
    value.toString(); // ❌ nem biztos
  }
}
```

> [!danger]  
> Ez az `if` **nem bizonyít típust**, csak truthy/falsy-t.

A TypeScript nem azt kérdezi:

> „létezik-e?”

Hanem ezt:

> „MELYIK típus?”

---

## 3️⃣ `typeof` – primitívekhez

### Mikor használd?

> [!tip]  
> `string`, `number`, `boolean`, `undefined`, `function`

```ts
function format(value: string | number) {
  if (typeof value === "string") {
    value.toUpperCase(); // ✅ string
  } else {
    value.toFixed(2);    // ✅ number
  }
}
```

> [!success]  
> `typeof` **lezárja a union-t**.

---

## 4️⃣ `in` – objektum property alapján

### Mikor használd?

> [!tip]  
> Objektum union-nél, amikor **mező különböztet meg**.

```ts
type User = { id: number; name: string };
type Admin = { id: number; name: string; permissions: string[] };

type Person = User | Admin;
```

```ts
function printPerson(p: Person) {
  if ("permissions" in p) {
    p.permissions; // ✅ Admin
  } else {
    p.name;        // ✅ User
  }
}
```

> [!success]  
> Az `in` **strukturális bizonyíték**.

---

## 5️⃣ `instanceof` – osztályokhoz

### Mikor használd?

> [!tip]  
> Csak akkor, ha **class** van a háttérben.

```ts
class ApiError extends Error {
  code: number;
}
```

```ts
function handleError(err: unknown) {
  if (err instanceof ApiError) {
    err.code; // ✅ ApiError
  }
}
```

> [!warning]  
> `instanceof` **nem működik** interface/type esetén.

---

## 6️⃣ Mi történik type guard után?

> [!important]  
> A TypeScript **emlékszik a szűkítésre** az adott scope-on belül.

```ts
if (typeof value === "string") {
  // itt value: string
}
// itt újra string | number
```

---

## 7️⃣ Gyakori hibák

> [!danger]  
> ❌ truthy/falsy ellenőrzés típus helyett  
> ❌ `instanceof` type/interface-hez  
> ❌ feltételezés bizonyítás nélkül

---

## 8️⃣ Mikor MELYIKET?

|Guard|Mire jó|
|---|---|
|`typeof`|primitívek|
|`in`|objektum mezők|
|`instanceof`|class példány|

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A type guard nem ellenőrzés, hanem bizonyítás a TypeScript számára.**

---

🧠 Gondolkodj el rajta:

- Hol használsz ma `if`-et típusbizonyítás nélkül?
    
- Mely union-jaid lennének tisztábbak guarddal?
    
- Hol kellene inkább `unknown` + type guard?