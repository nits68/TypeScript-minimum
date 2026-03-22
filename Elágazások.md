# TypeScript - Elágazások (szelekciók)
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

## 4️⃣ switch-case szerkezet

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

## 5️⃣ Igaz és hamis értékek

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

