---
title: Dockerignore
tags:
  - informatika
  - programming
  - programozás
  - docker
  - dev
  - dockerignore
status: complete
created: 2026-01-17
---
# Mi az a `.dockerignore`?

## 1️⃣ Mi ez? (nagyon egyszerűen)

A **`.dockerignore`** egy fájl, ami megmondja a Dockernek:

> **mely fájlokat NE küldje be a build során**

Ennyi.  
Nem futtat.  
Nem töröl.  
Nem befolyásolja a futó konténert.

---

## 2️⃣ Miért létezik?

Mert a `docker build` **nem válogat** magától.

Amikor ezt futtatod:

```bash
docker build .
```

A Docker:

- fogja az **aktuális mappát** (build context)
- és **felküldi az egészet** a Docker daemonnek

👉 Ha nincs `.dockerignore`, akkor:

- `node_modules`
- `.git`
- logok
- cache
- titkos fájlok

**mind mennek vele**.

---

## 3️⃣ Mentális modell (nagyon fontos)

> **Build context = csomag**, amit a Dockernek átnyújtasz
> 
> **`.dockerignore` = kidobod belőle a szemetet, mielőtt átadod**

Ha nincs `.dockerignore`:

- hatalmas csomag
- lassú build
- felesleges fájlok

---

## 4️⃣ Mit NEM csinál a `.dockerignore`?

Ez kritikus.

A `.dockerignore` **NEM**:

- törli a fájlokat a gépedről
- tiltja, hogy `RUN npm install` létrehozza a `node_modules`-t
- hatással van a futó containerre

> Csak a **host → Docker build** irányt szabályozza.

---

## 5️⃣ Kapcsolat a `COPY`-val

Ez a két sor együtt értelmezhető:

```dockerfile
COPY . .
```

A Docker ilyenkor:

1. megnézi a build contextet
2. kidobja, ami `.dockerignore`-ban van
3. a maradékot másolja be

👉 Ami `.dockerignore`-ban van, az **soha nem kerülhet be COPY-val**.

---

## 6️⃣ Klasszikus példa: `node_modules`

### Rossz (nincs `.dockerignore`)

- host `node_modules` bemásolódik
- Windows binárisok Linux containerbe
- random hibák

### Jó (van `.dockerignore`)

```dockerignore
node_modules
```

- a konténer **saját maga** telepít
- platformfüggetlen
- reprodukálható

---

## 7️⃣ DEV vs PROD – számít a `.dockerignore`?

### DEV

✔️ IGEN

- gyorsabb build
- nem keveredik host környezet

### PROD

✔️ IGEN (még fontosabb)

- kisebb image
- kevesebb támadási felület

👉 **Mindig kell.**

---

## 8️⃣ Tipikus `.dockerignore` fájl

```dockerignore
node_modules
.git
.env
Dockerfile
docker-compose.yml
npm-debug.log
dist
build
```

> [!tip]  
> Inkább legyen kicsit szigorúbb, mint túl laza.

---

## 9️⃣ Gyakori félreértések

- **„Ha benne van a dockerignore-ban, akkor a konténerben sincs.”** → nem feltétlen
- **„Kiváltja a multi-stage buildet.”** → nem
- **„Dev-ben nem kell.”** → kell
