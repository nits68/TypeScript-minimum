Természetesen 🙂
Az alábbi md formátumú jegyzet **ugyanabban a stílusban** készült, mint a mintád.

---

# TypeScript – Szöveges állományok írása és olvasása (Node.js)

> ⚠️ Fontos: Fájlkezeléshez **Node.js környezet** szükséges (böngészőben NEM fut).

---

## 1️⃣ A `fs` modul

A fájlkezeléshez a beépített `fs` (File System) modult használjuk.

### Importálás (modern TypeScript)

```ts
import fs from "fs";
```


---

# 📖 SZINKRON (Blocking) műveletek

A program megvárja, amíg a fájlművelet befejeződik.

---

## 2️⃣ Fájl írása – `writeFileSync()`

### Alap szintaxis

```ts
fs.writeFileSync(fajlnev, tartalom);
```

### Példa

```ts
import fs from "fs";

fs.writeFileSync("adat.txt", "Ez egy teszt szöveg.");
```

📌 Ha a fájl nem létezik → létrejön
📌 Ha létezik → felülírja

---

## 3️⃣ Fájl olvasása – `readFileSync()`

### Alap szintaxis

```ts
fs.readFileSync(fajlnev, encoding);
```

### Példa

```ts
import fs from "fs";

const tartalom: string = fs.readFileSync("adat.txt", "utf-8");
```

📌 Az `"utf-8"` megadása hasznos, különben Buffer típusú objektumot kapunk.

### Megoldás osztály tipikus konstruktor példa
```ts
  constructor(source: string) {
    const dataLines: string[] = fs.readFileSync(source, "utf-8").split("\n").slice(1);
    for (const line of dataLines) {
      const actLine: string = line.trim();
      if (actLine.length > 0) this.#rivers.push(new River(actLine));
    }
  }
```

---

## 4️⃣ Hozzáfűzés fájlhoz – `appendFileSync()`

```ts
fs.appendFileSync("adat.txt", "\nÚj sor hozzáadva.");
```

📌 Nem törli a régi tartalmat, hanem a végére ír.

---

# ⚡ ASZINKRON (Non-blocking) műveletek – Ajánlott

Nem állítják meg a program futását.

---

## 5️⃣ Fájl írása – `writeFile()` (Promise)

```ts
import { promises as fs } from "fs";

async function fajlIras() {
    await fs.writeFile("adat.txt", "Aszinkron írás példa");
    console.log("Kész!");
}

fajlIras();
```

---

## 6️⃣ Fájl olvasása – `readFile()`

```ts
import { promises as fs } from "fs";

async function fajlOlvasas() {
    const adat = await fs.readFile("adat.txt", "utf-8");
    console.log(adat);
}

fajlOlvasas();
```

---

## 7️⃣ Hibakezelés (try–catch)

Fájlműveleteknél **mindig ajánlott**.

```ts
import { promises as fs } from "fs";

async function biztonsagosOlvasas() {
    try {
        const adat = await fs.readFile("nemletezo.txt", "utf-8");
        console.log(adat);
    } catch (error) {
        console.log("Hiba történt:", error);
    }
}

biztonsagosOlvasas();
```

---

## 8️⃣ JSON fájl kezelése

Nagyon gyakori felhasználás.

---

## JSON írás

```ts
import { promises as fs } from "fs";

const user = {
    nev: "Anna",
    kor: 25
};

await fs.writeFile("user.json", JSON.stringify(user, null, 2));
```

📌 `JSON.stringify(obj, null, 2)` → szép formázott JSON

---

## JSON olvasás

```ts
const adat = await fs.readFile("user.json", "utf-8");

const userObj = JSON.parse(adat);

console.log(userObj.nev);
```

---

## 9️⃣ Fájl létezésének ellenőrzése

```ts
import * as fs from "fs";

if (fs.existsSync("adat.txt")) {
    console.log("A fájl létezik.");
}
```

---

## 🔟 Hasznos fájlműveletek

### 10.1 Fájl törlése

```ts
fs.unlinkSync("adat.txt");
```

---

### 10.2 Könyvtár létrehozása

```ts
fs.mkdirSync("ujmappa");
```

---

### 10.3 Könyvtár tartalmának listázása

```ts
const fajlok = fs.readdirSync("./");
console.log(fajlok);
```
