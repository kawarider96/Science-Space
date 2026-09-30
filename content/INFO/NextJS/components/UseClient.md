---
title: UseClient
tags:
  - programming
  - programozás
  - javascript
  - react
  - nextjs
  - useClient
  - dev
  - framework
created: 2026-01-22
status: complete
---
## Mi ez? (egy mondatban)

A `"use client"` egy fájl tetejére írt direktíva, amivel azt mondod Next.js-nek: **ez a komponens böngészőben fusson (Client Component)**, ne csak szerveren.

> [!info] **Mentális kép:** a Next.js App Routerben két világ van:
> 
> - **Szerver világ**: gyors, biztonságos, adatlekérésre király
>     
> - **Kliens világ**: interaktív (click, state, effect), de nehezebb és drágább (JS bundle)
>     
> 
> A `use client` olyan, mint egy **„átjáró-pecsét”**: innentől ez a fájl és a belőle importált komponensfa **kliens oldali** lesz.

---

## Miért létezik?

Mert az App Router alapból **Server Components**-re épül. Ez jó, mert:

- kevesebb JS megy a böngészőbe
    
- gyorsabb első render
    
- könnyebb szerver oldali adatlekérés
    

Viszont: ha kell **interakció**, kell a kliens.

> [!warning] Ha nincs rá okod, **ne tegyél mindent** `**use client**`**-re**. Olyan, mintha egy Ferrari motorját beletolnád egy bevásárlókocsiba: megy, csak… minek.

---

## Hogyan működik? (lépésről lépésre)

1. A fájl **első sorába** írod:
    
    "use client";
    
2. Ettől a komponens:
    
    - futtathat React hookokat (`useState`, `useEffect`, stb.)
        
    - kezelhet eseményeket (`onClick`, `onChange`)
        
    - használhat böngészős API-kat (`window`, `localStorage`, `document`)
        
3. **Fontos:** ami egy `use client` fájlból importálva kerül be (komponensfa lefelé), az is kliens oldalira húzódik.
    

> [!tip] **Minél lejjebb tedd a kliens-határt.** Legyen szerver oldali a layout + adatlekérés, és csak a kis interaktív „sziget” legyen kliens.

---

## Mire használjuk a valóságban?

Tipikus okok `use client`-re:

- gombnyomásra modal nyitás
    
- form inputok kezelése
    
- dinamikus UI állapot (tab, accordion, dropdown)
    
- animációk, drag&drop
    
- realtime dolgok (websocket UI)
    

> [!info] **Szabály:** ha a komponensednek kell **state/effect/event**, akkor valószínű `use client` kell.

---

## Hol találkozol vele programozásban / rendszerekben?

### 1) „Interaktív szigetek” modell

A szerver oldal adja a vázat (HTML + adat), a kliens csak az interaktív részeket.

graph TD

A[Server Layout] --> B[Server Page]

B --> C[Client Island: Filters]

B --> D[Server Product List]

C -->|user click| E[Client State]

E -->|request| F[Server Action / API]

### 2) Hydration (ráül a JS az SSR HTML-re)

- Szerver: legenerál HTML-t
    
- Böngésző: letölti a JS-t
    
- React: „összeházasítja” a HTML-t a komponensekkel (hydration)
    

> [!warning] **Hydration mismatch** tipikusan akkor jön, ha szerveren és kliensen mást renderelsz. Példa: `Date.now()`, `Math.random()`, locale-dátum formázás.

---

## Mini példák

### A) Interaktív gomb (kell a kliens)

"use client";

  

import { useState } from "react";

  

export function Counter() {

const [n, setN] = useState(0);

return (

<button onClick={() => setN(n + 1)}>

Klikk: {n}

</button>

);

}

### B) Szerver oldal + kliens sziget (ajánlott)

**Page (server)**

import { Counter } from "./Counter";

  

export default async function Page() {

// itt jöhet adatlekérés (db, fetch)

return (

<div>

<h1>Termékek</h1>

<Counter />

</div>

);

}

**Counter (client)**

"use client";

  

import { useState } from "react";

  

export function Counter() {

const [open, setOpen] = useState(false);

return (

<div>

<button onClick={() => setOpen(!open)}>Modal</button>

{open && <div>Helló modal</div>}

</div>

);

}

---

## Gyakori félreértések

- **„Ha** `**use client**`**, akkor nincs SSR”** → de, lehet SSR HTML, csak a komponens JS-sel hidratálódik.
    
- **„A** `**use client**` **csak egy komponensre vonatkozik”** → a hatás **lefelé terjed** az importált komponensfán.
    
- **„Inkább mindent kliensre rakok, az egyszerűbb”** → rövid távon igen, hosszú távon lassabb app + nagyobb bundle.
    

> [!tip] Ha bizonytalan vagy: kezdj Server Componenttel, és csak akkor tedd kliensre, ha a React az arcodba dobja, hogy „hooks only work in Client Components”.

---

## Ökölszabály (NEO-féle, kicsit szemtelen)

- **Szerver**: adat + váz + SEO + gyorsaság
    
- **Kliens**: interakció + állapot + UX cukorka
    

Ha mindent kliensre raksz, az olyan, mintha a pizzát **futár helyett te dobnád át az ablakon**: célba ér, csak nincs benne rendszer.

---

🧠 Gondolkodj el rajta:

- A saját Next.js appodban (Teletál) melyik 2-3 UI elem az, ami tényleg kliens „sziget” kell legyen, és mi az, ami nyugodtan maradhat szerver oldali?