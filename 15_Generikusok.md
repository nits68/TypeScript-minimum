# TypeScript – Generikusok (Generics)

A generikusok lehetővé teszik, hogy egy függvényt, típust vagy osztályt **típusfüggetlenül** írjunk meg, miközben a típusbiztonságot megtartjuk. A `Map<K, V>`, `Array<T>`, `Set<T>` mind generikus típusok – ebben a fejezetben megnézzük, hogyan írjunk sajátot.

---

## 1️⃣ A probléma, amit megold

Tegyük fel, hogy szeretnénk egy függvényt, ami visszaadja egy tömb első elemét, bármilyen típusú legyen is a tömb.

### Rossz megoldás (`any` típussal – típusbiztonság elveszik)

```ts
function elsoElem(tomb: any[]): any {
    return tomb[0];
}

let eredmeny = elsoElem([1, 2, 3]);
console.log(eredmeny.toUpperCase()); // Nem fordítási hiba, de FUTÁSKOR elszáll!
```

Az `any` típussal a fordító nem tud segíteni – bármit megengedünk, és csak futásidőben derül ki a hiba.

### Jó megoldás – generikus típusparaméterrel

```ts
function elsoElem<T>(tomb: T[]): T {
    return tomb[0];
}

let szamEredmeny = elsoElem<number>([1, 2, 3]);
console.log(szamEredmeny);          // 1
// szamEredmeny.toUpperCase();      // HIBA már fordításkor! (number-nek nincs ilyen metódusa)

let szovegEredmeny = elsoElem<string>(["alma", "körte"]);
console.log(szovegEredmeny.toUpperCase()); // "ALMA" – ez viszont helyes
```

📌 A `<T>` egy **típusparaméter** (a neve tetszőleges, de konvenció szerint `T`, `U`, `K`, `V` stb. rövid nagybetűk). A híváskor a TypeScript **ki is találja** (type inference), így a `<number>` és `<string>` explicit kiírása legtöbbször el is hagyható:

```ts
let szamEredmeny2 = elsoElem([1, 2, 3]); // T automatikusan number lesz
```

---

## 2️⃣ Több típusparaméter

```ts
function parba(kulcs: string, ertek: number): [string, number] {
    return [kulcs, ertek];
}
```

Ez a fenti csak `string`/`number` párra működik. Generikusan bármilyen típuspárra:

```ts
function parbaGen<K, V>(kulcs: K, ertek: V): [K, V] {
    return [kulcs, ertek];
}

console.log(parbaGen<string, number>("kor", 25));   // ["kor", 25]
console.log(parbaGen<string, boolean>("aktiv", true)); // ["aktiv", true]
```

---

## 3️⃣ Generikus típus (type / interface)

Nem csak függvényt, típust is írhatunk generikusan.

```ts
type Doboz<T> = {
    tartalom: T;
};

let szamosDoboz: Doboz<number> = { tartalom: 42 };
let szovegesDoboz: Doboz<string> = { tartalom: "Ajándék" };
```

```ts
interface Valasz<T> {
    siker: boolean;
    adat: T;
}

let felhasznaloValasz: Valasz<string> = { siker: true, adat: "Kovács Anna" };
```

---

## 4️⃣ Generikus osztály

```ts
class Verem<T> {
    #elemek: T[] = [];

    push(elem: T): void {
        this.#elemek.push(elem);
    }

    pop(): T | undefined {
        return this.#elemek.pop();
    }

    get meret(): number {
        return this.#elemek.length;
    }
}

let szamVerem = new Verem<number>();
szamVerem.push(10);
szamVerem.push(20);
console.log(szamVerem.pop()); // 20

let szovegVerem = new Verem<string>();
szovegVerem.push("első");
console.log(szovegVerem.pop()); // "első"
```

---

## 5️⃣ Korlátozott (constrained) generikus típus

Előfordulhat, hogy a típusparaméternek **valamilyen minimális elvárásnak** meg kell felelnie (pl. legyen neki `.length` tulajdonsága). Ezt az `extends` kulcsszóval korlátozhatjuk.

```ts
interface VanHossza {
    length: number;
}

function hosszKiir<T extends VanHossza>(elem: T): void {
    console.log(`A hossz: ${elem.length}`);
}

hosszKiir("szia");         // OK, a string-nek van .length-je (4)
hosszKiir([1, 2, 3]);      // OK, a tömbnek is van .length-je (3)
// hosszKiir(42);          // HIBA! A number-nek nincs .length tulajdonsága
```

---

## 🧠 Megjegyzés

* generikus típusparaméter (`<T>`) = "később eldöntött típus", ami a híváskor/létrehozáskor rögzül
* a `Map<K, V>`, `Array<T>`, `Set<T>`, `Promise<T>` mind beépített generikus típusok – innentől már tudod, hogy ezek pontosan mit jelentenek
* a legtöbb esetben a TypeScript **kikövetkezteti** a típusparamétert, nem kell mindig kiírni
* az `extends` generikus kontextusban **korlátozást** jelent (nem öröklést), ez eltér az osztályoknál megszokott `extends`-től
