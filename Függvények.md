# TypeScript – Függvények (Functions)

A függvények kód-egységbe zárt, újrafelhasználható utasítássorozatok. TypeScriptben a paraméterek és a visszatérési érték típusát is megadhatjuk (és javasolt is megadni).

---

## 1️⃣ Függvény deklaráció (function declaration)

### Alap szintaxis

```ts
function fuggvenyNev(parameter1: típus, parameter2: típus): visszatérésiTípus {
    // kód
    return valami;
}
```

### Példa

```ts
function osszead(a: number, b: number): number {
    return a + b;
}

console.log(osszead(3, 4)); // 7
```

---

## 2️⃣ A `void` visszatérési típus

Ha a függvény nem ad vissza értéket (csak csinál valamit, pl. kiír), a visszatérési típusa `void`.

```ts
function koszones(nev: string): void {
    console.log(`Szia, ${nev}!`);
}

koszones("Anna"); // Szia, Anna!
```

---

## 3️⃣ Függvénykifejezés (function expression)

A függvényt egy változóba is eltárolhatjuk, névtelen (anonim) formában.

```ts
const szorzas = function (a: number, b: number): number {
    return a * b;
};

console.log(szorzas(3, 4)); // 12
```

---

## 4️⃣ Arrow function (nyílfüggvény)

Rövidebb, modern szintaxis. Kifejezetten gyakori callback függvényeknél (pl. `map`, `filter`, `forEach`).

### Szintaxis

```ts
const fuggvenyNev = (parameter1: típus, parameter2: típus): visszatérésiTípus => {
    // kód
    return valami;
};
```

### Példa

```ts
const kivonas = (a: number, b: number): number => {
    return a - b;
};

console.log(kivonas(10, 4)); // 6
```

### Rövidített forma (egysoros kifejezés esetén nincs szükség `{}` és `return`-re)

```ts
const negyzet = (x: number): number => x * x;

console.log(negyzet(5)); // 25
```

---

## 5️⃣ Opcionális paraméterek (`?`)

Egy paramétert opcionálissá tehetünk `?` jellel. Ekkor a hívó elhagyhatja, ilyenkor az értéke `undefined` lesz.

⚠️ Az opcionális paraméterek csak a kötelező paraméterek **után** állhatnak.

```ts
function bemutatkozas(nev: string, beosztas?: string): void {
    if (beosztas) {
        console.log(`${nev} vagyok, ${beosztas}.`);
    } else {
        console.log(`${nev} vagyok.`);
    }
}

bemutatkozas("Kovács Éva", "tanár"); // Kovács Éva vagyok, tanár.
bemutatkozas("Kis Pál");             // Kis Pál vagyok.
```

---

## 6️⃣ Alapértelmezett paraméterérték (default parameter)

Ha a híváskor nem adunk meg értéket, az alapértelmezett érték kerül felhasználásra.

```ts
function level(cim: string, urgency: string = "normál"): void {
    console.log(`Levél: "${cim}" (${urgency})`);
}

level("Találkozó holnap");            // Levél: "Találkozó holnap" (normál)
level("Szerver leállt!", "sürgős");   // Levél: "Szerver leállt!" (sürgős)
```

> **Tipp:** opcionális paraméter (`?`) és alapértelmezett érték (`=`) hasonló célt szolgál, de a kettő **nem keverendő** ugyanazon a paraméteren – vagy `?`, vagy `=`, a kettő együtt fölösleges (és hibát is ad).

---

## 7️⃣ Rest paraméter (`...`)

Ha nem tudjuk előre, hány argumentumot kapunk, `...` jellel egy tömbbe gyűjthetjük őket. Csak az **utolsó** paraméter lehet rest paraméter.

```ts
function osszeadasSok(...szamok: number[]): number {
    return szamok.reduce((osszeg, aktualis) => osszeg + aktualis, 0);
}

console.log(osszeadasSok(1, 2));       // 3
console.log(osszeadasSok(1, 2, 3, 4)); // 10
console.log(osszeadasSok());           // 0
```

---

## 8️⃣ Függvénytípus megadása (type / interface)

Egy változó típusaként is megadhatjuk, hogy "milyen alakú" függvényt fogadhat el (milyen paraméterei és visszatérési típusa legyen).

```ts
type MatematikaiMuvelet = (a: number, b: number) => number;

const osszead2: MatematikaiMuvelet = (a, b) => a + b;
const szorzas2: MatematikaiMuvelet = (a, b) => a * b;

function szamol(a: number, b: number, muvelet: MatematikaiMuvelet): number {
    return muvelet(a, b);
}

console.log(szamol(4, 5, osszead2)); // 9
console.log(szamol(4, 5, szorzas2)); // 20
```

📌 Ez a mintázat teszi lehetővé, hogy egy függvényt **paraméterként** adjunk át egy másik függvénynek (ún. magasabb rendű függvény / higher-order function) – pontosan ezt csinálja a `map()`, `filter()`, `reduce()` és a `forEach()` is a háttérben.

---

## 9️⃣ Névtelen (anonim) függvény mint argumentum

Nagyon gyakori minta: a callback függvényt közvetlenül a hívás helyén definiáljuk, külön névadás nélkül.

```ts
let szamok: number[] = [1, 2, 3, 4, 5];

let paratlanok: number[] = szamok.filter((szam: number): boolean => {
    return szam % 2 !== 0;
});

console.log(paratlanok); // [1, 3, 5]
```

---

## 🔟 Túlterhelés (Overload) – csak érintőlegesen

TypeScriptben egy függvénynek több "aláírása" (signature) is lehet, ha a bemenettől függően más-más típusú eredményt ad vissza. Ez haladóbb téma, de érdemes tudni, hogy létezik.

```ts
function elsoElem(tomb: number[]): number;
function elsoElem(tomb: string[]): string;
function elsoElem(tomb: any[]): any {
    return tomb[0];
}

console.log(elsoElem([1, 2, 3]));         // 1
console.log(elsoElem(["a", "b", "c"]));   // "a"
```

---

## 🧠 Megjegyzés

* `function` deklaráció → jól olvasható, hoisted (a fájlban korábbra is hivatkozhatunk rá)
* arrow function → tömör, gyakori callback-eknél, **nincs saját `this`-e** (a környező kontextus `this`-ét használja)
* `?` és `=` → soha ne ugyanazon a paraméteren
* `...rest` → csak az utolsó paraméter lehet
* egy típus (`type`) is leírhatja egy függvény "alakját", ez teszi lehetővé, hogy függvényt adjunk át paraméterként
