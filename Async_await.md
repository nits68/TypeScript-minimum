# TypeScript – Promise és async/await

Az aszinkron (nem-blokkoló) programozás lényege, hogy egy hosszabb ideig tartó művelet (pl. fájlolvasás, hálózati kérés) **közben** a program tud mással is foglalkozni, nem áll meg és nem "fagy le". A `Szöveges_állományok.md` jegyzetben már felbukkant az `async`/`await`, itt önállóan, részletesebben nézzük meg.

---

## 1️⃣ A `Promise` (ígéret)

A `Promise` egy olyan objektum, ami egy **jövőben elkészülő** értéket képvisel. Három állapota lehet:

* **pending** (függőben) – még folyamatban van
* **fulfilled** (teljesült) – sikeresen befejeződött, van eredménye
* **rejected** (elutasított) – hiba történt

### Egyszerű Promise létrehozása

```ts
function varakozas(masodperc: number): Promise<string> {
    return new Promise((resolve, reject) => {
        if (masodperc < 0) {
            reject(new Error("A várakozás nem lehet negatív!"));
            return;
        }
        setTimeout(() => {
            resolve(`${masodperc} másodpercet vártunk.`);
        }, masodperc * 1000);
    });
}
```

📌 `resolve(...)` – sikeres befejezés, ez lesz az eredmény<br>
📌 `reject(...)` – hiba jelzése

---

## 2️⃣ A `Promise` kezelése `.then()`/`.catch()`-csel (klasszikus mód)

```ts
varakozas(2)
    .then((eredmeny) => {
        console.log(eredmeny); // "2 másodpercet vártunk."
    })
    .catch((hiba) => {
        console.log("Hiba:", hiba.message);
    });
```

Ez a forma működik, de sok egymásra épülő aszinkron lépésnél nehezen olvasható lánc (ún. "callback / then pokol") alakulhat ki.

---

## 3️⃣ Az `async`/`await` – modern, olvasható forma

Az `await` kulcsszó "megvárja" egy `Promise` eredményét, mintha szinkron kód lenne – miközben a háttérben a program mégsem áll le.

⚠️ Az `await` **csak** `async` kulcsszóval jelölt függvényen belül használható.

```ts
async function fuFolyamat(): Promise<void> {
    try {
        const eredmeny = await varakozas(2);
        console.log(eredmeny); // "2 másodpercet vártunk."
    } catch (hiba) {
        console.log("Hiba:", (hiba as Error).message);
    }
}

fuFolyamat();
```

📌 Egy `async` függvény **mindig** `Promise`-t ad vissza, még akkor is, ha a törzsében egyszerű `return`-t írunk.

```ts
async function ketszerez(szam: number): Promise<number> {
    return szam * 2; // valójában Promise<number>-t ad vissza
}

ketszerez(5).then((eredmeny) => console.log(eredmeny)); // 10
```

---

## 4️⃣ Több aszinkron művelet egymás után

```ts
async function feldolgozasSorban(): Promise<void> {
    console.log("Kezdés...");
    const elso = await varakozas(1);
    console.log(elso);
    const masodik = await varakozas(1);
    console.log(masodik);
    console.log("Kész!");
}

feldolgozasSorban();
```

Ez a két várakozást **egymás után**, sorban végzi el (összesen kb. 2 másodperc).

---

## 5️⃣ Több aszinkron művelet párhuzamosan – `Promise.all()`

Ha a műveletek **nem függenek egymástól**, érdemesebb párhuzamosan futtatni, mert így gyorsabb.

```ts
async function feldolgozasParhuzamosan(): Promise<void> {
    console.log("Kezdés...");
    const [elso, masodik] = await Promise.all([
        varakozas(1),
        varakozas(1)
    ]);
    console.log(elso, masodik);
    console.log("Kész!"); // kb. 1 másodperc alatt (nem 2!)
}

feldolgozasParhuzamosan();
```

---

## 6️⃣ Gyakorlati példa – aszinkron fájlolvasás

```ts
import { promises as fs } from "fs";

async function felhasznaloBetoltese(fajlnev: string): Promise<void> {
    try {
        const tartalom = await fs.readFile(fajlnev, "utf-8");
        const felhasznalo = JSON.parse(tartalom);
        console.log(`Betöltve: ${felhasznalo.nev}`);
    } catch (hiba) {
        console.log("Nem sikerült betölteni a felhasználót:", (hiba as Error).message);
    }
}

felhasznaloBetoltese("user.json");
```

---

## 🧠 Megjegyzés

* `Promise<T>` → "jövőbeli T típusú érték" ígérete
* `async function` → mindig `Promise`-t ad vissza, a törzsében használható `await`
* `await` → megvárja az eredményt, de nem blokkolja a teljes programot
* egymást **követő** lépéseknél → `await` sorban
* **egymástól független**, párhuzamosítható lépéseknél → `Promise.all([...])`
* hibakezelés aszinkron kódnál is `try-catch`-csel történik (lásd [Kivételkezelés.md](Kivételkezelés.md))
