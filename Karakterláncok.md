# TypeScript - Karakterláncok (stringek)

A `string` típus szöveges adatok tárolására szolgál. TypeScriptben (és modern JavaScriptben) a szövegek kezelése rendkívül rugalmas, és számos beépített metódussal rendelkezik.

---

## 1️⃣ Inicializálás (Létrehozás)

Háromféleképpen hozhatunk létre stringet literálok használatával:

1. **Dupla idézőjel** (`"`)
2. **Szimpla idézőjel** (`'`) - nincs funkcionális különbség a kettő között.
3. **Backtick** (`) - Template Literal használatával

### Alap szintaxis

```ts
let vezetekNev: string = "Kovács";
let keresztNev: string = 'János';

```

### Template Literal (Interpoláció)

A leghatékonyabb módszer változók beágyazására. Nem kell `+` jelekkel bajlódni.

```ts
let kor: number = 25;

// Hagyományos (nehezebben olvasható)
let udvozles1: string = "Szia, a nevem " + keresztNev + " és " + kor + " éves vagyok.";

// Modern (Template Literal) - Ajánlott!
let udvozles2: string = `Szia, a nevem ${keresztNev} és ${kor} éves vagyok.`;

```

---

## 2️⃣ Hossz lekérdezése

A `.length` tulajdonság megadja, hány karakterből áll a szöveg (beleértve a szóközöket is).

### Példa

```ts
let jelszo: string = "Titkos123";

console.log(jelszo.length); // 9

```

---

## 3️⃣ Indexelés (Karakterek elérése)

A stringek karakterei 0-tól kezdődően indexeltek.

### Hozzáférés

* `[]` zárójel (tömb-szerű elérés) - Modern és elterjedt.
* `.charAt()` metódus - Régebbi, de biztonságosabb bizonyos szélsőséges esetekben.
* `.at()` metódus - negatív is lehet az index, csak ES2022 (ES13)-tól!

### Példa

```ts
let szo: string = "TypeScript";

console.log(szo[0]);        // "T" (első karakter)
console.log(szo[4]);        // "S"
console.log(szo.charAt(1)); // "y"

// Utolsó karakter elérése
console.log(szo[szo.length - 1]); // "t"

```

### 🔴 Fontos: Immutability (Megváltoztathatatlanság)

A stringek TypeScriptben (és JS-ben) **nem módosíthatók** index alapján. Ha meg akarsz változtatni egy karaktert, új stringet kell létrehoznod.

```ts
let str = "alma";
// str[0] = "b"; // HIBÁS! Ez nem működik, nem dob hibát, de nem is változtat.

str = "b" + str.slice(1); // Helyes: új értéket adunk a változónak ("blma")

```

---

## 4️⃣ Keresés a szövegben

Gyakran kell vizsgálni, hogy egy szöveg tartalmaz-e egy részletet.

### Metódusok

* `.includes()`: Tartalmazza-e? (true/false)
* `.startsWith()`: Ezzel kezdődik? (true/false)
* `.endsWith()`: Ezzel végződik? (true/false)
* `.indexOf()`: Hányadik indexen kezdődik? (-1, ha nincs benne)
* `.lastIndexOf()`: Utolsó előfordulás indexe (-1, ha nincs benne)

### Példa

```ts
let mondat: string = "A TypeScript egy nagyszerű nyelv.";

console.log(mondat.includes("Script")); // true
console.log(mondat.startsWith("A"));    // true
console.log(mondat.endsWith("."));      // true

console.log(mondat.indexOf("nagy"));    // 17
console.log(mondat.indexOf("Java"));    // -1 (nincs benne)

```

---

## 5️⃣ Szöveg átalakítása (Kisbetű / Nagybetű)

Mivel a stringek összehasonlítása (pl. jelszavak, felhasználónevek) **érzékeny a kis- és nagybetűkre** (Case Sensitive), gyakran közös alakra hozzuk őket.

### Példa

```ts
let eredeti: string = "Hello World";

console.log(eredeti.toUpperCase()); // "HELLO WORLD"
console.log(eredeti.toLowerCase()); // "hello world"

// Gyakori használat összehasonlításnál:
let bemenet = "Admin";
if (bemenet.toLowerCase() === "admin") {
    console.log("Belépés engedélyezve.");
}

```

---

## 6️⃣ Darabolás és vágás

### `.slice(start, end)` - A legjobb vágó metódus

Kivesz egy részt a szövegből. Az `end` indexű karakter már nincs benne.

```ts
let fajl: string = "kep_2024.png";

// Az első 3 karakter
console.log(fajl.slice(0, 3)); // "kep"

// A 4. karaktertől a végéig
console.log(fajl.slice(4));    // "2024.png"

// Hátulról számolva (az utolsó 4 karakter)
console.log(fajl.slice(-4));   // ".png"

```

### `.split(elválasztó)` - Szöveg tömbbé alakítása

Nagyon hasznos, ha CSV adatokat vagy mondatokat kell feldolgozni.

```ts
let adatok: string = "alma,körte,szilva";
let tomb: string[] = adatok.split(",");

console.log(tomb); // ["alma", "körte", "szilva"]
console.log(tomb[1]); // "körte"

```

---

## 7️⃣ Tisztítás és Csere

### `.trim()` - Whitespace eltávolítás

Levágja a szóközöket, tabulátorokat és sortöréseket a szöveg **elejéről és végéről**. Űrlapok feldolgozásánál kötelező!

```ts
let email: string = "   user@example.com   ";

console.log(email.trim()); // "user@example.com"

```

### `.replace()` és `.replaceAll()`

Kicserél egy szövegrészletet egy másikra.

```ts
let szoveg: string = "A kék autó gyors. A kék szín szép.";

// Csak az ELSŐ találatot cseréli
console.log(szoveg.replace("kék", "piros")); 
// "A piros autó gyors. A kék szín szép."

// Az ÖSSZES találatot cseréli (ES2021+)
console.log(szoveg.replaceAll("kék", "piros")); 
// "A piros autó gyors. A piros szín szép."

```
