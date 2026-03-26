# TypeScript -- Egyszerű (primitív) adattípusok és literálok

## 1️⃣. Numerikus típus (number)

Numerikus típus (egész és lebegőpontos is)<br>
A JS/TS minden számot lebegőpontos módszerrel tárol (pazarlás)

``` ts
let kor: number = 18;
let pi: number = 3.14; // 3.14 -> numerikus literál
```

---

## 2️⃣. Szöveges típus (string)

Szöveges típus karakterek és karakterláncok tárolására

``` ts
let nev: string = "Anna";
let uzenet: string = `Szia ${nev}!`; // Template string literál
```

---

## 3️⃣. Logikai típus (boolean)

Logikai típus (lehetséges érték **true**, vagy **false**)

``` ts
let aktiv: boolean = true;
let kesz: boolean = false;
```


---

## 4️⃣. Union Type (Unió típus)

Akkor használjuk, ha egy változó különböző típusú értékeket is felvehet.

### Példa

```ts
let azonosito: string | number;

azonosito = 123;      // OK
azonosito = "A-123";  // OK
azonosito = true;  // HIBA

```

---

## 5️⃣. null (típus és érték is)

A nincs még referencia (összetett típusoknál a referencia a memóraicímet jelenti, ahol az adatokat tároljuk) állapot jelzésére.

``` ts
const player: Player | null = null;
```

---

## 6️⃣. undefined (típus és érték is)

Nincs inicializálva állapot jelzésére.

``` ts
let valami: szám | undefined = undefined;
```

---

## 7️⃣. bigint

Nagy egész számokhoz

``` ts
let nagy: bigint = 9007199254740991n;
```




## 8️⃣. A NaN (Not-a-Number) speciális érték (nem típus, a NaN típusa: number)

A `NaN` jelentése **"Nem Szám"**, de ironikus módon a JS/TS-ben ez egy `number` típusú speciális érték.<br>
Akkor kapjuk, ha egy matematikai műveletet nem lehet elvégezni.

---

### 8.1 Mikor keletkezik NaN?

Tipikus esetek, amikor `NaN` lesz az eredmény:

```ts
// 1. Szöveg számmá alakítása sikertelen
let szam: number = parseInt("alma"); // NaN

// 2. Matematikai művelet nem számmal
let eredmeny = 5 * "szoveg"; // TypeScriptben ez hiba, de futásidőben NaN

// 3. 0 osztása 0-val
let osztas: number = 0 / 0; // NaN (bár sima szám/0 az Infinity!)

// 4. Negatív szám gyöke (valós számok körében)
let gyok: number = Math.sqrt(-1); // NaN

```

---

### 8.2 A "NaN csapda" (Összehasonlítás)

A `NaN` a programozás egyik legfurcsább értéke: **nem egyenlő saját magával sem!**

Ezért **TILOS** így ellenőrizni:

```ts
let ertek: number = NaN;

if (ertek === NaN) { 
    // ❌ EZ SOHA NEM FUT LE!
    console.log("Ez nem szám.");
}

```

---

### 8.3 Hogyan ellenőrizzük helyesen?

Mivel az `===` nem működik, a beépített segédfüggvényeket kell használni.

### Helyes módszer: `Number.isNaN()`

```ts
let ertek: number = parseInt("Hello");

if (Number.isNaN(ertek)) {
    // ✅ Ez helyesen működik
    console.log("Hiba: Ez nem egy érvényes szám!");
}

```

> **Tipp:** Létezik egy globális `isNaN()` függvény is, de a `Number.isNaN()` szigorúbb és biztonságosabb TS-ben.

---

## 9️⃣ Típus info - typeof operátor

Bár a neve "Nem Szám", a típusa mégis szám.

```ts
console.log(typeof NaN); // "number"

```

Ezért, ha egy változó típusa `number`, az még nem garancia arra, hogy valódi, használható szám van benne – lehet `NaN` is!

---

## 🔟 Hasznos trükk (Alapértelmezett érték)

Ha egy művelet eredménye `NaN` lehet, használhatjuk a `||` (OR) operátort, vagy a `??` (nulla összevonás) operátort, hogy helyette 0-t vagy más értéket kapjunk.

```ts
let bemenet = parseInt("korte"); // NaN lenne
let biztosSzam = bemenet || 0;   // Ha NaN (ami fals), akkor 0 lesz

console.log(biztosSzam); // 0

```
