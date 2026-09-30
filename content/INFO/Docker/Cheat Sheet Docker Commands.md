
## **Docker alap parancsok**

| Mire jó?             | Parancs             | Megjegyzés                     |
| -------------------- | ------------------- | ------------------------------ |
| Docker verzió        | `docker --version`  | Telepített verzió              |
| Docker infók         | `docker info`       | Rendszer / engine infók        |
| Segítség             | `docker help`       | Általános help                 |
| Konkrét parancs help | `docker run --help` | Például `run`, `build`, `logs` |

## Image-ek
|Mire jó?|Parancs|Példa|
|---|---|---|
|Image letöltése|`docker pull <image>`|`docker pull nginx`|
|Konkrét verzió letöltése|`docker pull <image>:<tag>`|`docker pull mysql:8`|
|Image-ek listázása|`docker images`||
|Image törlése|`docker rmi <image>`|`docker rmi nginx`|
|Nem használt image-ek törlése|`docker image prune`|Óvatosan|
|Minden nem használt image törlése|`docker image prune -a`|Durvább takarítás|

## Container indítás
| Mire jó?                        | Parancs                                    | Példa                                            |
| ------------------------------- | ------------------------------------------ | ------------------------------------------------ |
| Container indítása              | `docker run <image>`                       | `docker run nginx`                               |
| Háttérben indítás               | `docker run -d <image>`                    | `docker run -d nginx`                            |
| Név adása                       | `docker run --name <név> <image>`          | `docker run --name my-nginx nginx`               |
| Port mapping                    | `docker run -p <host>:<container> <image>` | `docker run -p 8080:80 nginx`                    |
| Automatikus törlés kilépés után | `docker run --rm <image>`                  | `docker run --rm nginx`                          |
| Interaktív terminál             | `docker run -it <image> bash`              | `docker run -it ubuntu bash`                     |
| Munkakönyvtár beállítása        | `docker run -w <path> <image>`             | `docker run -w /app node:20`                     |
| Env változó átadása             | `docker run -e KEY=value <image>`          | `docker run -e MYSQL_ROOT_PASSWORD=root mysql:8` |

## Container kezelés

| Mire jó?                             | Parancs                      | Megjegyzés           |
| ------------------------------------ | ---------------------------- | -------------------- |
| Futó container-ek                    | `docker ps`                  | Csak aktívak         |
| Összes container                     | `docker ps -a`               | Leállítottak is      |
| Container leállítása                 | `docker stop <container>`    | Név vagy ID          |
| Container indítása                   | `docker start <container>`   | Már létező container |
| Container újraindítása               | `docker restart <container>` | Stop + start         |
| Container törlése                    | `docker rm <container>`      | Csak leállítottat    |
| Futó container kényszerített törlése | `docker rm -f <container>`   | Brutál, de hasznos   |
| Container átnevezése                 | `docker rename <régi> <új>`  |                      |

## Belépés containerbe
| Mire jó?                       | Parancs                             | Példa                                 |
| ------------------------------ | ----------------------------------- | ------------------------------------- |
| Bash shell nyitása             | `docker exec -it <container> bash`  | `docker exec -it app bash`            |
| Sh shell nyitása               | `docker exec -it <container> sh`    | Alpine image-eknél gyakori            |
| Parancs futtatása containerben | `docker exec <container> <parancs>` | `docker exec app php artisan migrate` |

## Logok és diagnosztika
| Mire jó?                     | Parancs                              | Megjegyzés     |
| ---------------------------- | ------------------------------------ | -------------- |
| Logok megtekintése           | `docker logs <container>`            |                |
| Logok követése élőben        | `docker logs -f <container>`         | Nagyon gyakori |
| Utolsó 100 sor               | `docker logs --tail 100 <container>` |                |
| Container részletes infó     | `docker inspect <container>`         | JSON-ben       |
| Erőforrás használat          | `docker stats`                       | CPU, RAM       |
| Futó folyamatok containerben | `docker top <container>`             |                |
| Portok listázása             | `docker port <container>`            |                |

## Dockerfile / Build
| Mire jó?                  | Parancs                              | Példa                                      |
| ------------------------- | ------------------------------------ | ------------------------------------------ |
| Image build               | `docker build -t <név> .`            | `docker build -t my-app .`                 |
| Image build taggel        | `docker build -t <név>:<tag> .`      | `docker build -t my-app:dev .`             |
| Build cache nélkül        | `docker build --no-cache -t <név> .` |                                            |
| Más Dockerfile használata | `docker build -f <file> -t <név> .`  | `docker build -f Dockerfile.prod -t app .` |
## Volume-ok
| Mire jó?                               | Parancs                                           | Példa                                             |
| -------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| Volume-ok listázása                    | `docker volume ls`                                |                                                   |
| Volume létrehozása                     | `docker volume create <név>`                      | `docker volume create mysql_data`                 |
| Volume törlése                         | `docker volume rm <név>`                          |                                                   |
| Nem használt volume-ok törlése         | `docker volume prune`                             | Vigyázat, adatvesztés                             |
| Volume mount                           | `docker run -v <volume>:<container_path> <image>` | `docker run -v mysql_data:/var/lib/mysql mysql:8` |
| Lokális mappa mount Windows PowerShell | `docker run -v ${PWD}:/app <image>`               | `docker run -v ${PWD}:/app node:20`               |
| Lokális mappa mount Linux/macOS        | `docker run -v $(pwd):/app <image>`               | `docker run -v $(pwd):/app node:20`               |

## Network
| Mire jó?                       | Parancs                                  | Példa                                |
| ------------------------------ | ---------------------------------------- | ------------------------------------ |
| Networkök listázása            | `docker network ls`                      |                                      |
| Network létrehozása            | `docker network create <név>`            | `docker network create app-net`      |
| Container indítása networkön   | `docker run --network <network> <image>` | `docker run --network app-net nginx` |
| Network törlése                | `docker network rm <név>`                |                                      |
| Nem használt networkök törlése | `docker network prune`                   |                                      |

## Fájlmásolás
| Mire jó?            | Parancs                                    | Példa                                    |
| ------------------- | ------------------------------------------ | ---------------------------------------- |
| Hostból containerbe | `docker cp <host_path> <container>:<path>` | `docker cp ./test.txt app:/app/test.txt` |
| Containerből hostra | `docker cp <container>:<path> <host_path>` | `docker cp app:/app/log.txt ./log.txt`   |

## Docker Compose
| Mire jó?                   | Parancs                              | Megjegyzés                       |
| -------------------------- | ------------------------------------ | -------------------------------- |
| Compose indítás            | `docker compose up`                  | Előtérben                        |
| Compose indítás háttérben  | `docker compose up -d`               | Ez a leggyakoribb                |
| Újrabuildeléssel indítás   | `docker compose up --build`          | Dockerfile változás után         |
| Leállítás                  | `docker compose down`                | Container-ek törlése             |
| Leállítás volume törléssel | `docker compose down -v`             | Adatbázist is törölhet, ne vakon |
| Compose logok              | `docker compose logs`                |                                  |
| Compose logok élőben       | `docker compose logs -f`             |                                  |
| Egy service logja          | `docker compose logs -f <service>`   | `docker compose logs -f app`     |
| Service shell              | `docker compose exec <service> bash` | `docker compose exec app bash`   |
| Service újraindítása       | `docker compose restart <service>`   | `docker compose restart app`     |
| Service-ek listázása       | `docker compose ps`                  |                                  |
## Takarítás
| Mire jó?                        | Parancs                            | Veszélyesség                |
| ------------------------------- | ---------------------------------- | --------------------------- |
| Leállított container-ek törlése | `docker container prune`           | Közepes                     |
| Nem használt image-ek törlése   | `docker image prune`               | Alacsony-közepes            |
| Nem használt volume-ok törlése  | `docker volume prune`              | Magasabb, adatvesztés lehet |
| Nem használt networkök törlése  | `docker network prune`             | Alacsony                    |
| Általános takarítás             | `docker system prune`              | Közepes                     |
| Brutál takarítás                | `docker system prune -a --volumes` | Docker atombomba            |

## Gyakori fejlesztői példák
| Cél                           | Parancs                                                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------------------------------- |
| Nginx 8080-on                 | `docker run -d --name nginx-dev -p 8080:80 nginx`                                                         |
| MySQL dev adatbázis           | `docker run -d --name mysql-dev -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=app -p 3306:3306 mysql:8`   |
| PostgreSQL dev adatbázis      | `docker run -d --name postgres-dev -e POSTGRES_PASSWORD=root -e POSTGRES_DB=app -p 5432:5432 postgres:16` |
| Redis                         | `docker run -d --name redis-dev -p 6379:6379 redis`                                                       |
| Node projektben `npm install` | `docker run --rm -it -v ${PWD}:/app -w /app node:20 npm install`                                          |
| Node dev szerver              | `docker run --rm -it -v ${PWD}:/app -w /app -p 3000:3000 node:20 npm run dev`                             |
| Composer install              | `docker run --rm -it -v ${PWD}:/app -w /app composer install`                                             |

## A legfontosabb parancsok, amit tényleg tudj
| Parancs                            | Mire jó?            |
| ---------------------------------- | ------------------- |
| `docker ps`                        | Futó container-ek   |
| `docker ps -a`                     | Összes container    |
| `docker images`                    | Image-ek            |
| `docker run -d -p 8080:80 nginx`   | Nginx indítása      |
| `docker exec -it <container> bash` | Belépés containerbe |
| `docker logs -f <container>`       | Log nézés           |
| `docker stop <container>`          | Leállítás           |
| `docker rm <container>`            | Törlés              |
| `docker build -t app .`            | Image build         |
| `docker compose up -d`             | Compose indítás     |
| `docker compose down`              | Compose leállítás   |
| `docker compose logs -f`           | Compose logok       |
| `docker system prune`              | Takarítás           |