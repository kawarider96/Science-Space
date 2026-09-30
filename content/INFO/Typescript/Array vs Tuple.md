## 1️⃣ Mi ez?

Az **array** és a **tuple** első ránézésre hasonló, de **teljesen más problémát oldanak meg**.

> [!important]  
> **Array = sok azonos dolog**  
> **Tuple = kevés, fix jelentésű dolog sorrendben**

Ha ezt nem választod szét fejben, nagyon gyorsan csúnya típusaid lesznek.

---

## 2️⃣ Array (`string[]`)

### Mi ez?

Az array **azonos típusú elemek listája**.

```ts
const names: string[] = ["Krisz", "Peti", "Anna"];
```

> [!info]  
> Az array azt mondja:  
> „Bárhány elem lehet, **mind ugyanaz a típus**.”

---

### Mit garantál az array?

```ts
names.push("Béla");     // ✅ oké
names.push(42);          // ❌ nem oké
```

> [!success]  
> A TypeScript garantálja az **egyneműséget**.

---

### Mikor használd az array-t?

> [!tip]  
> Akkor, amikor:
> 
> - lista
>     
> - iterálsz rajta (`map`, `filter`)
>     
> - az elemek száma változik
>     

**99%-ban array kell.**

---

## 3️⃣ Tuple (`[string, number]`)

### Mi ez?

A tuple egy **fix hosszúságú**, **fix jelentésű** adatszerkezet.

```ts
const userTuple: [string, number] = ["Krisz", 28];
```

> [!info]  
> A tuple azt mondja:  
> „Az első elem EZ, a második AZ.”

---

### Mit garantál a tuple?

```ts
userTuple[0]; // string
userTuple[1]; // number
userTuple[2]; // ❌ nincs ilyen
```

> [!success]  
> A **pozíció hordozza a jelentést**.

---

## 4️⃣ A leggyakoribb hiba tuple-nél

```ts
const user = ["Krisz", 28];
```

A TypeScript ezt így látja:

```ts
(string | number)[]
```

> [!danger]  
> Ez **nem tuple**, hanem egy kaotikus array.

✅ Helyesen:

```ts
const user: [string, number] = ["Krisz", 28];
```

---

## 5️⃣ Mikor kell tuple? (spoiler: ritkán)

> [!warning]  
> Ha gondolkodnod kell rajta, hogy kell-e tuple, akkor **valószínűleg nem kell**.

### Jó tuple use-case-ek

- `useState` visszatérés:

```ts
const [value, setValue] = useState(0);
```

- koordináták:

```ts
type Point = [number, number];
```

- key–value párok (átmenetileg)

---

### Rossz tuple use-case-ek

> [!danger]  
> ❌ API response  
> ❌ komplex adatmodell  
> ❌ üzleti logika

Ilyenkor **object kell**, nem tuple.

---

## 6️⃣ Miért veszélyes túlhasználni a tuple-t?

```ts
type UserTuple = [number, string, boolean, string];
```

> [!danger]  
> Ez **olvashatatlan**.

Melyik mit jelent?

- index 1?
- index 3?

👉 **Objektum sokkal jobb**:

```ts
type User = {
  id: number;
  name: string;
  isAdmin: boolean;
  email: string;
};
```

---

## 7️⃣ Mentális döntési szabály

> [!important]  
> Ha az adatnak **neve van**, objektum kell.  
> Ha az adatnak **csak pozíciója van**, tuple jöhet szóba.

---

## 8️⃣ Gyakori félreértések

> [!danger]  
> ❌ Tuple = kisebb object  
> ❌ Tuple = gyorsabb  
> ❌ Tuple = modernebb

> [!success]  
> ✅ Tuple = fix szerződés pozíció alapján

---

## 9️⃣ Egy mondatos összefoglaló

> [!quote]  
> **Array-ben az elemek számítanak, tuple-ben a helyük.**

---

🧠 Gondolkodj el rajta:

- Használsz-e tuple-t ott, ahol objektum lenne tisztább?
- Van-e olyan array-d, ami valójában fix jelentésű adatokat hordoz?
- Tudná-e egy másik fejlesztő, mit jelent a tuple-ed 3. eleme?