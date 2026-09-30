## 1️⃣ Mi ez?

A **utility típusok** előre elkészített **generikus segédeszközök**, amelyekkel **meglévő típusokat tudsz átalakítani** anélkül, hogy újraírnád őket.

> [!important]  
> Utility típus = **típus-transzformátor**, nem adat.

Ha jól használod őket:

- kevesebb duplikáció
- tisztább API-k
- erősebb szerződések

---

## 2️⃣ `Partial<T>` – minden opcionális

### Mi ez?

```ts
type Partial<T> = {
  [K in keyof T]?: T[K];
};
```

> [!info]  
> A `Partial` **minden mezőt opcionálissá tesz**.

---

### Példa

```ts
type User = {
  id: number;
  name: string;
  email: string;
};

const update: Partial<User> = {
  name: "Krisz",
};
```

> [!success]  
> Ideális **update payloadhoz**.

---

## 3️⃣ `Required<T>` – minden kötelező

### Mi ez?

```ts
type Required<T> = {
  [K in keyof T]-?: T[K];
};
```

> [!info]  
> A `Required` **elveszi az opcionálisságot**.

---

### Példa

```ts
type DraftUser = {
  id?: number;
  name?: string;
};

type FinalUser = Required<DraftUser>;
```

> [!success]  
> Hasznos validálás UTÁN.

---

## 4️⃣ `Pick<T, K>` – kiválasztás

### Mi ez?

```ts
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

---

### Példa

```ts
type UserPreview = Pick<User, "id" | "name">;
```

> [!success]  
> DTO-k, listák, summary view-k.

---

## 5️⃣ `Omit<T, K>` – kizárás

### Mi ez?

```ts
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

---

### Példa

```ts
type PublicUser = Omit<User, "email">;
```

> [!success]  
> API response-oknál arany.

---

## 6️⃣ `Record<K, V>` – kulcs–érték szerződés

### Mi ez?

```ts
type Record<K extends keyof any, V> = {
  [P in K]: V;
};
```

---

### Példa

```ts
type Role = "admin" | "user";

type Permissions = Record<Role, boolean>;
```

```ts
const perms: Permissions = {
  admin: true,
  user: false,
};
```

> [!success]  
> Kizárja a hiányzó vagy extra kulcsokat.

---

## 7️⃣ Mikor melyiket használd?

|Utility|Tipikus használat|
|---|---|
|Partial|update DTO|
|Required|validált adat|
|Pick|lista / preview|
|Omit|public DTO|
|Record|map / lookup|

---

## 8️⃣ Gyakori hibák

> [!danger]  
> ❌ mindent Partial-ra tenni  
> ❌ utility-k egymásba ágyazása érthetőség nélkül  
> ❌ Record ott, ahol object literal elég

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A utility típusok nem lustaságot, hanem következetességet adnak.**

---

🧠 Gondolkodj el rajta:

- Hol duplikálsz ma DTO-kat feleslegesen?
    
- Mely payloadjaid lennének tisztábbak utility-kkel?
    
- Hol tudnád lezárni az API szerződésedet ezekkel?