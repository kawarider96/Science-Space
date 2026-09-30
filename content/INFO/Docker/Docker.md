---
title: Docker
status: complete
tags:
  - informatika
  - docker
  - programming
  - programozás
  - dev
created: 2026-01-17
---
# Docker – teljes, szájbarágós összefoglaló

## 1️⃣ Mi a Docker? (egy mondatban)

A **Docker** egy eszköz, amivel alkalmazásokat **elszigetelten**, **reprodukálhatóan** és **ugyanúgy mindenhol** tudsz futtatni.

> Ha lebutítjuk:  
> **Docker = kontrollált futtatókörnyezet dobozokban**

---

## 2️⃣ Milyen problémát old meg?

Docker nélkül:

- „nálam megy, nálad nem”
- eltérő Node / PHP / Python verziók
- OS különbségek
- kézzel telepített függőségek

Dockerrel:

- ugyanaz az app
- ugyanaz a környezet
- ugyanaz a viselkedés

---

## 3️⃣ A Docker 4 alappillére (EZ A LÉNYEG)

### 🧱 1. Dockerfile – _hogyan épül fel_

Egy **build recept**:

- milyen alap image
- mit másolunk be
- mit futtatunk buildkor

👉 Image-et gyárt.

---

### 📦 2. Docker Image – _mi épül fel_

Egy **read-only sablon**:

- nem fut
- rétegekből áll
- újrahasznosítható

👉 Container ebből indul.

---

### 🚀 3. Docker Container – _mi fut_

Egy **élő példány** egy image-ből:

- processzek futnak benne
- van állapota
- ideiglenes

👉 Ha törlöd, az állapot elvész (volume nélkül).

---

### 🧹 4. `.dockerignore` – _mit ne lásson a Docker_

Megmondja:

- mit **ne küldjünk be** buildkor
- gyorsabb build
- tisztább image

👉 Nem futásidős dolog.

---

## 4️⃣ Hogyan áll össze ez a gyakorlatban?

```text
FORRÁSKÓD
   ↓
Dockerfile + .dockerignore
   ↓ docker build
Docker Image
   ↓ docker run
Docker Container (fut)
```

---

## 5️⃣ Rétegek és cache (miért gyors a Docker?)

- Minden Dockerfile sor = réteg
- Rétegek cache-elhetők
- Ha egy réteg változik → alatta minden újraépül

Ezért:

```dockerfile
COPY package.json .
RUN npm install
COPY . .
```

---

## 6️⃣ Multi-stage build (haladó, de fontos)

Két külön szerep:

- **builder**: fordít, csomagol
- **runtime**: csak futtat

Eredmény:

- kisebb image
- nincs felesleges tool
- biztonságosabb

---

## 7️⃣ Dev vs Prod Docker (nem ugyanaz!)

### DEV

- gyors iteráció
- volume mount
- hot reload
- nagyobb image oké

### PROD

- kicsi image
- statikus build
- nincs forráskód
- nincs build tool

👉 Ezt **nem szabad összekeverni**.

---

## 8️⃣ Volume – miért kell?

A container:

- ideiglenes
- törölhető

A volume:

- adatot ment
- túléli a container törlését

👉 adatbázis **SOHA** ne volume nélkül fusson.

---

## 9️⃣ Docker nem Virtual Machine

|Docker|VM|
|---|---|
|közös kernel|saját kernel|
|gyors indulás|lassú|
|könnyű|nehéz|

Docker = app izoláció, nem OS virtualizáció.

---

## 🔟 Mikor érdemes Dockert használni?

✔️ web appok  
✔️ backend szolgáltatások  
✔️ microservice  
✔️ CI/CD  
✔️ tanulás, reprodukció

❌ GUI-heavy desktop app  
❌ kernel-szintű kísérletezés

---

## Gyakori félreértések (összefoglalva)

- „A Docker egy VM” → nem
- „Az image fut” → nem
- „A dockerignore elég” → nem
- „A container adatot tárol” → nem