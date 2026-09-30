## 1️⃣ Mi ez?

A **type inference** azt jelenti, hogy a TypeScript **kitalálja helyetted a típust** az érték alapján.

```ts
let count = 10;
// count: number
```

> [!important]  
> A TypeScript **nem találgat**.  
> Következtet – szabályok alapján.

---

## 2️⃣ Miért létezik a type inference?

Ha mindenhez ezt kéne írni:

```ts
let count: number = 10;
```

akkor a TypeScript:

- zajos lenne
- lassítaná az írást
- elvesztené az ergonomiáját

> [!info]  
> A type inference célja: **kevesebb kód, ugyanannyi biztonság**.

---

## 3️⃣ Mikor következtet jól a TypeScript?

### 3.1 Egyszerű értékadásnál

```ts
let name = "Krisz";   // string
let age = 27;         // number
let isAdmin = false;  // boolean
```

> [!success]  
> Itt **nem kell** explicit típus.

---

### 3.2 Konstansoknál (`const`)

```ts
const role = "admin";
```

Itt a típus:

```ts
role: "admin"
```

> [!info]  
> `const` esetén a TS **szűkebb típust** ad (literal type).

---

### 3.3 Visszatérési érték alapján (egyszerű függvény)

```ts
function double(x: number) {
  return x * 2;
}
```

A TypeScript tudja:

```ts
(x: number) => number
```

> [!success]  
> Egyszerű logikánál jól működik.

---

## 4️⃣ Mikor NEM szabad rábízni a type inference-re?

Itt kezdődik a mérnöki döntés.

---

### 4.1 Üres inicializálásnál

```ts
let users = [];
```

A TS ezt látja:

```ts
let users: any[];
```

> [!danger]  
> **Láthatatlan `any`** – a legveszélyesebb fajta.

✅ Helyesen:

```ts
let users: User[] = [];
```

---

### 4.2 Később feltöltött változónál

```ts
let result;

result = fetchData();
```

> [!warning]  
> A TypeScript itt **nem tud következtetni**.

✅ Helyesen:

```ts
let result: ApiResponse;
```

---

### 4.3 Függvényeknél (publikus API)

```ts
function getUser(id: number) {
  return { id, name: "Krisz" };
}
```

> [!warning]  
> A visszatérési típus **később elcsúszhat**.

✅ Helyesen:

```ts
function getUser(id: number): User {
  return { id, name: "Krisz" };
}
```

> [!important]  
> **Publikus függvény = explicit szerződés**.

---

### 4.4 `useState` Reactben

```ts
const [user, setUser] = useState(null);
```

A TS ezt mondja:

```ts
user: null
```

> [!danger]  
> Innen nincs visszaút.

✅ Helyesen:

```ts
const [user, setUser] = useState<User | null>(null);
```

---

## 5️⃣ Implicit vs explicit – döntési szabályok

> [!tip]  
> **Olvasáskor legyen egyértelmű, mit vár a kód.**

### Használj implicit típust, ha:

- lokális változó
- azonnal értéket kap
- a típus triviális

```ts
let total = price * quantity;
```

---

### Használj explicit típust, ha:

- publikus API
- state
- függvény visszatérés
- üres inicializálás
- hosszú élettartamú változó 

```ts
let selectedUser: User | null = null;
```

---

## 6️⃣ Fontos felismerés

> [!important]  
> A type inference **kényelmi funkció**, nem biztonsági háló.

A TypeScript:

- segít, ha tud
- hallgat, ha nem

Neked kell tudni, **mikor kérsz segítséget**.

---

## 7️⃣ Gyakori hibák

> [!danger]  
> ❌ „Majd kitalálja”  
> ❌ Üres array típus nélkül  
> ❌ Függvény visszatérés explicit típus nélkül

> [!success]  
> ✅ Tudatos explicit szerződések  
> ✅ Inference ott, ahol biztonságos

---

## 8️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A jó TypeScript nem attól jó, hogy mindent kiírsz, hanem attól, hogy tudod, mikor nem szabad.**

---

🧠 Gondolkodj el rajta:

- Hol hagyatkozol ma vakon a type inference-re?
- Mely változóid élnek túl sokáig explicit típus nélkül?
- Hol lenne jobb, ha a típus dokumentáció is lenne?