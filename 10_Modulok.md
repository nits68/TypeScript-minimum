# TypeScript – Modulok (import / export)

A modulrendszer teszi lehetővé, hogy a kódot **több fájlba** szervezzük, és a fájlok között megosztjuk a függvényeket, típusokat, osztályokat. React és Next.js projektek gyakorlatilag kizárólag modulokból épülnek fel – minden komponens, hook, típus külön fájlban van, és `import`/`export`-tal kapcsolódnak össze.

---

## 1️⃣ Névre szóló export (named export)

Egy fájlból **több** dolgot is exportálhatunk, mindegyiket a saját nevén.

### `matematika.ts`

```ts
export function osszead(a: number, b: number): number {
    return a + b;
}

export function kivon(a: number, b: number): number {
    return a - b;
}

export const PI: number = 3.14159;
```

### `main.ts` – importálás

```ts
import { osszead, kivon, PI } from "./matematika";

console.log(osszead(2, 3)); // 5
console.log(kivon(5, 2));   // 3
console.log(PI);            // 3.14159
```

📌 A kapcsos zárójelben **pontosan úgy** kell megadni a nevet, ahogy exportálva lett.

---

## 2️⃣ Alapértelmezett export (default export)

Egy fájlból **legfeljebb egy** `default` export lehet. Ez tipikusan akkor jó, ha a fájlnak egyetlen "fő" exportált dolga van (ez a szokásos minta React komponenseknél).

### `Gomb.ts`

```ts
export default function Gomb(szoveg: string): string {
    return `[${szoveg}]`;
}
```

### `main.ts` – importálás (nincs kapcsos zárójel, és a név is szabadon megválasztható)

```ts
import Gomb from "./Gomb";
// import BarmiNev from "./Gomb"; // ez is működne, a default export neve szabadon átnevezhető

console.log(Gomb("Mentés")); // [Mentés]
```

---

## 3️⃣ Named és default export ugyanabból a fájlból

A kettő kombinálható.

```ts
export default function fooComponent(): string {
    return "Fő komponens";
}

export const VERZIO: string = "1.0.0";
export function segedFuggveny(): void {
    console.log("Segéd");
}
```

```ts
import fooComponent, { VERZIO, segedFuggveny } from "./foo";
```

---

## 4️⃣ Átnevezés importáláskor/exportáláskor (`as`)

Ha névütközés lenne, vagy csak olvashatóbb nevet szeretnénk, `as`-szal átnevezhetünk.

```ts
// importáláskor
import { osszead as plusz } from "./matematika";
console.log(plusz(2, 3)); // 5

// exportáláskor
function belsoNev(): void {}
export { belsoNev as nyilvanosNev };
```

---

## 5️⃣ Típusok exportálása/importálása

A `type` és `interface` ugyanúgy exportálható/importálható, mint bármi más.

```ts
// tipusok.ts
export type Felhasznalo = {
    nev: string;
    kor: number;
};
```

```ts
// main.ts
import type { Felhasznalo } from "./tipusok";
// vagy egyszerűen: import { Felhasznalo } from "./tipusok";

const u: Felhasznalo = { nev: "Anna", kor: 25 };
```

📌 Az `import type` explicit jelzi, hogy **csak típust** importálunk (nem futásidejű értéket) – ez segít a fordítónak, és néhány build-eszköznél (pl. Next.js) kifejezetten elvárt konvenció.

---

## 6️⃣ Minden exportált tag begyűjtése egyben (`* as`)

```ts
import * as Matek from "./matematika";

console.log(Matek.osszead(2, 3)); // 5
console.log(Matek.PI);            // 3.14159
```

---

## 7️⃣ Re-export – "gyűjtőfájl" (barrel file) minta

Gyakori minta, hogy egy `index.ts` fájl összegyűjti és továbbadja egy mappa exportjait, hogy máshonnan egyetlen helyről lehessen importálni.

```ts
// components/index.ts
export { default as Gomb } from "./Gomb";
export { default as Kartya } from "./Kartya";
```

```ts
// main.ts
import { Gomb, Kartya } from "./components";
```

---

## 8️⃣ Kitekintés – React/Next.js konvenciók

* React komponenseket jellemzően **default export**-tal exportálnak (`export default function Fejlec() {...}`), mert egy fájl = egy komponens a megszokott minta.
* Segédfüggvényeket, típusokat, konstansokat inkább **named export**-tal exportálnak, mert ezekből több is lehet egy fájlban (`export function formatDatum() {...}`, `export type ApiValasz = {...}`).
* Next.js-ben az `app`/`pages` mappában lévő fájloknak **kötelezően default exportot** kell tartalmazniuk (ez adja az adott oldal/route komponensét).

---

## 🧠 Megjegyzés

* named export → `export function/const/type ...` és `import { nev } from "..."` (kapcsos zárójellel, pontos névvel)
* default export → `export default ...` és `import barmilyenNev from "..."` (nincs kapcsos zárójel, a név szabadon választható)
* egy fájlban **legfeljebb egy** default export lehet, named exportból **több** is
* `import type { ... }` → jelzi, hogy csak típust importálunk, nem futásidejű értéket
