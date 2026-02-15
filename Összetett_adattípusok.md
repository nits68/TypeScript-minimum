# TypeScript - Összetett típusok és adatszerkezetek

## 1️⃣ Tömbök (Arrays)

Azonos típusú elemek tárolására. Kétféle jelölés létezik, de az első az elterjedtebb.

### Szintaxis
```ts
let lista1: típus[] = [...];
let lista2: Array<típus> = [...]; // Generikus írásmód
```

### Példa

```ts
let szamok: number[] = [1, 2, 3, 4, 5];
let nevek: Array<string> = ["Anna", "Béla", "Cecil"];

// Hozzáadás
nevek.push("Dénes"); 

```

---

## 2️⃣ Tuple

Olyan speciális tömb, amelynek **rögzített a hossza**, és az elemek **típusa eltérő** lehet.

### Példa

```ts
// [név, életkor, aktív-e]
let dolgozo: [string, number, boolean];

dolgozo = ["Kiss János", 35, true]; 

// Hiba lenne: dolgozo = [35, "Kiss János", true]; // A sorrend számít!

```

---

## 3️⃣ Objektumok (Objects) és definiálásuk

Kulcs-érték párok tárolása. TypeScriptben érdemes `interface` vagy `type` segítségével előre definiálni a szerkezetet.

### Definíció interface-el

```ts
interface Auto {
    marka: string;
    evjarat: number;
    elektromos?: boolean; // ?: Opcionális tulajdonság (nem kötelező)
}
```

### Definíció type-al (ajánlott)

```ts
type Auto = {
    marka: string;
    evjarat: number;
    elektromos?: boolean; // ?: Opcionális tulajdonság (nem kötelező)
};
```

### Példa

```ts
let kocsi: Auto = {
    marka: "Toyota",
    evjarat: 2022
    // Az 'elektromos' itt hiányozhat, mert opcionális
};

console.log(kocsi.marka);

```

---

## 4️⃣ Map (Szótár / Kulcs-Érték párok tárolása)

Hasonlít az objektumhoz, de a kulcs **bármilyen típusú** lehet (nem csak string), és megőrzi a beszúrási sorrendet.

### Létrehozás és használat

```ts
// Kulcs: string, Érték: number
let raktar = new Map<string, number>();

// Elemek hozzáadása
raktar.set("Alma", 100);
raktar.set("Körte", 50);

// Érték lekérése
console.log(raktar.get("Alma")); // 100

// Ellenőrzés és Törlés
if (raktar.has("Szilva")) {
    console.log("Van szilva!");
}
raktar.delete("Körte");

```

---

## 5️⃣ Set (Halmaz)

Olyan gyűjtemény, amelyben minden érték **csak egyszer** szerepelhet (egyedi elemek).

### Létrehozás és használat

```ts
let egyediSzamok = new Set<number>();

egyediSzamok.add(10);
egyediSzamok.add(20);
egyediSzamok.add(10); // Ezt figyelmen kívül hagyja, mert a 10 már benne van!

console.log(egyediSzamok.size); // 2

```

### Tipp: Tömb szűrése (Duplikációk eltávolítása)

```ts
let szamok = [1, 2, 2, 3, 3, 3, 4];
let egyedi = [...new Set(szamok)]; // [1, 2, 3, 4]

```

---

## 6️⃣ Enum (Felsorolás)

Konstans értékek definiálása, hogy a kód olvashatóbb (tisztább) legyen.

### Példa

```ts
enum Irany {
    Eszak,  // 0
    Del,    // 1
    Kelet,  // 2
    Nyugat  // 3
}

let mozgus: Irany = Irany.Eszak;

if (mozgus === Irany.Eszak) {
    console.log("Észak felé megyünk");
}

```

### String Enum (Olvashatóbb értékek)

```ts
enum Statusz {
    Sikeres = "SUCCESS",
    Hiba = "ERROR"
}
console.log(Statusz.Hiba); // "ERROR"

```

---

## 7️⃣ Union Type (Unió típus)

Akkor használjuk, ha egy változó több típusú értéket is felvehet.

### Példa

```ts
let azonosito: string | number;

azonosito = 123;      // OK
azonosito = "A-123";  // OK
azonosito = true;  // HIBA

```

### Literal Type (Konkrét értékek)

```ts
// Csak ez a három szöveg fogadható el
type Lampaszin = "piros" | "sárga" | "zöld";

let lampa: Lampaszin = "piros";
// lampa = "kék"; // HIBA

```
---

## 8️⃣ A length és a size tulajdonság
Gyakori hiba a `.length` és `.size` összekeverése.

A szabály: a hagyományos, indexelhető típusoknál length, a modern kollekcióknál (Set, Map) size tulajdonság van.