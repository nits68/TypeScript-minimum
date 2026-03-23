Örömmel kiegészítem az állományodat\! A kért operátorokat logikusan beillesztettem a struktúrádba: a bitenkénti operátorok egy új, önálló főszekciót kaptak, míg a többi a „Speciális operátorok” közé került, folytatva a számozást.

Íme a teljes, frissített Markdown tartalom, amit egy az egyben bemásolhatsz a fájlodba:

-----

# TypeScript – Operátorok (Operators)

Az operátorok segítségével műveleteket végezhetünk változókon és értékeken.

-----

## 1️⃣ Aritmetikai operátorok (matematikai műveletek)

### Operátorok

```ts
+   // összeadás
-   // kivonás
* // szorzás
/   // osztás
%   // maradék (modulo)
** // hatványozás
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

-----

## 2️⃣ Értékadó operátorok (assignment)

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

-----

## 3️⃣ Összehasonlító operátorok (comparison)

### Operátorok

```ts
==   // egyenlő (érték szerint, ha kell összehasonlítás előtt konvertál)
===  // szigorúan egyenlő (érték + típus)
!=   // nem egyenlő (érték szerint, ha kell összehasonlítás előtt konvertál)
!==  // szigorúan nem egyenlő (érték + típus)
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

-----

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

-----

## 5️⃣ Inkrementáló és dekrementáló operátorok

### Operátorok

```ts
++  // növelés 1-gyel
--  // csökkentés 1-gyel
// állhat prefix és postfix pozícióban (lsd. példa)
```

### Példa

```ts
let i: number = 5;

i++; // i = i + 1
console.log(i); // 6

i--; // i = i - 1
console.log(i); // 5

// prefix (előbb növel, majd használ)
console.log(++i); // 6
console.log(i); // 6

// postfix (előbb használ, majd növel)
console.log(i++); // 6
console.log(i); // 7
```

-----

## 6️⃣ Feltételes operátor `?:` (ternáris)

Egyetlen három operandusú operátor, röviden lehet if-else szerkezetet megvalósítani vele.

### Szintaxis

```ts
feltétel ? érték_ha_a_feltétel_igaz : érték_ha_a_feltétel_hamis
```

### Példa

```ts
let kor: number = 18;

let uzenet: string = kor >= 18 ? "Felnőtt" : "Kiskorú";

console.log(uzenet); // "Felnőtt"
```

-----

## 7️⃣ Speciális operátorok

### 7.1 A dupla tagadás `!!` operátor (double bang)

Ha bármilyen értéket valódi `boolean` típusú értékké (true/false) akarsz alakítani, használd a `!!` jelet.

```ts
console.log(!!"hello"); // true
console.log(!!0);       // false
console.log(!!{});      // true
```

### 7.2 Nullás összevonás `??` (nullish coalescing)

Ez egy olyan alapértelmezett értéket biztosító operátor, amely visszaadja a jobb oldali operandust,<br>
ha a bal oldali operandus szigorúan **null** vagy **undefined**.<br>
Minden más esetben a bal oldali értéket tartja meg.<br>
Hasonlít a `||` (VAGY) operátorhoz, de **sokkal biztonságosabb**, szigorúbb.

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

### 7.3 Opcionális láncolás `?.` (optional chaining)

Segítségével úgy érhetünk el egy objektum mélyén lévő tulajdonságot, hogy nem kell ellenőrizni minden szinten (minden szülőt), hogy létezik-e a tulajdonság (adat).<br>
Ha valami hiányzik, nem dob hibát (Error), hanem `undefined`-et ad vissza.

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
if (kocsi && kocsi.tulaj && kocsi.tulaj.cim) { /* ... */ }

// Modern módszer (?.)
console.log(kocsi.tulaj?.cim?.varos); 
// undefined lesz (Nem dob hibát a végrehajtás!)
```

### 7.4. A spread `...` (Spread / Rest - kibontás és maradék paraméter)

A három pontnak kétféle felhasználása van attól függően, hol használjuk.

#### 7.4.1 Spread (Kibontás)

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
console.log(maximum); // 80
```

#### 7.4.2 Rest (Maradék begyűjtése)

Függvényeknél használjuk, ha nem tudjuk előre, hány paramétert kapunk.

```ts
// Bármennyi számot elfogad és tömbként kezeli őket
function osszeadas(...szamok: number[]): number {
    return szamok.reduce((a, b) => a + b, 0);
}

console.log(osszeadas(1, 2));       // 3
console.log(osszeadas(1, 2, 3, 4)); // 10
```

### 7.5 A `new` operátor

Új objektum példány létrehozása.

```ts
class Ember {
  nev: string;

  constructor(nev: string) {
    this.nev = nev;
  }
}

let e1 = new Ember("Anna");
console.log(e1.nev); // Anna
```

### 7.6 Tagválasztó `.` operátor

Objektumok (osztálypéldányok) tulajdonságainak és metódusainak elérése.

```ts
let user = {
  nev: "Béla",
  kor: 25
};

console.log(user.nev); // Béla
```

### 7.7 A `typeof` operátor

Egy változó típusát adja vissza (stringként).

```ts
let szam: number = 42;

console.log(typeof szam); // "number"
```

### 7.8 Az `instanceof` operátor

Megvizsgálja, hogy egy objektum egy adott osztály példánya-e.

```ts
class Auto {}

let a1 = new Auto();

console.log(a1 instanceof Auto); // true
```

### 7.9 Az `as` operátor (type assertion)

Típus kényszerítése TypeScript-ben. Azt mondjuk a fordítónak: "Tudom, mit csinálok, kezeld ezt a változót ilyen típusként".

```ts
let ertek: unknown = "Hello";

let hossz: number = (ertek as string).length;

console.log(hossz); // 5
```

### 7.10 Nem-null állítás `!.` (non-null assertion)

Azt üzenjük vele a TypeScript fordítónak, hogy biztosak vagyunk benne: a változó értéke az adott pillanatban **nem lehet** `null` vagy `undefined`.

```ts
let becenev: string | null = "Gabi";

// Ha nem tennénk ki a !-t, a TS hibát jelezhetne a Strict mód miatt
let karakterekSzama = becenev!.length; 

console.log(karakterekSzama); // 4
```

### 7.11 A `delete` operátor

Töröl egy tulajdonságot egy objektumból. (TypeScriptben ehhez a tulajdonságnak általában opcionálisnak kell lennie).

```ts
interface Adat {
    id: number;
    titkos?: string; // Opcionális, így törölhető
}

let csomag: Adat = { id: 1, titkos: "jelszo123" };
delete csomag.titkos;

console.log(csomag); // { id: 1 }
```

### 7.12 A `void` operátor

Kiértékel egy kifejezést, majd eldobja az eredményt és `undefined`-ot ad vissza. Régebben vagy speciális esetekben (pl. hiperhivatkozásoknál az oldalon való ugrás megakadályozására) használták.

```ts
let eredmeny = void (2 + 2);

console.log(eredmeny); // undefined
```

### 7.13 A `keyof` operátor

Típus-szintű operátor. Segítségével kinyerhetjük egy objektum típusának kulcsait, és csinálhatunk belőlük egy unió (union) típust.

```ts
type Kutyus = { nev: string; fajta: string; kor: number };

// "nev" | "fajta" | "kor"
type KutyusKulcsok = keyof Kutyus; 

let property: KutyusKulcsok = "fajta"; // Helyes
// let rosszProperty: KutyusKulcsok = "szin"; // Hiba! A "szin" nem létezik a Kutyusban
```

### 7.14 A `satisfies` operátor (TS 4.9+)

Ellenőrzi, hogy egy kifejezés megfelel-e egy adott típusnak, anélkül, hogy a változó elveszítené a legpontosabb, specifikus típusát.

```ts
type Szinek = "piros" | "kék" | "zöld";

// Ellenőrzi, hogy a "piros" benne van-e a Szinek-ben, 
// de a myColor típusa szigorúan "piros" marad, nem lesz a tágabb Szinek unió.
let myColor = "piros" satisfies Szinek; 
```

-----

## 8️⃣ Bitenkénti operátorok (Bitwise)

Ezek az operátorok a számokat 32 bites bináris (0 és 1) formában kezelik, és bit-szinten végeznek rajtuk műveleteket. A mindennapi fejlesztés során (hacsak nem hardver-közeli dolgokat vagy specifikus algoritmusokat írsz) ritkán van rájuk szükség.

### Operátorok

```ts
&    // Bitenkénti ÉS (AND)
|    // Bitenkénti VAGY (OR)
^    // Bitenkénti KIZÁRÓ VAGY (XOR)
~    // Bitenkénti TAGADÁS (NOT) - a bitek invertálása
<<   // Eltolás balra (Left shift)
>>   // Eltolás jobbra (előjel megtartásával) (Right shift)
>>>  // Eltolás jobbra (előjel nélküli, nullával feltöltő) (Zero-fill right shift)
```

### Példa

```ts
let a = 5;  // Binárisan: 0101
let b = 3;  // Binárisan: 0011

console.log(a & b); // 1  (Binárisan 0001, mert csak az utolsó bit egyezik)
console.log(a | b); // 7  (Binárisan 0111)
console.log(a ^ b); // 6  (Binárisan 0110)
console.log(~a);    // -6 (Minden bit megfordul)
console.log(a << 1); // 10 (Minden bit 1-gyel balra csúszik, ami szorzás 2-vel: 1010)
```

-----

## 🧠 Megjegyzés

  * `new`, `.`, `instanceof` → objektum-orientált használat
  * `typeof`, `as`, `keyof`, `satisfies`, `!.` → típuskezelés és TypeScript specifikumok
  * `??`, `?.` → biztonságos, modern kód
  * `&`, `|`, `^` stb. → bináris, bit szintű operációk

