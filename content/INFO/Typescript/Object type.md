## 1️⃣ Mi ez?

Az **object type** azt írja le, hogy egy objektumnak:

- **milyen mezői vannak**
- **azok milyen típusúak**
- **melyek kötelezők és melyek opcionálisak**

> [!important]  
> Objektum típus = **adat szerződés**, nem csak adathalmaz.

---

## 2️⃣ Alap objektum típus

### Egyszerű példa

```ts
type User = {
  id: number;
  name: string;
  isAdmin: boolean;
};
```

> [!info]  
> Ez azt jelenti:
> 
> - a `User` objektumnak **pont ezek a mezői vannak**
>     
> - más nem lehet rajta
>     
> - ezek nem hiányozhatnak
>     

---

### Használat

```ts
const user: User = {
  id: 1,
  name: "Krisz",
  isAdmin: true,
};
```

> [!success]  
> A TypeScript **ellenőrzi a struktúrát**.

---

## 3️⃣ Optional property (`?`)

### Mi ez?

Az opcionális property azt jelenti, hogy a mező:

- **lehet, hogy létezik**
- **lehet, hogy nem**

```ts
type User = {
  id: number;
  name: string;
  email?: string;
};
```

> [!important]  
> `email?: string` = `string | undefined`

---

### Használat

```ts
const user1: User = {
  id: 1,
  name: "Krisz",
};

const user2: User = {
  id: 2,
  name: "Peti",
  email: "peti@email.com",
};
```

> [!success]  
> Mindkettő helyes.

---

### Tipikus hiba

```ts
user.email.toLowerCase(); // ❌ lehet undefined
```

✅ Helyesen:

```ts
if (user.email) {
  user.email.toLowerCase();
}
```

> [!warning]  
> Optional property **mindig ellenőrzést igényel**.

---

## 4️⃣ Readonly property

### Mi ez?

A `readonly` mező:

- inicializáláskor beállítható
- **utána nem módosítható**

```ts
type User = {
  readonly id: number;
  name: string;
};
```

---

### Mit véd meg?

```ts
user.id = 2; // ❌ nem engedi
user.name = "Más"; // ✅
```

> [!success]  
> Megakadályozza az **állapot szétcsúszását**.

---

## 5️⃣ Mikor használj readonly-t?

> [!tip]  
> Akkor, amikor:
> 
> - az érték azonosító
>     
> - adatbázisból jön
>     
> - nem változhat üzleti logika szerint
>     

Példák:

- `id`
- `createdAt`
- `uuid`

---

## 6️⃣ Optional vs Readonly – ne keverd!

> [!danger]  
> ❌ Optional ≠ readonly

- optional → **lehet, hogy nincs**
- readonly → **van, de nem módosítható**

---

## 7️⃣ Objektum típus mint dokumentáció

```ts
type LoginPayload = {
  username: string;
  password: string;
};
```

> [!info]  
> Ez:
> 
> - jobb, mint komment
>     
> - jobb, mint wiki
>     
> - mindig igaz
>     

---

## 8️⃣ Gyakori hibák

> [!danger]  
> ❌ mindent optional-ra teszel  
> ❌ readonly-t nem használsz soha  
> ❌ objektumot tuple-lel modellezel

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **Az object type nem adatstruktúra, hanem üzleti szerződés a kódban.**

---

🧠 Gondolkodj el rajta:

- Mely mezők nem változhatnának a modelljeidben?
- Hol használsz optional property-t lustaságból?
- Mely objektumod lenne tisztább egy szigorúbb típussal?