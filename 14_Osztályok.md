# TypeScript – Osztályok (Class)

Az osztályok segítségével objektumorientált módon szervezhetjük a kódot.
TypeScriptben a class erősebb típusrendszerrel rendelkezik, mint a sima JavaScript.

---

## 1️⃣ Osztály létrehozása

### Alap szintaxis

```ts
class OsztalyAzonsítója {
    // mezők (változók)
    // jellemzők
    // konstruktor
    // metódusok (függvények)
}
```

### Példa

```ts
class Ember {
    nev: string;
    eletkor: number;

    constructor(nev: string, eletkor: number) {
        this.nev = nev;
        this.eletkor = eletkor;
    }

    koszon(): void {
        console.log("Szia! A nevem " + this.nev);
    }
}

const ember1 = new Ember("Anna", 25);
ember1.koszon();
```

---

## 2️⃣ Mezők (tagváltozók)

### Példa típusmegadással

```ts
class Auto {
    marka: string;
    evjarat: number;
}
```

### Inicializálás konstruktorban

```ts
class Auto {
    marka: string;
    evjarat: number;

    constructor(marka: string, evjarat: number) {
        this.marka = marka;
        this.evjarat = evjarat;
    }
}
```

---

## 3️⃣ Láthatósági szintek (Access Modifiers)

### 🔓 public (alapértelmezett, nem írjuk ki)

Bárhonnan elérhető.

```ts
public nev: string;
```

### 🔒 private, vagy # karakterrel kezdődő azonosító

Csak az osztályon belül elérhető.

```ts
private egyenleg: number;
#kód: string; // #-al kezdődő tagazonosítok private szintet jelentenek
```

### 🛡 protected

Az osztályban és a leszármazott osztályokban érhető el.

```ts
protected azonosito: number;
```

### Példa

```ts
class Bankszamla {
    tulaj: string; // alapértelmezetten public
    #egyenleg: number;

    constructor(tulaj: string, egyenleg: number) {
        this.tulaj = tulaj;
        this.#egyenleg = egyenleg;
    }

    public lekerEgyenleg(): number {
        return this.#egyenleg;
    }
}
```

---

## 4️⃣ Getter és Setter - Jellemzők definiálása

Lehetővé teszi az adatok ellenőrzött elérését (olvasását és írását).
Ha csak getter van, akkor csak olvasható jellemzőnek hívjuk

```ts
class Diak {
    #jegy: number = 1;

    get jegy(): number {
        return this.#jegy;
    }

    set jegy(ertek: number) { // nincs value, mint a C#-ban!
        if (ertek >= 1 && ertek <= 5) {
            this.#jegy = ertek;
        }
    }
}
```

Használat:

```ts
let d = new Diak();
d.jegy = 4;
console.log(d.jegy);
```

---

## 5️⃣ Statikus tagok

Az osztályhoz tartoznak, nem a példányhoz.

```ts
class MathSeged {
    static duplaz(szam: number): number {
        return szam * 2;
    }
}

console.log(MathSeged.duplaz(5)); // 10
```

Nem kell `new`.

---

## 6️⃣ readonly mezők

Csak egyszer adható érték (általában konstruktorban).

```ts
class Felhasznalo {
    readonly id: number;

    constructor(id: number) {
        this.id = id;
    }
}
```

---

## 7️⃣ Öröklés (extends)

Egy osztály örökölhet tagokat egy másikból.

```ts
class Allat {
    nev: string;

    constructor(nev: string) {
        this.nev = nev;
    }

    hangotAd(): void {
        console.log("Valamilyen hang");
    }
}
```

### Leszármazott osztály

```ts
class Kutya extends Allat {
    fajta: string;

    constructor(nev: string, fajta: string) {
        super(nev); // szülő konstruktor hívása
        this.fajta = fajta;
    }

    override hangotAd(): void {
        console.log("Vau!");
    }
}
```

---

## 8️⃣ Metódus felülírás (Override)

A leszármazott osztály újradefiniálhat egy metódust.

```ts
class Macska extends Allat {
    override hangotAd(): void {
        console.log("Miau!");
    }
}
```

Az `override` kulcsszó segít a hibák elkerülésében (TS 4.3+).

---

## 9️⃣ Absztrakt osztály (abstract)

Nem példányosítható, csak öröklési célból hozzuk őket létre

```ts
abstract class Jarmu {
    abstract mozog(): void;
}

class Auto extends Jarmu {
    mozog(): void {
        console.log("Az autó gurul.");
    }
}
```

---

## 🔟 Interface implementálása

Osztály megvalósíthat egy interface-t.

```ts
interface Repulo {
    repul(): void;
}

class Madar implements Repulo {
    repul(): void {
        console.log("A madár repül.");
    }
}
```
