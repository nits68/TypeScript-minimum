# TypeScript – A `tsconfig.json` alapjai

A `tsconfig.json` fájl mondja meg a TypeScript fordítónak (`tsc`), **hogyan** fordítsa le a `.ts` fájlokat JavaScriptre, és **milyen szigorúan** ellenőrizze a kódot. Ez a fájl a projekt gyökerében helyezkedik el.

---

## 1️⃣ Létrehozása

Ha van telepítve TypeScript (`npm install -g typescript` vagy projekten belül), a következő paranccsal generálható egy alap `tsconfig.json`:

```bash
tsc --init
```

---

## 2️⃣ Minimális, tanuláshoz ajánlott konfiguráció

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*.ts"]
}
```

---

## 3️⃣ A legfontosabb beállítások röviden

### `target`

Milyen JavaScript verzióra fordítson (pl. `ES5`, `ES2020`, `ESNext`). Minél újabb a `target`, annál kevesebb "átalakítást" végez a fordító (pl. az `async/await` natívan megmarad ES2017+ esetén, régebbi cél esetén régi mintára fordítja vissza).

### `module`

Milyen modulrendszert használjon a kimeneti kód (`commonjs` – Node.js klasszikus `require`, vagy `esnext`/`es2020` – modern `import`/`export`).

### `outDir` / `rootDir`

Hova kerüljenek a lefordított `.js` fájlok (`outDir`), és honnan olvassa be a forrás `.ts` fájlokat (`rootDir`). Így a forrás és a fordított kód elkülönül egymástól.

### `strict`

**A legfontosabb kapcsoló.** Bekapcsolja az összes szigorú típusellenőrzési szabályt egyszerre, ezek közül a legfontosabbak:

* `strictNullChecks` – a `null` és `undefined` **csak ott** engedélyezett, ahol a típus expliciten megengedi (`string | null`), különben fordítási hibát kapunk. **Ez a legtöbb "miért nem fordul le a kódom" kérdés forrása kezdőknél.**
* `noImplicitAny` – tilos, hogy egy változó/paraméter típusa hallgatólagosan `any` legyen (mindig meg kell adni a típust, vagy a fordítónak ki kell tudnia következtetnie).
* `strictPropertyInitialization` – osztály mezőit kötelező inicializálni (konstruktorban vagy alapértékkel), különben hiba.

### `esModuleInterop`

Megkönnyíti a régi stílusú (`require`) és a modern (`import`) modulok keverését – enélkül bizonyos `import fs from "fs";` jellegű importok hibát adhatnak.

### `skipLibCheck`

Kihagyja a `node_modules`-ban lévő típusdefiníciós fájlok (`.d.ts`) ellenőrzését – gyorsítja a fordítást, tanuláshoz ajánlott bekapcsolva hagyni.

---

## 4️⃣ Miért fontos a `strict: true`?

### Példa – `strict: false` esetén (engedékeny mód)

```ts
let nev; // implicit "any" típus lenne
nev = "Anna";
nev = 5; // strict módban ez fordítási hibát adna, itt lefut

function udvozol(nev) { // paraméter típusa nélkül is "elfogadja"
    console.log("Szia " + nev);
}
```

### Ugyanez `strict: true` esetén

```ts
let nev; // Hiba: implicit "any" típus (noImplicitAny)

function udvozol(nev) { // Hiba: a paraméternek nincs megadva típusa
    console.log("Szia " + nev);
}
```

📌 **Tanulási szempontból erősen ajánlott mindig `strict: true`-val dolgozni** – így a fordító azonnal jelzi a hibákat, amiket egyébként csak futásidőben (vagy egyáltalán nem) vennénk észre.

---

## 5️⃣ Fordítás és futtatás

```bash
# Egyszeri fordítás (a tsconfig.json alapján)
tsc

# Automatikus újrafordítás mentéskor (watch mód)
tsc --watch

# Lefordított JS futtatása Node.js-szel
node dist/index.js
```

> **Tipp:** Fejlesztés közben kényelmes lehet a `ts-node` csomag, amivel `.ts` fájl közvetlenül futtatható előzetes fordítás nélkül: `npx ts-node src/index.ts`.

---

## 🧠 Megjegyzés

* `tsconfig.json` = a fordító "beállításai" egy adott projekthez
* `strict: true` → mindig kapcsold be, ez tanítja meg a helyes típusgondolkodást
* `strictNullChecks` → a `null`/`undefined`-dal kapcsolatos hibák nagy részét már fordításkor elkapja
* `outDir`/`rootDir` → tartsd külön a forrást (`src`) és a lefordított kódot (`dist`)
