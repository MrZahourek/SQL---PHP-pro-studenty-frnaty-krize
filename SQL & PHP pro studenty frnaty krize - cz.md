### Základní příkazy v SQL

#### Začněme tím, že si uvedeme několik příkazů, které budeme na začátek potřebovat:

- **CREATE** - vytvoří novou databázi nebo tabulku  
- **DROP** - odstraní databáze nebo tabulky  
- **USE** - přepne "aktivní úložiště" v pracovním prostředí  
- **ALTER** - mění vlastnosti tabulek a databází, používá se také k úpravě dat  
  - **READ ONLY** - Booleovská vlastnost databáze, která, pokud je rovna 1, zabrání jakýmkoliv úpravám  
- **SELECT** - vybírá specifikovaná data z tabulky  
- **FROM** - specifikátor pro jiné příkazy, umožňuje adresovat konkrétní tabulku nebo databázi  
- **WHERE** - specifikátor pro výběr dat, funguje podobně jako podmínka "if"  
- **RENAME *klíčové_slovo* TO** - přejmenuje tabulky, databáze a jejich sloupce; používá se spolu s příkazem ALTER  
- **MODIFY** - mění datový typ sloupce a další vlastnosti sloupců; používá se spolu s příkazem ALTER  
- **ADD** - přidává nový sloupec do tabulky, používá se spolu s příkazem ALTER  
- **DATABASE** - klíčové slovo označující, že adresovaný prvek je databáze  
- **TABLE** - klíčové slovo označující, že adresovaný prvek je tabulka  
- **COLUMN** - klíčové slovo označující, že adresovaný prvek je sloupec v tabulce  

#### Základní a nejčastější typy proměnných:

- **DECIMAL(size, d)** - používá se pro vytvoření desetinného čísla. Parametr "size" určuje celkovou délku čísla (např. 1.25 má délku 3, 123.456 má délku 6). Parametr "d" určuje počet desetinných míst (např. **d = 2**, **size = 5**... 1.23 bude fungovat, 1234.3 bude také fungovat, ale 12.345 nebude).  

---

### Porovnávací operátory pro příkaz WHERE:

Před pokračováním si uveďme všechny porovnávací operátory, které lze použít s příkazem WHERE.  

---

### První kroky: Vytvoření databáze a tabulky

SQL se píše jinak než jazyky, na které jsme zatím zvyklí, protože ho píšeme pouze jako příkazy v příkazovém řádku. Podobá se to BASH skriptování, které jsme dělali na hodinách operačních systémů.  

K vytvoření databáze použijeme příkaz **CREATE** a klíčové slovo **DATABASE**, abychom specifikovali, že vytváříme databázi. Poté ji nastavíme jako naše pracovní "úložiště" pomocí příkazu **USE**.

```sql
CREATE DATABASE DB_name;
USE DB_name;
```

*Poznámka: Jak můžete vidět, každý řádek končí středníkem. Podobně jako v JavaScriptu nebo PHP to označuje konec řádku. Také si můžete všimnout, že příkazy píšu velkými písmeny, což je však pouze stylistická volba, protože SQL není na velká a malá písmena citlivé. Takže i tento kód je funkční a platný:*

```sql
create database dbName;
Create Database DBname;
```

---

### Odstranění databáze nebo tabulky

Pokud chceme odstranit databázi nebo tabulku v ní, použijeme příkaz **DROP**.

```sql
DROP TABLE table_name;
DROP DATABASE db_name;
```

*Poznámka: Pokud smažete databázi, smažete tím také všechny tabulky, které obsahovala.*

---

### Přejmenování tabulek a databází

K přejmenování tabulky nebo databáze použijeme příkaz **RENAME TO**, ale nezapomeňte: **tato metoda nefunguje pro sloupce**. K tomu se dostaneme později.

```sql
RENAME TABLE students TO children_who_study;
RENAME DATABASE myDB TO DB;
```

---

### Vytvoření tabulky

Po vytvoření databáze do ní přidáme tabulku. Pokud příkaz nefunguje, zkuste znovu použít příkaz **USE** a ujistěte se, že název databáze je správný. K vytvoření tabulky použijeme podobný příkaz jako při vytváření databáze. Poté specifikujeme sloupce a jejich datové typy. V tomto příkladu vytvoříme tabulku pro databázi školy.

```sql
CREATE TABLE students (
    studentID INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    mark_average DECIMAL(3, 2),
    parent_phone INT,
    email VARCHAR(45)
);
```

Pro zobrazení sloupců, které jsme vytvořili, použijeme tento příkaz:

```sql
SELECT * FROM students;
```

*Poznámka: Znak "*" označuje všechno v tabulce specifikované příkazem **FROM**.*

---

### Použití příkazu ALTER

#### Přidání nového sloupce

K přidání sloupce použijeme příkaz **ALTER** spolu s příkazem **ADD**. Musíme specifikovat upravovaný prvek pomocí klíčového slova a jeho názvu. Poté přidáme příkaz **ADD**, za ním název nového sloupce a jeho datový typ.

```sql
ALTER TABLE students
ADD class VARCHAR(5);
```

*Poznámka: Zde jsem kód rozepsal na více řádků kvůli čitelnosti. Pokud chcete, můžete ho napsat i na jeden řádek:*  
```sql
ALTER TABLE students ADD class VARCHAR(5);
```

#### Přejmenování sloupce

K přejmenování sloupce (ale **ne databáze nebo tabulky**) použijeme příkaz **ALTER** spolu s klíčovým slovem **COLUMN**.

```sql
ALTER TABLE students
RENAME COLUMN email TO student_email;
```

#### Změna datového typu

K úpravě datového typu nebo jeho vlastností použijeme příkaz **MODIFY** spolu s příkazem **ALTER**.

```sql
ALTER TABLE students
MODIFY email VARCHAR(100);
```

#### Přesunutí sloupce

K přesunutí sloupce použijeme příkaz **MODIFY** spolu s klíčovým slovem **AFTER**. Musíme specifikovat sloupec s jeho **přesným** datovým typem a poté uvést, za který sloupec jej chceme přesunout.

```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
AFTER last_name;
```

Pokud chceme sloupec umístit na první místo, napíšeme:

```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
FIRST;
```

#### Smazání sloupce

K odstranění sloupce použijeme příkaz **DROP** spolu s **ALTER**.

```sql
ALTER TABLE students
DROP COLUMN parent_phone;
```

---

### Vkládání dat do tabulky

K tomu použijeme příkazy:

- **INSERT INTO (*sloupce*)** - umožňuje vložit data do tabulek (od této chvíle to nazýváme vytvořením řádků). Argument v závorkách není povinný.  
- **VALUES (*hodnoty*)**  

---
