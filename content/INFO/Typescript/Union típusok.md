## 1️⃣ Mi ez?

A **union típus** azt jelenti, hogy egy érték **több lehetséges típus valamelyike lehet**.

```ts
let value: string | number;
```

> [!important]  
> Union = **VAGY kapcsolat**, nem ÉS.

Ez az a pont, ahol a TypeScript **elszakad a klasszikus típusnyelvektől**, és brutálisan erőssé válik.

---

## 2️⃣ Miért létezik a union típus?

A valóság **nem fekete-fehér**:

- egy adat lehet jó vagy hibás
- egy mező lehet kitöltve vagy üres
- egy kérés sikeres vagy sikertelen

> [!info]  
> A union típus lehetővé teszi, hogy **ezeket a valós állapotokat őszintén leírd**.

---

## 3️⃣ Alap példa – `string | number`

```ts
function printId(id: string | number) {
  console.log(id);
}
```

> [!warning]  
> Itt **nem tudod**, hogy `id` string vagy number.

Ezért EZ NEM OKÉ:

```ts
id.toUpperCase(); // ❌
```

A TypeScript azt mondja:

> „Bizonyítsd be.”

---

## 4️⃣ Union + type narrowing

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    id.toUpperCase(); // ✅ string
  } else {
    id.toFixed(0);    // ✅ number
  }
}
```

> [!success]  
> A union **kényszerít a helyes elágazásra**.

---

## 5️⃣ Miért az egyik legerősebb TS-features?

> [!important]  
> A union típus **rákényszerít, hogy minden lehetséges állapottal számolj**.

Ez:

- csökkenti a rejtett bugokat
- dokumentálja az állapotokat
- kiszorítja a "majd meglátjuk" logikát

---

## 6️⃣ Valós példa #1 – API response

### Rossz (JS-es gondolkodás)

```ts
const res = await fetch("/api/user");
const data = await res.json();

console.log(data.user.name);
```

> [!danger]  
> Feltételezésre épít.

---

### Jó (union típusos gondolkodás)

```ts
type UserResponse =
  | { status: "success"; user: User }
  | { status: "error"; message: string };
```

```ts
if (data.status === "success") {
  data.user.name; // ✅
} else {
  data.message;   // ✅
}
```

> [!success]  
> **Nem tudsz hibás ágat elfelejteni.**

---

## 7️⃣ Valós példa #2 – Form state

```ts
type FormState =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success" }
  | { status: "error"; error: string };
```

```ts
if (form.status === "error") {
  form.error; // ✅ csak itt létezik
}
```

> [!info]  
> Ez **state machine**, nem ad-hoc boolean flag-ek.

---

## 8️⃣ Union vs `any`

> [!danger]  
> `any` = „bármi lehet, nem érdekel”

> [!success]  
> `string | number` = „pontosan EZEK lehetnek”

> [!important]  
> A union **korlátoz**, és ettől biztonságos.

---

## 9️⃣ Tipikus hibák

> [!danger]  
> ❌ túl tág union (`string | number | boolean | null`)  
> ❌ union használata ott, ahol object kell  
> ❌ type narrowing kihagyása

---

## 🔟 Egy mondatos összefoglaló

> [!quote]  
> **A union típus nem bizonytalanság, hanem őszinteség a lehetséges állapotokról.**

---

🧠 Gondolkodj el rajta:

- Hol használsz ma boolean flag-eket union helyett?
- Mely API válaszaid lennének tisztábbak union típusokkal?
- Hol mondod ki most először típusosan, hogy "hiba is lehet"?