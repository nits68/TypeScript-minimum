# TypeScript – Math objektum (Matematikai műveletek)

A `Math` egy beépített objektum, amely matematikai műveletekhez tartalmaz statikus metódusokat és konstansokat.
Nem kell példányosítani (`new Math()` ❌).

---

## 1️⃣ Fontos konstansok

```ts
Math.PI      // π (3.14159...)
Math.E       // Euler-féle szám
Math.SQRT2   // √2
Math.LN2     // ln(2)
```

### Példa

```ts
let korSugar: number = 5;
let terulet: number = Math.PI * korSugar ** 2;

console.log(terulet);
```

---

## 2️⃣ Kerekítési műveletek

### Metódusok

* `Math.round(x)` – Legközelebbi egész
* `Math.floor(x)` – Lefelé kerekít
* `Math.ceil(x)` – Felfelé kerekít
* `Math.trunc(x)` – Levágja a tizedesrészt

### Példa

```ts
let szam: number = 4.7;

console.log(Math.round(szam)); // 5
console.log(Math.floor(szam)); // 4
console.log(Math.ceil(szam));  // 5
console.log(Math.trunc(szam)); // 4
```

---

## 3️⃣ Alap matematikai függvények

### Metódusok

* `Math.abs(x)` – Abszolút érték
* `Math.pow(alap, kitevő)` – Hatványozás
* `Math.sqrt(x)` – Négyzetgyök
* `Math.cbrt(x)` – Köbgyök
* `Math.sign(x)` – Signum (Előjel fg.) (-1, 0, 1)

### Példa

```ts
let a: number = -9;

console.log(Math.abs(a));      // 9
console.log(Math.sqrt(16));    // 4
console.log(Math.pow(2, 3));   // 8
console.log(2 ** 3);           // 8 (modern megoldás)
```

---

## 4️⃣ Minimum és Maximum

### Metódusok

* `Math.min(...)`
* `Math.max(...)`

### Példa

```ts
console.log(Math.min(3, 7, 1)); // 1
console.log(Math.max(3, 7, 1)); // 7
```

### Tömb esetén (spread operátor)

```ts
let szamok: number[] = [10, 5, 8, 20];

let maximum: number = Math.max(...szamok);
console.log(maximum); // 20
```

---

## 5️⃣ Véletlenszám generálás

### `Math.random()`

0 és 1 közötti lebegőpontos (valós, tört) számot ad vissza.

`0 <= generáltSzám < 1`

```ts
let r: number = Math.random();
console.log(r);

let nevek: string[] = ["Anna", "Béla", "Csilla"];

let index: number = Math.floor(Math.random() * nevek.length);
console.log(nevek[index]); // véletlenül kiválasztott név
```

### Egész szám 10–99 között (kétjegyű szám)

```ts
let randomSzam: number = Math.floor(Math.random() * 90) + 10;
console.log(randomSzam);
```

---

## 6️⃣ Trigonometriai függvények

⚠️ FIGYELEM! Radiánban várják és adják vissza a szögértéket!

### Metódusok

* `Math.sin(x)`
* `Math.cos(x)`
* `Math.tan(x)`
* `Math.asin(x)`
* `Math.acos(x)`
* `Math.atan(x)`

### Példa (fok → radián)

```ts
let fok: number = 90;
let radian: number = fok * Math.PI / 180;

console.log(Math.sin(radian)); // 1
```

---

## 7️⃣ Logaritmus és exponenciális műveletek

* `Math.log(x)` – természetes logaritmus
* `Math.log10(x)`
* `Math.log2(x)`
* `Math.exp(x)` – e^x

### Példa

```ts
console.log(Math.log(Math.E));  // 1
console.log(Math.log10(100));   // 2
console.log(Math.exp(1));       // e
```