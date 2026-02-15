# TypeScript - Elágazások (szelekciók) és operátorok
## 1️⃣ if utasítás

### Alap szintaxis

``` ts
if (feltétel) {
    // kód, ha a feltétel igaz
}
```

### Példa

``` ts
let eletkor: number = 18;

if (eletkor >= 18) {
    console.log("Nagykorú.");
}
```

---

## 2️⃣ if - else

``` ts
if (feltétel) {
    // ha igaz
} else {
    // ha hamis
}
```

### Példa

``` ts
let szam: number = 5;

if (szam % 2 === 0) {
    console.log("Páros");
} else {
    console.log("Páratlan");
}
```

---

## 3️⃣ if - else if - else

``` ts
if (feltétel1) {
    // ha ez igaz
} else if (feltétel2) { // else if-ből több is lehet
    // ha az első hamis, de ez igaz
} else {
    // ha egyik sem igaz
}
```

### Példa

``` ts
let jegy: number = 4;

if (jegy === 5) {
    console.log("Jeles");
} else if (jegy === 4) {
    console.log("Jó");
} else if (jegy === 3) {
    console.log("Közepes");
} else {
    console.log("Elégtelen vagy elégséges");
}
```

---

### 4️⃣ Logikai operátorok

* `&&` és
* `||` vagy
* `!`  nem (tagadás)


---

## 5️⃣ switch-case szerkezet

``` ts
switch (kifejezés) {
    case ertek1:
        // kód, ha kifejezés == ertek1
        break;
    case ertek2:
        // kód, ha kifejezés == ertek2
        break;
    default:
        // ha egyikkel sem lesz egyenlő
}
```

---

## 6️⃣ Ternáris operátor (feltételes kifejezés)

Ez az `if-else` szerkezet rövidített változata. Akkor a leghasznosabb, ha egy változónak szeretnénk értéket adni egy feltétel alapján, egyetlen sorban.

### Alap szintaxis

```ts
feltétel ? érték_ha_igaz : érték_ha_hamis;
```

### Példa 1 (Egyszerű értékadás)

```ts
let eletkor: number = 20;

// Ha 18 vagy több, akkor "Felnőtt", különben "Gyerek"
let statusz: string = (eletkor >= 18) ? "Felnőtt" : "Gyerek";

console.log(statusz); // "Felnőtt"

```

### Példa 2 (Páros vagy páratlan)

```ts
let szam: number = 7;

// Egyetlen sorban eldöntjük
let eredmeny: string = (szam % 2 === 0) ? "Páros" : "Páratlan";

console.log(eredmeny); // "Páratlan"

```

---

## 7️⃣ Összehasonlító operátorok


  * `===` egyenlő - értékek és típusok is azonosak
  * `==` egyenlő - értékek implicit típuskonverzió után azonosak       
  * `!==` nem egyenlő
  * `>` nagyobb
  * `<` kisebb
  * `>=` nagyobb vagy egyenlő
  * `<=` kisebb vagy egyenlő
  

---

## 8️⃣ Igaz és hamis értékek

TypeScriptben nem csak a `boolean` típusú változók kerülhetnek egy `if` feltételébe, vagy `ciklus` feltételbe. Minden értéknek van egy "igazságtartalma", amikor logikai környezetben használjuk őket.

### 🔴 False (Hamisnak értékelt) értékek

Ezek azok, amelyek egy `if` feltételben `false`-ként viselkednek (tehát az `else` ág futna le):

* `false`
* `0`, `-0` (nulla)
* `0n` (BigInt nulla)
* `""`, `''` (üres string)
* `null`
* `undefined`
* `NaN`

### 🟢 True (Igaznak értékelt) értékek

**Minden más**, ami nem szerepel a fenti (hamis) listában, `true`-ként viselkedik.
**Gyakori csapdák (ezek mind IGAZAK!):**

* `[]` üres tömb
* `{}` üres objektum
* `"0"` szövegként a nulla
* `"false"` szövegként a false szó
* `" "` szóköz karaktert tartalmazó string
* `-1` negatív számok

### Példa

```ts
// Falsy példa
let nev: string = "";

if (nev) {
    console.log("Van név: " + nev);
} else {
    console.log("Nincs megadva név (üres string)."); // Ez fut le
}

// Truthy csapda (Üres tömb)
let lista: string[] = [];

if (lista) {
    console.log("A lista létezik (igaz), még ha üres is!"); // Ez lefut!
}

// Helyes ellenőrzés üres tömbre:
if (lista.length > 0) {
    // Csak akkor fut le, ha van benne elem
}

```

### Tipp: A `!!` (dupla tagadás) operátor

Ha bármilyen értéket valódi `boolean` típussá (true/false) akarsz alakítani, használd a `!!` jelet.

```ts
console.log(!!"hello"); // true
console.log(!!0);       // false
console.log(!!{});      // true

```

## 9️⃣ Egyéb operátorok (felsorolás)

### Aritmetikai (matematikai) operátorok

* `+` (összeadás)
* `-` (kivonás)
* `*` (szorzás)
* `/` (osztás)
* `%` (maradékos osztás - modulo)
* `**` (hatványozás)
* `egész osztás` (TS-ben nincs, helyette `Math.floor(a/b)`)

### Frissitő (léptető) operátorok

* `++` (inkrementálás - növelés eggyel)
* `--` (dekrementálás - csökkentés eggyel)

Fontos hogy `prefix`, vagy `postfix` pozícióban használjuk őket. Prefix: előbb növel, majd használ: `Math.sqrt(++x)`. Postfix: előbb használ, majd növel: `Math.sqrt(x++)`.

### Értékadó operátorok (rövidítések)

* `=` (értékadás)
* `+=` (hozzáadás és értékadás)
* `-=` (kivonás és értékadás)
* `*=` (szorzás és értékadás)
* `/=` (osztás és értékadás)


### Típus operátorok

* `typeof` (Visszaadja a változó típusát szövegként)
* `instanceof` (Megvizsgálja, hogy egy objektum egy adott osztály példánya-e)
* `as` (Type Assertion - Típus kényszerítés)


## 🔟 Modern TS/JS operátorok (Rész

Ezek az operátorok segítenek rövidebb, tisztább és hibatűrőbb kódot írni.

### 1. `??` (Nullish Coalescing - Nullás Összevoná́s)

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

### 2. `?.` (Optional Chaining - Opcionális Láncolás)

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

---

### 3. `...` (Spread / Rest - Kibontás és Begyűjtés)

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