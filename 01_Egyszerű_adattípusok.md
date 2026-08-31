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

A `null` egy **tudatosan, explicit módon** beállított "üres" érték – azt fejezi ki, hogy "itt szándékosan nincs semmi", pl. mert egy objektum-hivatkozás (referencia) még nem mutat semmilyen valós adatra. (Összetett típusoknál a referencia a memóriacímet jelenti, ahol az adatokat tároljuk.)

``` ts
interface Player {
    nev: string;
}

let player: Player | null = null; // Szándékosan üres: még nincs kiválasztva játékos

// Később, amikor tényleg lesz adat:
player = { nev: "Kovács Anna" };
```

📌 A `null`-t **mi magunk** adjuk értékül – ez a lényegi különbség az `undefined`-tól (lásd lent).

### `strictNullChecks` – miért írjuk ki mindig a union típust?

Alapértelmezett (`strict: true`) TypeScript beállítás mellett egy `Player` típusú változónak **nem** adható `null` érték, csak ha ezt a típus explicit módon megengedi (`Player | null`). Enélkül a fordító már a deklarációnál hibát jelezne:

```ts
let jatekos: Player = null; // HIBA strict módban! (lásd 18_Tsconfig_alapok.md)
let jatekos2: Player | null = null; // OK, mert a típus megengedi
```

### Ellenőrzés null-ra

```ts
if (player === null) {
    console.log("Még nincs kiválasztva játékos.");
} else {
    console.log(`A játékos neve: ${player.nev}`);
}
```

---

## 6️⃣. undefined (típus és érték is)

Az `undefined` azt jelzi, hogy egy változó **létezik ugyan, de még nem kapott értéket** – ez a JavaScript/TypeScript **alapértelmezett**, automatikusan adott állapota, nem mi állítjuk be tudatosan (bár megtehetjük).

``` ts
let valami: number | undefined;   // Nincs kezdőérték megadva -> automatikusan undefined
console.log(valami);              // undefined

let masik: number | undefined = undefined; // Explicit módon is beállítható
```

### Tipikus előfordulási helyek

```ts
// 1. Deklarált, de még nem inicializált változó
let eredmeny: string | undefined;
console.log(eredmeny); // undefined

// 2. Nem létező objektum-tulajdonság elérése
type Auto = { marka: string; szin?: string }; // a szin opcionális
let kocsi: Auto = { marka: "Toyota" };
console.log(kocsi.szin); // undefined (a tulajdonság nincs megadva)

// 3. Függvény, aminek nincs return utasítása (visszatérési típusa void, az érték undefined)
function logol(): void {
    console.log("Csak kiír valamit.");
}
console.log(logol()); // undefined

// 4. Hiányzó függvényargumentum
function koszont(nev: string, cim?: string) {
    console.log(cim); // undefined, ha a hívó nem adta meg
}
koszont("Béla");
```

---

## 7️⃣. `null` vs. `undefined` – mikor melyiket használjuk?

| Szempont | `null` | `undefined` |
|---|---|---|
| Ki állítja be? | A **programozó**, tudatosan | A **JS/TS motor**, automatikusan |
| Jelentése | "Szándékosan nincs érték/referencia" | "Még nincs érték, nincs inicializálva" |
| Tipikus helyzet | Egy objektum-referencia még nem mutat semmire (pl. `player`, `kivalasztottElem`) | Deklarált, de nem inicializált változó; hiányzó opcionális mező/paraméter; `void` függvény visszatérése |
| `typeof` eredménye | `"object"` ⚠️ (történelmi JS-hiba, lásd lent) | `"undefined"` |

### ⚠️ A `typeof null` csapdája

```ts
console.log(typeof null);      // "object"  <- ez egy régi, javítatlanul hagyott JS-hiba!
console.log(typeof undefined); // "undefined"
```

A `typeof null === "object"` history-hiba a JavaScript első verziójából ered, és a visszafelé kompatibilitás miatt máig sem javították ki. **Emiatt a `typeof` NEM alkalmas `null` ellenőrzésére** – helyette mindig a szigorú egyenlőség (`=== null`) használandó.

### Egyenlőség-vizsgálat: `==` vs `===`

```ts
console.log(null == undefined);   // true  (a == laza összehasonlítás egyenértékűnek veszi őket)
console.log(null === undefined);  // false (a === szigorú összehasonlítás megkülönbözteti a típusukat is)
```

📌 Éppen ezért kényelmes és biztonságos a **`??` (nullish coalescing)** operátor, mert egyetlen feltétellel kezeli mindkét esetet – lásd [02_Operátorok.md](02_Operátorok.md) 7.2-es pontját:

```ts
let ertek1: number | null = null;
let ertek2: number | undefined = undefined;

console.log(ertek1 ?? 0); // 0 (null esetén is működik)
console.log(ertek2 ?? 0); // 0 (undefined esetén is működik)
```

---

## 8️⃣. bigint

Nagy egész számokhoz

``` ts
let nagy: bigint = 9007199254740991n;
```




## 9️⃣. A NaN (Not-a-Number) speciális érték (nem típus, a NaN típusa: number)

A `NaN` jelentése **"Nem Szám"**, de ironikus módon a JS/TS-ben ez egy `number` típusú speciális érték.<br>
Akkor kapjuk, ha egy matematikai műveletet nem lehet elvégezni.

---

### 9.1 Mikor keletkezik NaN?

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

### 9.2 A "NaN csapda" (Összehasonlítás)

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

### 9.3 Hogyan ellenőrizzük helyesen?

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

## 🔟 Típus info - typeof operátor

Bár a neve "Nem Szám", a típusa mégis szám.

```ts
console.log(typeof NaN); // "number"

```

Ezért, ha egy változó típusa `number`, az még nem garancia arra, hogy valódi, használható szám van benne – lehet `NaN` is!

---

## 1️⃣1️⃣ Hasznos trükk (Alapértelmezett érték)

Ha egy művelet eredménye `NaN` lehet, használhatjuk a `||` (OR) operátort, vagy a `??` (nulla összevonás) operátort, hogy helyette 0-t vagy más értéket kapjunk.

```ts
let bemenet = parseInt("korte"); // NaN lenne
let biztosSzam = bemenet || 0;   // Ha NaN (ami fals), akkor 0 lesz

console.log(biztosSzam); // 0

```
