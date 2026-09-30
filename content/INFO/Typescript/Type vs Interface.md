## 1️⃣ Mi ez?

A **`type`** és az **`interface`** **ugyanarra szolgál**: objektumok és szerződések leírására.

Mégis ez az egyik legtöbbet vitatott téma a TypeScriptben.

> [!important]  
> **Ez nem vallásháború.**  
> Ez **tervezési döntés**.

---

## 2️⃣ A legfontosabb igazság (amit ritkán mondanak ki)

> [!quote]  
> **A legtöbb esetben mindegy, hogy `type` vagy `interface`.**

De:

- vannak **technikai különbségek**
- és vannak **architekturális következmények**

---

## 3️⃣ Mi a különbség valójában?

### 3.1 `interface`

```ts
interface User {
  id: number;
  name: string;
}
```

Jellemzők:

- csak **objektum formára** való
- **kiterjeszthető** (`extends`)
- **összeolvad** (declaration merging)

> [!info]  
> Az `interface` **nyitott szerződés**.

---

### 3.2 `type`

```ts
type User = {
  id: number;
  name: string;
};
```

Jellemzők:

- objektum, union, tuple, primitive is lehet
- **nem olvad össze**
- erősebb kifejezőerő

> [!info]  
> A `type` **lezárt definíció**.

---

## 4️⃣ Konkrét technikai különbségek

### 4.1 Union csak `type`

```ts
type Status = "success" | "error";
```

```ts
// ❌ interface nem tud ilyet
```

> [!success]  
> Union = `type`

---

### 4.2 Declaration merging (`interface`)

```ts
interface User {
  id: number;
}

interface User {
  name: string;
}
```

Ez eredményezi:

```ts
interface User {
  id: number;
  name: string;
}
```

> [!warning]  
> Ez lehet feature **és** veszély.

---

## 5️⃣ Mikor melyiket használd?

### Használj `interface`-t, ha:

> [!tip]
> 
> - **publikus szerződés** (API, library)
>     
> - **bővíthető struktúra**
>     
> - framework-integráció (React props, DOM)
>     

```ts
interface Props {
  title: string;
}
```

---

### Használj `type`-ot, ha:

> [!tip]
> 
> - **union / literal**
>     
> - komplex típuslogika
>     
> - belső domain modellek
>     

```ts
type ApiState =
  | { status: "loading" }
  | { status: "success"; data: Data }
  | { status: "error"; message: string };
```

---

## 6️⃣ Frontend + backend szerződések

### Backend oldal (API response)

```ts
type UserDto = {
  id: number;
  name: string;
};
```

> [!info]  
> Backend DTO = **lezárt adatmodell** → `type`

---

### Frontend oldal (komponens props)

```ts
interface UserCardProps {
  user: UserDto;
  onSelect(): void;
}
```

> [!info]  
> Frontend props = **bővíthető szerződés** → `interface`

---

## 7️⃣ Gyakori hibák

> [!danger]  
> ❌ mindent interface-re írni  
> ❌ mindent type-ra írni  
> ❌ nem tudni, miért azt használod

---

## 8️⃣ Jó ökölszabály

> [!important]
> 
> - **Ha adat → `type`**
>     
> - **Ha szerződés → `interface`**
>     

Ez nem dogma, hanem **praktikus döntési minta**.

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **A `type` lezár, az `interface` megnyit – és neked kell tudni, mikor melyikre van szükség.**

---

🧠 Gondolkodj el rajta:

- Hol lenne veszélyes az interface összeolvadás?
- Mely típusaid valójában lezárt domain modellek?
- Hol akarsz jövőbeni bővíthetőséget?