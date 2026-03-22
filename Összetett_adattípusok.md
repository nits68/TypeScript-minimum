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
let ures: number[] = [];

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

// Típuskonverzió, ha biztosan van "Körte" kulcs a szótárban:
const körteMennyisége: number = raktar.get("Körte") as number; // Mivel a get() visszatérhet undefined értékkel is
// Vagy undefined esetén legyen nulla az érték:
const körteMennyisége: number = raktar.get("Körte") ?? 0;
// A ?? (nullish coalescing operator) azt jelenti: 
// ha a bal oldali érték null vagy undefined, akkor a jobb oldalt használja.
// A || (logical OR) operátor is használható, 
// de az minden "falsy" érték (0, "", false, null, undefined, NaN, -0, 0n) esetén a jobb oldalt adja vissza.

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


## 6️⃣ Mátrix (kétdimenziós tömb)

Ninics dedikált kétdimenziós tömb, a mátrixot tömb típusú tömbként tudjuk megvalósítani<br>
(pl.: szám típusú tömböket tartalmazó tömb)

```ts
    const matrixSzám: number[][] = [
      [3, 5, 6, 3],
      [5, 9, 0, 4],
      [4, 7, 8, 9],
    ];
```

Mátrixok deklarálása és inicializálása üres tömbbel

```ts
const mátrixSzöveg: string[][] = [];
const mátrixSzám: number[][] = [];
const mátrixLogikai: boolean[][] = [];
```

Mátrix feltöltése azonos (pl.: "#") értékekkel

```ts
const sorokSzáma: number = 5;
const oszlopokSzáma: number = 8;
 while (mátrixSzöveg.length < sorokSzáma) {
      matrixSzöveg.push(new Array<string>(oszlopokSzáma).fill("#"));
 }

 // vagy deklarálása és inicializálása egy lépésben:
const mátrixSzöveg: string[][] = Array.from({ length: sorokSzáma }, () => Array(oszlopokSzáma).fill("#"));
```

Mátrix kiírásához több soros szöveg készítése

```ts
const kiíráshoz: string = mátrixSzöveg.map((sor) => sor.join(" ")).join("\n");
```

Mátrix feltöltése állományból (matrix.txt)

```ts
    const adatsorok: string[] = fs.readFileSync("matrix.txt", "utf8").split("\n").map((s) => s.trim());
    while (adatsorok.at(-1)?.length === 0) adatsorok.pop(); // üres adatsorok törlése

    for (let sor = 0; sor < adatsorok.length; sor++) {
      const aktuálisSor: number[] = []; // Létrehozunk egy üres sort
      for (let oszlop = 0; oszlop < adatsorok[sor].length; oszlop++) {
        aktuálisSor.push(Number(adatsorok[sor][oszlop])); // konvertálni kell!
      }
      mátrixSzám.push(aktuálisSor); // A feltöltött adatsort hozzáadjuk a mátrixhoz
    }
```


---

## 7️⃣ Enum (Felsorolás)

Konstans értékek definiálása, hogy a kód olvashatóbb (tisztább) legyen.

### Példa

```ts
enum Irany {
    Eszak,  // 0
    Del,    // 1
    Kelet,  // 2
    Nyugat  // 3
}

let mozgás: Irany = Irany.Eszak;

console.log(Irany.Eszak); // 0
if (mozgás === Irany.Eszak) {
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

## 8️⃣ Literal Type (Konkrét értékek)

```ts
// Csak ez a három szöveg fogadható el
type Lampaszin = "piros" | "sárga" | "zöld";

let lampa: Lampaszin = "piros";
// lampa = "kék"; // HIBA

```
---

##  A length és a size tulajdonság
Gyakori hiba a `.length` és `.size` összekeverése.

A szabály: a hagyományos, indexelhető típusoknál length, a modern kollekcióknál (Set, Map) size tulajdonság van.