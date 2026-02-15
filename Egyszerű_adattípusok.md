# TypeScript -- Egyszerű (primitív) adattípusok

## 1. number

Szám típus (egész és lebegőpontos is)

``` ts
let kor: number = 18;
let pi: number = 3.14;
```

---

## 2. string

Szöveg típus

``` ts
let nev: string = "Anna";
let uzenet: string = `Szia ${nev}!`; // Template string
```

---

## 3. boolean

Logikai érték (true / false)

``` ts
let aktiv: boolean = true;
let kesz: boolean = false;
```

---

## 4. null

Szándékosan „nincs érték"

``` ts
let adat: null = null;
```

---

## 5. undefined

Nincs inicializálva

``` ts
let valami: undefined = undefined;
```

---

## 6. bigint

Nagy egész számokhoz

``` ts
let nagy: bigint = 9007199254740991n;
```

---


# Az unió típus (nem primitív, de fontos)

## string \| number

``` ts
let azonosito: string | number = 123;
azonosito = "ABC123";
```


A `NaN` (Not-a-Number) a TypeScriptben (és JavaScriptben) egy speciális érték, nem pedig önálló adattípus. Technikailag a `number` típushoz tartozik, de azt jelzi, hogy egy matematikai művelet eredménye nem értelmezhető számként.

Itt van a `NaN` összefoglalója az általad kedvelt formátumban:


# A NaN (Not-a-Number) érték

A `NaN` jelentése **"Nem Szám"**, de ironikus módon a nyelvben ez egy `number` típusú speciális érték. Akkor kapjuk, ha egy matematikai műveletet nem lehet elvégezni.

---

## 1️⃣ Mikor keletkezik NaN?

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

## 2️⃣ A "NaN csapda" (Összehasonlítás)

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

## 3️⃣ Hogyan ellenőrizzük helyesen?

Mivel az `===` nem működik, a beépített segédfüggvényeket kell használni.

### Helyes módszer: `Number.isNaN()`

```ts
let ertek: number = parseInt("Hello");

if (Number.isNaN(ertek)) {
    // ✅ Ez helyesen működik
    console.log("Hiba: Ez nem egy érvényes szám!");
}

```

> **Tipp:** Létezik egy globális `isNaN()` függvény is, de a `Number.isNaN()` szigorúbb és biztonságosabb TypeScriptben.

---

## 4️⃣ Típus info

Bár a neve "Nem Szám", a típusa mégis szám.

```ts
console.log(typeof NaN); // "number"

```

Ezért, ha egy változó típusa `number`, az még nem garancia arra, hogy valódi, használható szám van benne – lehet `NaN` is!

---

## 5️⃣ Hasznos trükk (Alapértelmezett érték)

Ha egy művelet eredménye `NaN` lehet, használhatjuk a `||` (vagy) operátort, hogy helyette 0-t vagy más értéket kapjunk.

```ts
let bemenet = parseInt("korte"); // NaN lenne
let biztosSzam = bemenet || 0;   // Ha NaN (ami fals), akkor 0 lesz

console.log(biztosSzam); // 0

```
