---
title: UseEffect
tags:
  - frontend
  - react
  - useEffect
  - dev
  - programming
  - programozás
created: 2026-01-23
status: complete
---
## Mi ez?

A `useEffect` egy **React Hook**, amivel **mellékhatásokat** kezelsz a komponens életciklusa során.

> Mellékhatás = minden, ami **nem pusztán JSX kirajzolás**

Példák:

- API hívás
    
- event listener feliratkozás
    
- timer (`setInterval`, `setTimeout`)
    
- DOM-manipuláció
    
- logolás
    

---

## Miért létezik?

Reactben a render **tiszta függvény** kell legyen:

- ugyanarra az inputra ugyanazt rajzolja ki
    
- ne csináljon "külső dolgokat"
    

> [!info] A `useEffect` **kiköltözteti** ezeket a külső dolgokat a renderből.

---

## Alap szintaxis

```tsx
useEffect(() => {

// side effect

}, [deps]);

```
Három része van:

1. **effect függvény** – mit csináljon
    
2. **dependency array** – mikor fusson
    
3. **cleanup (opcionális)** – mit takarítson el
    

---

## Mikor fut le? (EZ A LÉNYEG)

### 1️⃣ Dependency array nélkül

```tsx
useEffect(() => {

	console.log("minden rendernél");

});
```

> [!warning] ⚠️ **MINDEN rendernél lefut** → legtöbbször rossz ötlet

---

### 2️⃣ Üres dependency array `[]`

```tsx
useEffect(() => {

	console.log("csak egyszer, mountkor");

}, []);
```

> [!info] ✔️ **Komponens betöltésekor egyszer**
> 
> Mentális modell: `componentDidMount`

Tipikus használat:

- első API hívás
    
- inicializálás
    

---

### 3️⃣ Konkrét dependencykkel

```tsx
useEffect(() => {

	console.log("username megváltozott");

}, [username]);
```

> [!info] ✔️ Lefut **mountkor** és **minden alkalommal**, amikor `username` változik

> [!tip] Dependency = minden változó, amit **használsz az effectben**

---

## Cleanup – mikor és miért kell?

A cleanup egy **visszatérő függvény** az effectből:

```tsx
useEffect(() => {

	const id = setInterval(() => {
		console.log("tick");
	}, 1000);
	
	return () => {
		clearInterval(id);
	};

}, []);
```

> [!warning] Ha nincs cleanup:
> 
> - memória szivárgás
>     
> - duplikált eventek
>     
> - "szellemtimerek"
>     

---

### Mikor fut le a cleanup?

> [!info] A cleanup **mindig az effect újrafutása ELŐTT**, és **unmountkor** fut le.

Mentális sorrend:

1. cleanup (régi)
    
2. effect (új)
    

---

## Tipikus példák

### Event listener

```tsx
useEffect(() => {

	const handler = () => console.log("resize");
	
	window.addEventListener("resize", handler);

	return () => {
	
	window.removeEventListener("resize", handler);
	
	};

}, []);
```

> [!info] Listener mindig cleanup-pal jár

---

### API hívás

```tsx
useEffect(() => {

	fetch("/api/user")
	
	.then(r => r.json())
	
	.then(setUser);

}, []);
```


> [!tip] Fetch **nem igényel cleanupot**, de abort controller profi megoldás

---

## Gyakori félreértések

> [!warning] ❌ "Ha nincs useEffect, nem lehet hook"

Nem igaz.

- `useState`, `useContext` **useEffect nélkül is működik**
    

---

> [!warning] ❌ "A dependency array opcionális"

Technikailag igen. Gyakorlatban: **ha elhagyod, tudnod kell miért**.

---

> [!warning] ❌ "Az effect olyan, mint egy if"

Nem. Az effect **reakció** az állapotváltozásra.

---

## Mentális modell (jegyezd meg!)

> [!info] **Render = mit látsz**  
> **useEffect = mit csinál a világban**

Vagy viccesebben:

> React rajzol.  
> useEffect intézkedik.

---

## Mikor NE használj useEffect-et?

> [!tip] Ha csak:
> 
> - számolsz
>     
> - formázol adatot
>     
> - state-ből származtatsz értéket
>     

Akkor:

- sima változó
    
- vagy `useMemo`
    

---

## Egy mondatos összefoglaló

> [!info] **A useEffect arra való, hogy a React komponensed reagáljon a világra – és rendet hagyjon maga után.**

---

🧠 Gondolkodj el rajta:

- Melyik useEffect-ed fut le túl sokszor?
    
- Hol hiányzik cleanup?
    
- Mit tudnál kiszervezni custom hookba?