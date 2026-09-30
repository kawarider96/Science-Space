
| Típus       | Mit jelent        | Mikor használd                            | Tipikus hiba                                | Megjegyzés                                              |
| ----------- | ----------------- | ----------------------------------------- | ------------------------------------------- | ------------------------------------------------------- |
| `string`    | Szöveg            | Nevek, címek, ID-k, UI szöveg             | Számot vagy boolean-t próbálsz belerakni    | Nem „idézőjeles cucc”, hanem **szövegként kezelt adat** |
| `number`    | Szám              | Számolás, ár, életkor                     | `string`-ként kapott számot nem konvertálsz | Nincs `int` / `float`, minden number                    |
| `boolean`   | Igaz / hamis      | Flag-ek, állapotok                        | Truthy/falsy értékek használata             | Csak `true` vagy `false`                                |
| `undefined` | Nincs érték (még) | Inicializálatlan változó, opcionális mező | Direkt beállítod, pedig nem kéne            | Általában a JS adja                                     |
| `null`      | Szándékosan üres  | „Van mező, de nincs adat”                 | `undefined`-del kevered                     | Tudatos döntés                                          |
| `any`       | Bármi             | 🔥 **szinte soha**                        | Kikapcsolod a TS-t                          | Olyan, mintha JS-t írnál                                |
