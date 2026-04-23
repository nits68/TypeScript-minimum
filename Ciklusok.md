
# TypeScript - Ciklusok (iteráció)
Az iteráció egy olyan vezérlési szerkezet, ami egy vagy több utasítást ismétel.

## 1️⃣ for ciklus (számláló vezérelt)

Akkor hasznljuk, ha előre tudjuk, hogy hányszor kell ismételni a kódot,<br>
vagy egy összetett adatszerkezet bejárásakor szükségünk van az elemek indexeire.

### Alap szintaxis

```ts
for (kezdeti_érték; ciklusfeltétel; léptetés) {
    // Ciklusmag (CM): kód, ami ismétlődik
}

```

### Példa

```ts
for (let counter: number = 0; counter < 5; counter++) {
    console.log(`A számláló értéke:  ${counter}`);
}

```

---

## 2️⃣ while ciklus (elöltesztelő ciklus)

Akkor használjuk, ha nem tudjuk előre a lépések számát, csak egy feltételhez kötjük a futást.<br>
Először ellenőrzi a feltételt, és csak utána fut le ha ciklusfeltétel igaz.<br>
Lehet olyan eset, hogy a ciklusmag egyszer sem fut le.

### Alap szintaxis

```ts
while (ciklusfeltétel) {
    // kód, ami addig ismétlődik, amíg a ciklusfeltétel igaz értékű
}

```

### Példa

```ts
let szamlalo: number = 10;

while (szamlalo > 0) {
    console.log(szamlalo);
    szamlalo--; // Csökkentjük az értéket
}

```

---

## 3️⃣ do-while ciklus (hátultesztelő ciklus)

Hasonlít a while ciklushoz, de itt a ciklusmag ** egyszer mindenképpen lefut**, mivel a ciklusfeltételt a ciklusmag után ellenőrzi.

### Alap szintaxis

```ts
do {
    // ez egyszer mindenképp lefut
} while (ciklusfeltétel);

```

### Példa

```ts
let dobas: number;

do {
    dobas = Math.floor(Math.random() * 6) + 1;
    console.log("Dobott szám: " + dobas);
} while (dobas !== 6); // Addig dobálunk, amíg 6-ost nem kapunk

```

---

## 4️⃣ for-of ciklus (érték szerinti bejárás, C# foreach)

Modern és kényelmes megoldás tömbök (vagy más iterálható objektumok) elemeinek bejárására.<br>
A ciklus változója az **értékeket** veszi fel a tömbből. Akkor használjuk jellemzően, ha nincs szükség az indexek értékeire.

### Alap szintaxis

```ts
for (const elem of tomb) {
    // kód
}

```

### Példa

```ts
let gyumolcsok: string[] = ["Alma", "Körte", "Szilva"];

for (const gyumolcs of gyumolcsok) {
    console.log(gyumolcs);
}

```

---

## 5️⃣ for-in ciklus (kulcs/index szerinti bejárás)

Objektumok tulajdonságainak vagy tömbök indexeinek bejárására szolgál. A **kulcsokat (vagy indexeket)** adja vissza.

### Alap szintaxis

```ts
for (const kulcs in objektum) {
    // kód
}

```

### Példa

```ts
let autok: string[] = ["Audi", "BMW", "Mercedes"];

for (const index in autok) {
    console.log(index + ": " + autok[index]); 
    // Kimenet pl: "0: Audi"
}

```

---

## 6️⃣ Vezérlő utasítások

A ciklusok futását befolyásoló kulcsszavak.

### break (megszakítás)

Azonnal kilép a ciklusból.

```ts
for (let i: number = 0; i < 10; i++) {
    if (i === 5) {
        break; // Itt megáll a ciklus
    }
    console.log(i);
}

```

### continue (következő iteráció)

Kihagyja a jelenlegi "kört", és ugrik a következőre.

```ts
for (let i: number = 0; i < 5; i++) {
    if (i === 2) {
        continue; // A 2-est nem írja ki, de folytatja 3-mal
    }
    console.log(i);
}

```


## 7️⃣ `forEach()` metódus (tömbök bejárása, nem ciklus!)

A `forEach()` egy tömb-metódus, amely végigmegy a tömb minden elemén, és egy megadott függvényt hajt végre rajtuk.

Ez **nem klasszikus ciklus**, hanem egy **metódus**, amely egy **callback függvényt** kap paraméterként.

### Alap szintaxis

```ts
tomb.forEach((elem[, index]) => {
    // kód
});
```

* `elem` – az aktuális elem értéke
* `index` – az aktuális elem indexe (elhagyható)

---

### Példa

```ts
let szamok: number[] = [10, 20, 30];

szamok.forEach((ertek, index) => {
    console.log(index + ": " + ertek);
});
```

Kimenet:

```
0: 10
1: 20
2: 30
```

---

### Fontos különbségek a ciklusokhoz képest

A `forEach()` **nem működik teljesen úgy, mint egy ciklus**.

❗ **Nem használható benne:**

* `break`
* `continue`
* `return` a ciklus megszakítására

A `return` csak a callback függvényből lép ki, **nem állítja meg a forEach futását**.

---

### Példa – a `return` nem szakítja meg a bejárást

```ts
let szamok: number[] = [1, 2, 3, 4];

szamok.forEach((szam) => {
    if (szam === 3) {
        return; // csak a callbackből lép ki
    }
    console.log(szam);
});
```

Kimenet:

```
1
2
4
```

A ciklus **nem áll meg**, csak a 3-as elem feldolgozása marad ki.

---

### Mikor érdemes használni?

A `forEach()` akkor jó választás, ha:

* egy tömb **minden elemén végre akarunk hajtani valamit**
* **nem kell megszakítani** a bejárást
* nincs szükség bonyolult vezérlésre

Ha **meg kell szakítani a ciklust**, akkor inkább használd:

* `for`
* `for-of`
---
