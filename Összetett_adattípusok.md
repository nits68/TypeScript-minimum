# TypeScript - Összetett típusok és adatszerkezetek

## 1️⃣ Tömbök (Arrays)

Azonos típusú elemek tárolására. Kétféle jelölés létezik, de az első az elterjedtebb.

### Szintaxis
```ts
let tömb1: típus[] = [...];
let tömb2: Array<típus> = [...]; // Generikus írásmód
```

### Példa

```ts
let szamok: number[] = [1, 2, 3, 4, 5];
let nevek: Array<string> = ["Anna", "Béla", "Cecil"];

// Hozzáadás
nevek.push("Dénes"); 

// Bejárás klasszikus for ciklussal:
let szamok: number[] = [10, 20, 30];

for (let i = 0; i < szamok.length; i++) {
    console.log(i, szamok[i]);
}

// Bejárás for-in ciklussal:
for (let index in szamok) {
    console.log(index, szamok[index]);
}

// Bejárás for-of ciklussal (ninics index):
for (let ertek of szamok) {
    console.log(ertek);
}

// Bejárás a tömb forEach() metódusával (nincs break, continue és return):
szamok.forEach((ertek, index) => {
    console.log(index, ertek);
});

// Speciális lehetőségek:
let duplazott = szamok.map(szam => szam * 2);
let nagyok = szamok.filter(szam => szam > 15);
let osszeg = szamok.reduce((acc, szam) => acc + szam, 0);

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

// Tuple inincializálása kezdőértékekkel:
let adat: [string, number] = ["Péter", 25];

// Hivatkozás Tuple elemeire (indexeléssel):
console.log(adat[0]); // Péter
console.log(adat[1]); // 25
```

---

## 3️⃣ Objektumok (Objects) és definiálásuk

Kulcs-érték párok tárolása. TypeScript-ben érdemes `interface` vagy `type` segítségével előre definiálni a szerkezetet.

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
// Üres szótár (Map)
let raktar = new Map<string, number>();

// Elemekkel inicializált szótár:
const stat: Map<string, number> = new Map<string, number>([
  ["alma", 2],
  ["körte", 4],
  ["szilva", 3],
  ["barack", 5],
]);

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

// Szótár bejárása for-of ciklussal
for (const [kulcs, érték] of stat) {
    vissza += `${kulcs} pálinka ${érték} liter\n`;
  }
```

---

## 5️⃣ Set (Halmaz)

Olyan gyűjtemény, amelyben minden érték **csak egyszer** szerepelhet (egyedi elemek).

### Létrehozás és használat

```ts
// Üres halmaz létrehozása
let egyediSzamok = new Set<number>();

// Elemek hozzáadása
egyediSzamok.add(10);
egyediSzamok.add(20);
egyediSzamok.add(10); // Ezt figyelmen kívül hagyja, mert a 10 már benne van!

// Halmaz elemszáma:
console.log(egyediSzamok.size); // 2

// Halmaz inicializálása kezőértékekkel
let egyediSzamok2 = new Set<number>([1, 2, 2, 3, 4, 4]);
console.log(egyediSzamok2); // {1, 2, 3, 4}
```

### Tipp: Tömb szűrése: Duplikációk eltávolítása

```ts
let szamok = [1, 2, 2, 3, 3, 3, 4];
// Halmaz inicializálássa tömbbel "new Set(szamok)", majd a halmazt tömbbé alakítjuk vissza "[...new ]":
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