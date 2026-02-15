
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
