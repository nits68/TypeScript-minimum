# TypeScript – Utility típusok (Partial, Pick, Omit, Record, Readonly)

A TypeScript beépített "segéd-típusokat" (utility types) biztosít, amelyekkel **meglévő típusokból** hozhatunk létre új típusváltozatokat, kódduplikálás nélkül. Ezek a típusok folyamatosan előkerülnek Prisma generált modelljeivel, React props-okkal és API-válaszokkal dolgozva.

---

## 1️⃣ A probléma, amit megoldanak

Tegyük fel, van egy alap típusunk:

```ts
type Felhasznalo = {
    id: number;
    nev: string;
    email: string;
    jelszo: string;
};
```

Egy űrlap-frissítésnél lehet, hogy **nem minden mező kötelező** (`Partial`), egy listázásnál lehet, hogy a `jelszo`-t **nem akarjuk visszaadni** (`Omit`), egy bejelentkezési formhoz pedig **csak** az `email` és `jelszo` kell (`Pick`). Enélkül a fejlesztő ilyenkor 3 külön típust írna kézzel – az utility típusok ezt automatizálják.

---

## 2️⃣ `Partial<T>` – minden mező opcionálissá tétele

Az összes mezőt `?`-essé (opcionálissá) teszi.

```ts
type FelhasznaloFrissites = Partial<Felhasznalo>;
// Ezzel egyenértékű:
// type FelhasznaloFrissites = {
//     id?: number;
//     nev?: string;
//     email?: string;
//     jelszo?: string;
// };

function felhasznaloFrissit(id: number, adatok: Partial<Felhasznalo>): void {
    console.log(`Frissítés: id=${id}`, adatok);
}

felhasznaloFrissit(1, { nev: "Új Név" }); // csak a nev mezőt küldjük, a többi opcionális
```

---

## 3️⃣ `Required<T>` – minden mező kötelezővé tétele

A `Partial` ellentéte: minden opcionális (`?`) mezőt kötelezővé tesz.

```ts
type Beallitas = { tema?: string; betumeret?: number };

type TeljesBeallitas = Required<Beallitas>;
// { tema: string; betumeret: number } – mindkettő kötelező innentől
```

---

## 4️⃣ `Pick<T, Kulcsok>` – csak bizonyos mezők kiválasztása

Egy meglévő típusból **csak a megadott** mezőket tartja meg.

```ts
type BejelentkezesAdat = Pick<Felhasznalo, "email" | "jelszo">;
// { email: string; jelszo: string }

function bejelentkezes(adat: BejelentkezesAdat): void {
    console.log(`Bejelentkezés: ${adat.email}`);
}

bejelentkezes({ email: "anna@example.com", jelszo: "titkos123" });
```

---

## 5️⃣ `Omit<T, Kulcsok>` – bizonyos mezők kihagyása

A `Pick` ellentéte: mindent megtart, **kivéve** a megadott mezőket. Nagyon gyakori, ha érzékeny adatot (pl. jelszót) nem akarunk visszaadni.

```ts
type FelhasznaloNyilvanos = Omit<Felhasznalo, "jelszo">;
// { id: number; nev: string; email: string }

function felhasznaloMegjelenit(felhasznalo: FelhasznaloNyilvanos): void {
    console.log(`${felhasznalo.nev} (${felhasznalo.email})`);
}

// Jelszó nélkül biztonságosan átadható más rétegnek (pl. a felhasználói felületnek)
```

---

## 6️⃣ `Record<Kulcsok, Ertek>` – kulcs-érték típusú objektum leírása

Olyan objektum típusát írja le, amelynek a kulcsai egy meghatározott halmazból jönnek, az értékei pedig egységes típusúak.

```ts
type Jegy = "jeles" | "jó" | "közepes" | "elégséges" | "elégtelen";

type JegyPontszam = Record<Jegy, number>;

const pontszamok: JegyPontszam = {
    "jeles": 5,
    "jó": 4,
    "közepes": 3,
    "elégséges": 2,
    "elégtelen": 1
};

console.log(pontszamok["jeles"]); // 5
```

📌 A `Record<string, number>` gyakorlatilag ugyanazt fejezi ki, mint egy egyszerű "szótár" objektum, de típusosan.

---

## 7️⃣ `Readonly<T>` – minden mező csak olvasható

Minden mezőt `readonly`-vá tesz, így az objektum létrehozás után nem módosítható.

```ts
type FelhasznaloReadonly = Readonly<Felhasznalo>;

const rendszergazda: FelhasznaloReadonly = {
    id: 1,
    nev: "Admin",
    email: "admin@example.com",
    jelszo: "titkos"
};

// rendszergazda.nev = "Más Név"; // HIBA! readonly mezőt nem lehet módosítani
```

---

## 8️⃣ Kombinálás – az utility típusok egymásba ágyazhatók

```ts
type FelhasznaloListaelem = Readonly<Omit<Felhasznalo, "jelszo">>;
// { readonly id: number; readonly nev: string; readonly email: string }
```

---

## 9️⃣ Kitekintés – miért fontos ez Prisma/React kontextusban?

* **Prisma** minden adatbázis-modellhez (pl. `User`) automatikusan generál egy TypeScript típust. Ha csak bizonyos mezőket akarunk visszaadni egy API válaszban, tipikusan `Omit`/`Pick`-et használunk rá, ahelyett hogy kézzel írnánk újra a típust.
* **React** komponenseknél gyakori, hogy egy "szerkesztő" komponens props-ja egy meglévő adattípus `Partial<T>` változata (mert szerkesztéskor még nincs minden mező kitöltve), egy "megjelenítő" komponensé pedig `Readonly<T>` (mert csak megjelenít, nem módosít).

---

## 🧠 Megjegyzés

* `Partial<T>` → minden mező opcionális
* `Required<T>` → minden mező kötelező
* `Pick<T, "a" | "b">` → csak a megadott mezők
* `Omit<T, "a" | "b">` → minden mező, a megadottak kivételével
* `Record<K, V>` → egységes típusú kulcs-érték objektum leírása
* `Readonly<T>` → minden mező csak olvasható
* ezek egymásba is ágyazhatók (pl. `Readonly<Omit<T, "jelszo">>`)
