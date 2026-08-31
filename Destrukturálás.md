# TypeScript – Destrukturálás (Destructuring)

A destrukturálás lehetővé teszi, hogy egy objektum vagy tömb elemeit közvetlenül külön változókba "csomagoljuk ki", felesleges köztes lépések nélkül. React, Next.js és Prisma kódban szinte minden sorban találkozunk vele (props, state, hook visszatérési érték, API válasz).

---

## 1️⃣ Objektum destrukturálás

### A probléma – hagyományos hozzáférés

```ts
let felhasznalo = { nev: "Kovács Anna", kor: 25, varos: "Budapest" };

let nev = felhasznalo.nev;
let kor = felhasznalo.kor;
console.log(nev, kor); // Kovács Anna 25
```

### Destrukturálással – rövidebb, olvashatóbb

```ts
let felhasznalo = { nev: "Kovács Anna", kor: 25, varos: "Budapest" };

const { nev, kor } = felhasznalo; // a "varos" mezőt nem kérjük le
console.log(nev, kor); // Kovács Anna 25
```

📌 A `{}` bal oldalon **nem objektumot jelent**, hanem azt mondja: "vedd ki ezeket a névvel megegyező kulcsú mezőket az objektumból".

---

## 2️⃣ Átnevezés destrukturáláskor

Ha a kinyert változónak más nevet szeretnénk adni, mint az eredeti kulcs, kettősponttal átnevezhetjük.

```ts
let felhasznalo = { nev: "Kovács Anna", kor: 25 };

const { nev: teljesNev, kor: eletkor } = felhasznalo;
console.log(teljesNev, eletkor); // Kovács Anna 25
```

---

## 3️⃣ Alapértelmezett érték destrukturáláskor

Ha egy mező hiányozhat (opcionális), alapértelmezett értéket adhatunk neki.

```ts
type Beallitas = { tema?: string; betumeret?: number };

const beallitas: Beallitas = { tema: "sötét" };

const { tema = "világos", betumeret = 14 } = beallitas;
console.log(tema, betumeret); // sötét 14 (a betumeret az alapértelmezettet kapja)
```

---

## 4️⃣ Beágyazott (nested) destrukturálás

Objektumon belüli objektumból is közvetlenül kinyerhetünk mezőket.

```ts
type Rendeles = {
    azonosito: number;
    vevo: {
        nev: string;
        cim: {
            varos: string;
        };
    };
};

const rendeles: Rendeles = {
    azonosito: 1001,
    vevo: { nev: "Nagy Béla", cim: { varos: "Szeged" } }
};

const { vevo: { nev, cim: { varos } } } = rendeles;
console.log(nev, varos); // Nagy Béla Szeged
```

---

## 5️⃣ Tömb destrukturálás

Tömbnél a **sorrend** számít (nem a név, mint objektumnál).

```ts
let koordinata: [number, number] = [10, 20];

const [x, y] = koordinata;
console.log(x, y); // 10 20
```

### Elemek kihagyása

```ts
let szinek = ["piros", "zöld", "kék"];

const [elso, , harmadik] = szinek; // a másodikat kihagyjuk
console.log(elso, harmadik); // piros kék
```

### Rest elem tömb destrukturálásnál

```ts
let szamok = [1, 2, 3, 4, 5];

const [elsoSzam, ...tobbi] = szamok;
console.log(elsoSzam); // 1
console.log(tobbi);    // [2, 3, 4, 5]
```

---

## 6️⃣ Destrukturálás függvényparaméterben

Ez az a forma, amivel React komponensek `props`-ját szinte mindig kezelik.

### Hagyományos módszer

```ts
type Props = { nev: string; kor: number };

function bemutatkozas(props: Props): string {
    return `${props.nev}, ${props.kor} éves.`;
}
```

### Destrukturált paraméter – React-stílusban

```ts
function bemutatkozas({ nev, kor }: Props): string {
    return `${nev}, ${kor} éves.`;
}

console.log(bemutatkozas({ nev: "Tóth Eszter", kor: 30 })); // Tóth Eszter, 30 éves.
```

Alapértelmezett értékkel kombinálva:

```ts
type GombProps = { szoveg: string; szin?: string };

function gomb({ szoveg, szin = "kék" }: GombProps): string {
    return `[${szoveg}] (${szin})`;
}

console.log(gomb({ szoveg: "Mentés" })); // [Mentés] (kék)
```

---

## 7️⃣ Kitekintés – `useState` és destrukturálás (React)

React-ben a `useState` hook egy **tömböt** ad vissza `[aktuálisÉrték, beállítóFüggvény]` alakban – ezért destrukturáljuk tömbként, nem objektumként:

```ts
// React kódban (csak illusztráció, itt nem futtatható):
// const [szamlalo, setSzamlalo] = useState<number>(0);
```

📌 Éppen ezért fontos érteni a különbséget: **objektum** destrukturálásnál a *név*, **tömb** destrukturálásnál a *sorrend* számít.

---

## 🧠 Megjegyzés

* objektum destrukturálás → `{ mezo } = objektum` (a **név** számít)
* tömb destrukturálás → `[elem1, elem2] = tomb` (a **sorrend** számít)
* mindkettőnél lehet átnevezni, alapértelmezett értéket adni, és `...rest`-tel maradékot gyűjteni
* függvényparaméterben destrukturálva olvashatóbb kód érhető el – ez a React props-kezelés alapmintája
