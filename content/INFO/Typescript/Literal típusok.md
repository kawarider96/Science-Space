## 1️⃣ Mi ez?

A **literal típus** azt jelenti, hogy egy változó **nem csak egy típusba tartozik**, hanem **konkrét érték(ek) egyikére van korlátozva**.

```ts
let status: "success" | "error";
```

> [!important]  
> Itt nem azt mondod, hogy „string”.  
> Azt mondod: **pont EZEK a stringek lehetnek**.

Ez a TypeScript egyik **legprecízebb fegyvere**.

---

## 2️⃣ Miért léteznek a literal típusok?

A valós rendszerekben az állapotok **nem végtelenek**:

- egy kérés lehet sikeres vagy hibás
- egy modal lehet nyitva vagy zárva
- egy user lehet admin vagy user

> [!info]  
> A literal típus lehetővé teszi, hogy ezeket az **állapotokat lezárd**.

Nincs „majd meglátjuk”. Csak engedélyezett értékek vannak.

---

## 3️⃣ Alap példa – `"success" | "error"`

```ts
type RequestStatus = "success" | "error";

let status: RequestStatus;

status = "success"; // ✅
status = "error";   // ✅
status = "pending"; // ❌
```

> [!success]  
> A TypeScript **megállítja a rossz állapotokat**.

---

## 4️⃣ Literal típus ≠ sima string

```ts
let status = "success";
```

A TS ezt így látja:

```ts
status: "success"
```

De:

```ts
let status: string = "success";
```

> [!danger]  
> Itt **elvesztetted a pontosságot**.

---

## 5️⃣ Literal típusok és type inference

```ts
const status = "success";
```

> [!info]  
> `const` esetén a TypeScript **automatikusan literal típust ad**.

Ezért működik jól:

```ts
const STATUS_SUCCESS = "success" as const;
```

---

## 6️⃣ Enum vs Literal union

### Enum

```ts
enum Status {
  Success = "success",
  Error = "error",
}
```

### Literal union

```ts
type Status = "success" | "error";
```

---

## 7️⃣ Mikor jobb a literal enum helyett?

> [!success]  
> Literal union jobb, ha:
> 
> - frontend state
>     
> - API response
>     
> - nincs szükség runtime objektumra
>     
> - minél kevesebb JS kódot akarsz
>     

> [!warning]  
> Enum jobb, ha:
> 
> - runtime-ban is kell
>     
> - shared backend–frontend konstans
>     
> - switch-case sok helyen
>     

---

## 8️⃣ Frontend state modellezés

### Rossz (boolean pokol)

```ts
let isLoading = false;
let hasError = false;
```

> [!danger]  
> Kombinálható, értelmetlen állapotok.

---

### Jó (literal + union)

```ts
type State = "idle" | "loading" | "success" | "error";

let state: State = "idle";
```

```ts
if (state === "error") {
  // pontosan tudod, hol vagy
}
```

> [!success]  
> Ez már **állapotgép**, nem tippelés.

---

## 9️⃣ Literal + object = szupererő

```ts
type ApiState =
  | { status: "loading" }
  | { status: "success"; data: Data }
  | { status: "error"; message: string };
```

> [!important]  
> Ez a **discriminated union** alapja.

---

## 🔟 Gyakori hibák

> [!danger]  
> ❌ túl tág string típus  
> ❌ enum mindenre  
> ❌ boolean flag-ek literal helyett

---

## 1️⃣1️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A literal típus nem adatot, hanem állapotot ír le – és ez frontendben aranyat ér.**

---

🧠 Gondolkodj el rajta:

- Hol használsz ma stringeket, amik valójában állapotok?
- Hol lenne jobb literal enum helyett?
- Mely boolean flag-eket tudnád kiváltani literal state-tel?