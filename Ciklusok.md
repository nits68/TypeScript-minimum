
# TypeScript - Ciklusok (iteráció)

## 1️⃣ for ciklus (számlálós)

Ez a leggyakrabban használt ciklus, ha előre tudjuk, hányszor kell lefuttatni a kódot.

### Alap szintaxis

```ts
for (kezdeti_érték; feltétel; léptetés) {
    // kód, ami ismétlődik
}

```

### Példa

```ts
for (let i: number = 0; i < 5; i++) {
    console.log("A számláló értéke: " + i);
}

```

---

## 2️⃣ while ciklus (elöltesztelős)

Akkor használjuk, ha nem tudjuk előre a lépések számát, csak egy feltételhez kötjük a futást. Először ellenőrzi a feltételt, és csak utána fut le.

### Alap szintaxis

```ts
while (feltétel) {
    // kód, ami addig ismétlődik, amíg a feltétel igaz
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

## 3️⃣ do-while ciklus (hátultesztelős)

Hasonlít a while ciklushoz, de ez **legalább egyszer mindenképpen lefut**, mivel a feltételt a ciklusmag után ellenőrzi.

### Alap szintaxis

```ts
do {
    // ez egyszer mindenképp lefut
} while (feltétel);

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

## 4️⃣ for-of ciklus (érték szerinti bejárás)

Modern és kényelmes megoldás tömbök (vagy más iterálható objektumok) elemeinek bejárására. Az **értékeket** adja vissza. Akkor használjuk jellemzően, ha nincs szükség az indexek értékeire.

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

Kihagyja a jelenlegi kört, és ugrik a következőre.

```ts
for (let i: number = 0; i < 5; i++) {
    if (i === 2) {
        continue; // A 2-est nem írja ki, de folytatja 3-mal
    }
    console.log(i);
}

```


## 7️⃣ `forEach()` metódus (tömbök bejárása)

A `forEach()` egy tömbmetódus, amely végigmegy a tömb minden elemén, és egy megadott függvényt hajt végre rajtuk.

Ez **nem klasszikus ciklus**, hanem egy **metódus**, amely egy **callback függvényt** kap paraméterként.

### Alap szintaxis

```ts
tomb.forEach((elem[, index]) => {
    // kód
});
```

* `elem` – az aktuális elem értéke
* `index` – az aktuális elem indexe (nem elhagyható)

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

Ha **meg kell szakítani a ciklust**, akkor inkább:

* `for`
* `for-of`

ciklust érdemes használni.

---
