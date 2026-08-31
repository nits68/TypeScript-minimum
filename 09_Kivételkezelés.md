# TypeScript – Kivételkezelés (try-catch-finally)

Futásidőben előfordulhatnak olyan hibák, amiket a fordító nem tud előre kiszűrni (pl. hibás fájlnév, hálózati hiba, hibás felhasználói bemenet). Ezeket a **kivételkezelés** eszközeivel tudjuk lekezelni, hogy a program ne álljon le váratlanul.

---

## 1️⃣ A `try-catch` alapszerkezete

### Alap szintaxis

```ts
try {
    // kód, ami esetleg hibát dobhat
} catch (hiba) {
    // ez fut le, ha a try blokkban hiba történt
}
```

### Példa

```ts
function osztas(a: number, b: number): number {
    if (b === 0) {
        throw new Error("Nullával nem lehet osztani!");
    }
    return a / b;
}

try {
    console.log(osztas(10, 2)); // 5
    console.log(osztas(10, 0)); // ide nem jut el a program
} catch (hiba) {
    console.log("Hiba történt:", (hiba as Error).message);
}
```

📌 A `throw` kulcsszóval **dobhatunk** (jelezhetünk) egy hibát. Ha a `try` blokkban `throw` történik, a vezérlés azonnal a `catch` ágra ugrik.

---

## 2️⃣ A `finally` blokk

A `finally` blokk **mindig lefut** – hibától függetlenül. Jellemzően "takarítási" feladatokra használjuk (pl. erőforrás felszabadítása, fájl bezárása).

```ts
function feldolgozas(szam: number): void {
    try {
        if (szam < 0) {
            throw new Error("Negatív szám nem megengedett!");
        }
        console.log(`Feldolgozva: ${szam}`);
    } catch (hiba) {
        console.log("Hiba:", (hiba as Error).message);
    } finally {
        console.log("Feldolgozás vége (mindenképp lefut).");
    }
}

feldolgozas(5);   // Feldolgozva: 5 / Feldolgozás vége (mindenképp lefut).
feldolgozas(-3);  // Hiba: Negatív szám nem megengedett! / Feldolgozás vége (mindenképp lefut).
```

---

## 3️⃣ A `catch` paraméter típusa (`unknown`)

TypeScriptben a `catch` blokk hibaparamétere alapértelmezetten **`unknown`** típusú (nem `any`), mert **bármit** el lehet dobni JavaScriptben, nem csak `Error` példányt. Emiatt típuskényszerítés (`as`) vagy típusellenőrzés szükséges, mielőtt a hiba tulajdonságait (pl. `.message`) használnánk.

```ts
try {
    throw new Error("Valami elromlott");
} catch (hiba) {
    // hiba típusa: unknown

    if (hiba instanceof Error) {
        console.log(hiba.message); // csak ellenőrzés után biztonságos
    } else {
        console.log("Ismeretlen hiba:", hiba);
    }
}
```

> **Fontos:** a `hiba.message` közvetlen elérése `instanceof Error` ellenőrzés **nélkül** fordítási hibát adna, mert az `unknown` típuson semmilyen tulajdonság nem érhető el ellenőrzés nélkül.

---

## 4️⃣ Saját hibaosztály létrehozása

Nagyobb programoknál hasznos lehet saját, sajátos hibatípusokat létrehozni az `Error` osztályból örökölve.

```ts
class ErvenytelenKorError extends Error {
    constructor(kor: number) {
        super(`Érvénytelen életkor: ${kor}`);
        this.name = "ErvenytelenKorError";
    }
}

function eletkorEllenoriz(kor: number): void {
    if (kor < 0 || kor > 130) {
        throw new ErvenytelenKorError(kor);
    }
    console.log(`Az életkor rendben: ${kor}`);
}

try {
    eletkorEllenoriz(200);
} catch (hiba) {
    if (hiba instanceof ErvenytelenKorError) {
        console.log("Egyedi hiba elkapva:", hiba.message);
    }
}
```

---

## 5️⃣ Beágyazott (nested) try-catch és több hibatípus

Egy `catch`-en belül több hibatípust is meg lehet különböztetni `instanceof`-fal.

```ts
try {
    JSON.parse("{ ez nem érvényes JSON }");
} catch (hiba) {
    if (hiba instanceof SyntaxError) {
        console.log("Hibás JSON formátum:", hiba.message);
    } else if (hiba instanceof Error) {
        console.log("Egyéb hiba:", hiba.message);
    }
}
```

---

## 6️⃣ Kivételkezelés fájlműveleteknél (gyakorlati példa)

```ts
import fs from "fs";

function fajlBiztonsagosOlvasas(nev: string): string {
    try {
        return fs.readFileSync(nev, "utf-8");
    } catch (hiba) {
        console.log(`Nem sikerült beolvasni a(z) "${nev}" fájlt.`);
        return "";
    }
}

const tartalom = fajlBiztonsagosOlvasas("nemletezo.txt");
console.log(tartalom === "" ? "Üres/hibás beolvasás." : tartalom);
```

---

## 🧠 Megjegyzés

* `try` → a "veszélyes" kód
* `catch` → mi történjen hiba esetén (a paraméter típusa `unknown`, ellenőrizni kell mielőtt használjuk)
* `finally` → mindig lefut, hibától függetlenül
* `throw new Error("...")` → hiba dobása
* saját hibaosztályok az `Error`-ból örökölve olvashatóbb, kezelhetőbb hibakezelést tesznek lehetővé
