---
title: Dockerfile
tags:
  - informatika
  - docker
  - dockerfile
  - programming
  - programozás
  - dev
status: complete
created: 2026-01-17
---

# Mi az a Dockerfile?

A **Dockerfile** egy szöveges fájl, ami **receptként** leírja, hogyan épül fel egy Docker **image**.

- **build időben** fut (amikor image-et építesz)
- **nem** konténer futás közben


> [!info]  
> **Mentális modell**
> 
> - **Dockerfile = recept**
>     
> - **Image = kész étel**
>     
> - **Container = futó alkalmazás**
>     

Kapcsolódó:

- [[Docker image]]
- [[Docker container]]
- [[dockerignore]]

---

## Miért létezik?

- **Reprodukálhatóság**: ugyanaz az image, ugyanúgy épül fel bárhol.
- **Automatizálás**: nem kézzel telepítgetsz gépenként.
- **Verziózhatóság**: a build folyamat is kóddá válik.

---

## Hogyan működik?

A Docker a Dockerfile sorait **fentről lefelé** végrehajtja. Minden lépésből általában **layer** lesz.

> [!tip]  
> **Cache logika**: ha egy korábbi layer változik, az utána következő lépések cache-e eldobódik.  
> Ezért szokás előbb a `package*.json`-t másolni, csak utána a teljes forrást.

---

# Alap utasítások (instructions)

## `FROM`

**Alap image** kiválasztása. Ez szinte mindig kötelező (kivéve `scratch`).

```dockerfile
FROM node:20-alpine //npm hez pl react, nextjs
 VAGY
FROM php:8.3-fpm //laravelhez
```

---

## `WORKDIR`

Beállítja a munkakönyvtárat a konténeren belül.

```dockerfile
WORKDIR /app

VAGY
WORKDIR /var/www/html  //laravelnél
```

Minden `RUN`, `COPY`, `CMD` innen „számol”.

---

## `COPY`

Fájlok másolása a hostról az image-be.

```dockerfile
COPY package.json .
COPY . .
```

> [!note]  
> **Preferáld a `COPY`-t az `ADD` helyett.**  
> `ADD` csak speciális esetben kell (pl. automatikus tar kicsomagolás vagy URL-es letöltés, de ezeket is jobb általában explicit módon kezelni).

---

## `RUN`

Build időben lefutó parancs (pl. csomagtelepítés, build).

```dockerfile
RUN npm install
```

> [!warning]  
> Minden `RUN` általában **új layer**.  
> Ez jó cache-hez, de ha túl sok apró `RUN` van, feleslegesen nőhet az image.

---

## `CMD`

A konténer **alapértelmezett indítóparancsa**.

```dockerfile
CMD ["npm", "run", "start"]
```

- könnyen felülírható `docker run ... <parancs>`-sal
    

---

## `ENTRYPOINT`

„Ez maga az alkalmazás” – fixebb belépési pont.

```dockerfile
ENTRYPOINT ["node", "server.js"]
```

> [!warning]  
> Nehezebb felülírni, ezért **ritkábban** használd. Akkor jó, ha tényleg mindig ugyanazt kell futtatni.

---

## `EXPOSE`

**Dokumentációs jellegű** port jelzés.

```dockerfile
EXPOSE 3000
```

> [!important]  
> Nem nyit portot. Csak jelzi, hogy az app ezen a porton „szokott” figyelni.

---

## `ENV`

Futásidő környezeti változó.

```dockerfile
ENV NODE_ENV=production
```

---

## `ARG`

Build időben használható változó.

```dockerfile
ARG NODE_VERSION=20
```

> [!warning]  
> Futáskor már **nem** létezik (a konténerben nem elérhető úgy, mint az `ENV`).

---

## `USER`

Ne rootként fusson az app.

```dockerfile
USER node
```

> [!tip]  
> Biztonsági best practice: ha az app kompromittálódik, kisebb kárt tud csinálni.

---

## `HEALTHCHECK`

A konténer „egészségi állapotát” ellenőrzi.

```dockerfile
HEALTHCHECK CMD curl -f http://localhost:3000 || exit 1
```

---

# Teljes alap példa (Node.js)

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

---

# Multi-stage build (haladó, de nagyon hasznos)

**Cél:**

- kisebb végső image
- a build eszközök ne kerüljenek bele a runtime image-be

```dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm ci
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/main.js"]
```

> [!tip]  
> A `builder` stage-ben lehet minden nehéz build-tool, a végső stage-ben csak a futtatáshoz kellő cucc marad.

---

# `.dockerignore` (gyakorlatilag kötelező)

Olyan, mint a `.gitignore`, csak Docker buildhez: ami itt benne van, azt **nem** küldi be a Docker a build contextbe.

```dockerignore
node_modules
.git
.env
Dockerfile
```

> [!important]  
> Ez gyorsítja a buildet és csökkenti a véletlen „szemetet” az image-ben.

---

## Gyakori félreértések

- **„A Dockerfile futás közben fut.”** → nem, az image építésekor fut.
- **„Az EXPOSE portot nyit.”** → nem, csak jelöl.
- **„ARG és ENV ugyanaz.”** → nem: `ARG` buildhez, `ENV` futáshoz.
