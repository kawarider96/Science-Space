## 1️⃣ Mi ez?

A **generikus interface / type** lehetővé teszi, hogy **adatstruktúrákat írj le úgy**, hogy a belső adat típusa **később legyen megadva**, de a **szerkezet fix maradjon**.

```ts
type ApiResponse<T> = {
  status: "success" | "error";
  data: T;
};
```

> [!important]  
> Itt **nem a forma változik**, hanem a _tartalom típusa_.

---

## 2️⃣ Mentális modell

Gondolj a generikus objektumra úgy, mint egy **dobozra**:

> „A doboz alakja mindig ugyanaz, csak azt döntjük el később, _mi van benne_.”

```text
ApiResponse< T >
┌───────────────────┐
│ status            │
│ data :     T      │
└───────────────────┘
```

---

## 3️⃣ API response wrapper

### Probléma generikus nélkül

```ts
type UserResponse = {
  status: "success" | "error";
  data: User;
};

type ProductResponse = {
  status: "success" | "error";
  data: Product;
};
```

> [!warning]  
> Ugyanaz a struktúra, csak a `data` más.

---

### Megoldás generikussal

```ts
type ApiResponse<T> = {
  status: "success" | "error";
  data: T;
};
```

Használat:

```ts
ApiResponse<User>
ApiResponse<Product>
```

> [!success]  
> Nincs duplikáció, a típus megmarad.

---

## 4️⃣ Form state típusok

```ts
type FormState<T> = {
  values: T;
  errors: Partial<Record<keyof T, string>>;
  isSubmitting: boolean;
};
```

Használat:

```ts
type LoginForm = {
  username: string;
  password: string;
};

const loginForm: FormState<LoginForm> = {
  values: { username: "", password: "" },
  errors: {},
  isSubmitting: false,
};
```

> [!info]  
> A form logika **független a konkrét mezőktől**.

---

## 5️⃣ Repository pattern TypeScriptben

### Generikus repository szerződés

```ts
interface Repository<T> {
  findById(id: number): Promise<T | null>;
  findAll(): Promise<T[]>;
  save(entity: T): Promise<void>;
}
```

Használat:

```ts
class UserRepository implements Repository<User> {
  async findById(id: number) { /* ... */ }
  async findAll() { /* ... */ }
  async save(user: User) { /* ... */ }
}
```

> [!success]  
> A repository **nem tudja**, mi az a `User`, de **konzisztensen kezeli**.

---

## 6️⃣ Interface vs type generikussal

> [!tip]
> 
> - `interface` → szerződés (Repository)
>     
> - `type` → adat (ApiResponse, FormState)
>     

Mindkettő lehet generikus.

---

## 7️⃣ Mikor ideális a generikus object?

> [!important]  
> Akkor, amikor:
> 
> - a struktúra fix
>     
> - a tartalom változó
>     
> - a logika típusfüggetlen
>     

---

## 8️⃣ Gyakori hibák

> [!danger]  
> ❌ túl absztrakt generikusok  
> ❌ `any` a generikus helyén  
> ❌ generikus ott, ahol nincs valódi variancia

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A generikus interface nem bonyolít, hanem felszabadít: egy logika, sok típus.**

---

🧠 Gondolkodj el rajta:

- Hol duplikálod ma ugyanazt az adatstruktúrát?
    
- Mely service / repository logikád független a konkrét adattól?
    
- Hol veszítesz típusinformációt generikus hiányában?