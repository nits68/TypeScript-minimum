# TypeScript – Operátorok (Operators)

Az operátorok segítségével műveleteket végezhetünk változókon és értékeken.

---

## 1️⃣ Aritmetikai operátorok (Matematikai műveletek)

### Operátorok

```ts
+   // összeadás
-   // kivonás
*   // szorzás
/   // osztás
%   // maradék (modulo)
**  // hatványozás
```

### Példa

```ts
let a: number = 10;
let b: number = 3;

console.log(a + b);  // 13
console.log(a - b);  // 7
console.log(a * b);  // 30
console.log(a / b);  // 3.333...
console.log(a % b);  // 1
console.log(a ** b); // 1000
```

---

## 2️⃣ Értékadó operátorok (Assignment)

### Operátorok

```ts
=    // értékadás
+=   // növelés és értékadás
-=   // csökkentés és értékadás
*=   // szorzás és értékadás
/=   // osztás és értékadás
%=   // maradék és értékadás
```

### Példa

```ts
let x: number = 5;

x += 3; // x = x + 3
console.log(x); // 8

x *= 2; // x = x * 2
console.log(x); // 16
```

---

## 3️⃣ Összehasonlító operátorok (Comparison)

### Operátorok

```ts
==   // egyenlő (érték szerint)
===  // szigorúan egyenlő (érték + típus)
!=   // nem egyenlő
!==  // szigorúan nem egyenlő
>    // nagyobb
<    // kisebb
>=   // nagyobb vagy egyenlő
<=   // kisebb vagy egyenlő
```

### Példa

```ts
let a: number = 5;
let b: number = 10;

console.log(a == "5");   // true (nem ajánlott)
console.log(a === 5);    // true
console.log(a === "5");  // false

console.log(a < b);      // true
console.log(a >= b);     // false
```

---

## 4️⃣ Logikai operátorok (Logical)

### Operátorok

```ts
&&  // ÉS (AND)
||  // VAGY (OR)
!   // NEM (NOT)
```

### Példa

```ts
let vanJegy: boolean = true;
let vanDiak: boolean = false;

console.log(vanJegy && vanDiak); // false
console.log(vanJegy || vanDiak); // true
console.log(!vanJegy);           // false
```

---

## 5️⃣ Inkrementáló és dekrementáló operátorok

### Operátorok

```ts
++  // növelés 1-gyel
--  // csökkentés 1-gyel
```

### Példa

```ts
let i: number = 5;

i++; // i = i + 1
console.log(i); // 6

i--; // i = i - 1
console.log(i); // 5
```

---

## 6️⃣ Ternáris operátor (feltételes)

Rövid if-else szerkezet.

### Szintaxis

```ts
feltétel ? érték1 : érték2
```

### Példa

```ts
let kor: number = 18;

let uzenet: string = kor >= 18 ? "Felnőtt" : "Kiskorú";

console.log(uzenet);
```


## 7️⃣ Speciális operátorok

### 7.1 A dupla tagadás (`!!`) operátor

Ha bármilyen értéket valódi `boolean` típussá (true/false) akarsz alakítani, használd a `!!` jelet.

```ts
console.log(!!"hello"); // true
console.log(!!0);       // false
console.log(!!{});      // true
```

### 7.2 Nullás összevonás `??` (Nullish Coalescing)

Hasonlít a `||` (VAGY) operátorhoz, de **sokkal biztonságosabb**.

* A `||` operátor lecseréli a `0`-t és az üres szöveget `""` is, mert azok "hamisnak" számítanak.
* A `??` **CSAK** akkor adja vissza a jobb oldali értéket, ha a bal oldali `null` vagy `undefined`.

**Példa:**

```ts
let pontszam: number | null = 0; // A játékosnak 0 pontja van
let nev: string | undefined = undefined; // Nincs neve

// Hagyományos VAGY (||) - HIBA!
let eredmeny1 = pontszam || 10; 
console.log(eredmeny1); // 10 (Hibás, felülírta a valós 0 értéket!)

// Modern Nullish Coalescing (??) - HELYES!
let eredmeny2 = pontszam ?? 10; 
console.log(eredmeny2); // 0 (Megtartotta a 0-t, mert az nem null/undefined)

// Név esetén
let felhasznalo = nev ?? "Vendég";
console.log(felhasznalo); // "Vendég" (Mivel a név undefined volt)

```

---

### 7.3 Opcionális láncolás `?.` (Optional Chaining)

Segítségével úgy érhetünk el egy objektum mélyén lévő tulajdonságot, hogy nem kell ellenőrizni minden szinten, létezik-e az adat. Ha valami hiányzik, nem dob hibát (Error), hanem `undefined`-et ad vissza.

**Példa:**

```ts
type Auto = {
    tulaj?: { // A tulajdonos nem biztos, hogy létezik
        cim?: { // A cím sem biztos
            varos: string
        }
    }
};

let kocsi: Auto = {}; // Üres objektum

// Hagyományos módszer (Hosszú és csúnya)
if (kocsi && kocsi.tulaj && kocsi.tulaj.cim) { ... }

// Modern módszer (?.)
console.log(kocsi.tulaj?.cim?.varos); 
// undefined lesz (Nem omlik össze a program!)
```

### 7.4. A spread `...` (Spread / Rest - Kibontás és Begyűjtés)

A három pontnak kétféle felhasználása van attól függően, hol használjuk.

#### A) Spread (Kibontás)

Tömbök vagy objektumok tartalmát "szétteríti", másolja.

```ts
let gyumolcsok1 = ["Alma", "Körte"];
let gyumolcsok2 = ["Szilva", "Barack"];
const szamok: number[] = [80, 10, 5, 5];

// Két tömb összefűzése
let osszesGyumolcs = [...gyumolcsok1, ...gyumolcsok2, "Dinnye"];
console.log(osszesGyumolcs); 
// ["Alma", "Körte", "Szilva", "Barack", "Dinnye"]

// Objektum másolása és bővítése
let user = { nev: "Péter", kor: 30 };
let userUpdate = { ...user, varos: "Budapest" }; 
// { nev: "Péter", kor: 30, varos: "Budapest" }

// Maximum meghatározása
const maximum = Math.max(...szamok); 
console.log(maximum); // 100

```

#### B) Rest (Maradék begyűjtése)

Függvényeknél használjuk, ha nem tudjuk előre, hány paramétert kapunk.

```ts
// Bármennyi számot elfogad és tömbként kezeli őket
function osszeadas(...szamok: number[]): number {
    return szamok.reduce((a, b) => a + b, 0);
}

console.log(osszeadas(1, 2));       // 3
console.log(osszeadas(1, 2, 3, 4)); // 10

```

---

### 7.5 A `new` operátor

Új objektum példány létrehozása.

```ts id="4p3vqt"
class Ember {
  nev: string;

  constructor(nev: string) {
    this.nev = nev;
  }
}

let e1 = new Ember("Anna");
console.log(e1.nev); // Anna
```

---

### 7.6 Tagválasztó `.` operátor

Objektumok tulajdonságainak és metódusainak elérése.

```ts id="grw7z8"
let user = {
  nev: "Béla",
  kor: 25
};

console.log(user.nev); // Béla
```

---

### 7.7 Az `typeof` operátor

Egy változó típusát adja vissza (stringként).

```ts id="1z6r8v"
let szam: number = 42;

console.log(typeof szam); // "number"
```

---

### 7.8 Az `instanceof` operátor

Megvizsgálja, hogy egy objektum egy adott osztály példánya-e.

```ts id="qf0l9h"
class Auto {}

let a1 = new Auto();

console.log(a1 instanceof Auto); // true
```

---

### 7.9 Az `as` operátor (Type Assertion)

Típus kényszerítése TypeScript-ben.

```ts id="g0c0xm"
let ertek: unknown = "Hello";

let hossz: number = (ertek as string).length;

console.log(hossz); // 5
```

---

## 🧠 Megjegyzés

* `new`, `.`, `instanceof` → objektum-orientált használat
* `typeof`, `as` → típuskezelés
* `??`, `?.` → biztonságos, modern kód

