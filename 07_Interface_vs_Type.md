# TypeScript – Interface vs. Type

A TypeScriptben egy objektum "alakját" (szerkezetét) kétféleképpen is leírhatjuk: `interface`-szel vagy `type`-pal. Kezdőknek ez az egyik leggyakoribb zavarforrás, mert a két eszköz sokszor ugyanazt tudja.

---

## 1️⃣ Alap szintaxis

### interface

```ts
interface Auto {
    marka: string;
    evjarat: number;
}
```

### type

```ts
type Auto = {
    marka: string;
    evjarat: number;
};
```

### Használatuk teljesen egyforma

```ts
let kocsi: Auto = {
    marka: "Toyota",
    evjarat: 2022
};
```

📌 Egyszerű objektum-szerkezetek leírására **mindkettő ugyanúgy jó**, a választás sokszor stílus kérdése.

---

## 2️⃣ Amiben egyformák

* Opcionális tulajdonság (`?`) mindkettőnél működik.
* `readonly` mező mindkettőnél működik.
* Osztály (`class`) mindkettőt tudja implementálni (`implements`).
* Függvénytípust mindkettővel le lehet írni.

```ts
interface SzemelyI {
    nev: string;
    readonly szuletesiEv: number;
    beosztas?: string;
}

type SzemelyT = {
    nev: string;
    readonly szuletesiEv: number;
    beosztas?: string;
};
```

---

## 3️⃣ Amiben különböznek

### 3.1 Kiterjesztés (öröklés)

Az `interface` az `extends` kulcsszóval bővíthető, a `type` `&` (intersection, metszet) operátorral "fűzhető össze".

```ts
// interface bővítése
interface Allat {
    nev: string;
}

interface Kutya extends Allat {
    fajta: string;
}

// type bővítése (intersection)
type AllatT = {
    nev: string;
};

type KutyaT = AllatT & {
    fajta: string;
};
```

### 3.2 Union (unió) típus

Uniót **csak `type`-pal** lehet létrehozni, `interface`-szel nem.

```ts
type Lampaszin = "piros" | "sárga" | "zöld";
type Azonosito = string | number;
```

### 3.3 Elsődleges (primitív), tuple, függvény típusok elnevezése

Ha nem objektumot, hanem pl. egy uniót, egy tuple-t vagy egy primitív típus szinonimáját szeretnénk elnevezni, csak `type` jöhet szóba.

```ts
type Koordinata = [number, number]; // tuple elnevezése
type Eves = number;                 // primitív típus "beceneve"
```

### 3.4 Deklaráció-összevonás (declaration merging)

Ha **kétszer** deklarálunk egy `interface`-t ugyanazzal a névvel, TypeScript automatikusan **összevonja** a mezőiket. `type`-nál ez hibát adna (nem lehet kétszer ugyanazt a nevet deklarálni).

```ts
interface Config {
    nev: string;
}

interface Config {
    verzio: number;
}

// A Config innentől { nev: string; verzio: number } alakú
const c: Config = { nev: "App", verzio: 1 };
```

---

## 4️⃣ Melyiket válasszam?

Nincs egységes szabály, de a gyakorlatban elterjedt ökölszabályok:

* **Objektum "alakok" leírására** (pl. egy `Auto`, egy `Felhasznalo` szerkezete) mindkettő jó – sok csapat konvencióból az `interface`-t részesíti előnyben, mert jobban bővíthető és a hibaüzenetek is olvashatóbbak lehetnek.
* **Ha unió típusra, tuple-ra vagy függvénytípusra van szükség**, akkor **kötelezően** `type` kell.
* **Ha könyvtárat/API-t készítesz**, amit mások bővíthetnek, az `interface` deklaráció-összevonása hasznos lehet.

> Ebben a jegyzetsorozatban a legtöbb helyen `type`-ot használunk, mert ez rugalmasabb (union, tuple is leírható vele) – de mindkettőt látnod és értened kell, mert mindkettővel találkozni fogsz kész kódban.

---

## 🧠 Megjegyzés

* `interface` → bővíthető (`extends`, deklaráció-összevonás), csak objektum-alakra
* `type` → mindenre jó (union, tuple, primitív, objektum), de nem vonható össze duplikált névvel
* egyszerű objektum-szerkezetnél a kettő **felcserélhető**
