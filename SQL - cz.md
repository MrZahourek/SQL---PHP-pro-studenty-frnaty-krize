## Výběr dat

### **Základní příkazy**

- **SELECT**: Získává data určená v dotazu. Nemůže fungovat samostatně a vyžaduje další příkazy jako `FROM`.

- **FROM**: Specifikuje zdrojovou tabulku, ze které budou data vybrána. Tabulka musí existovat v aktuální databázi.

- **WHERE**: Filtruje výsledky na základě určených podmínek. Běžné operátory zahrnují:

    - `=`: Rovná se (funguje s čísly i slovy).

    - `<>, !=`: Nerovná se.

    - `<, >, <=, >=`: Menší než, větší než a jejich inkluzivní formy.

    - `AND, OR`: Kombinace více podmínek (všechny musí být pravdivé pro `AND`, jedna musí být pravdivá pro `OR`).

    - `LIKE`: Porovnání podle vzoru (např. hledání hodnot začínajících na 's').

        - `%`: Reprezentuje nula nebo více znaků (`%r` končí na 'r', `r%` začíná na 'r', `%r%` obsahuje 'r').

        - `_`: Odpovídá jednomu znaku (`__ll` může odpovídat 'wall', 'hall', 'yell').

    - `BETWEEN`: Filtruje v rámci číselného rozsahu.

    - `IN`: Filtruje na základě seznamu hodnot (např. `WHERE column IN (1, 2, 3)`).

- **ORDER BY**: Třídí výsledky dotazu podle specifikovaného sloupce.

    - `DESC`: Sestupné pořadí (výchozí).

    - `ASC`: Vzestupné pořadí.

- **AVG**: Vypočítá průměr číselných hodnot (funguje pouze na sloupcích `INT`).


---

### **Příklady**

#### Výběr všech dat z tabulky:

```mysql
SELECT * FROM users;
```

_Poznámka: `*` vybírá všechny sloupce._

#### Výběr specifických sloupců:

```mysql
SELECT username, post_amount, follower_count FROM users;
```

#### Filtrání výsledků pomocí `WHERE`:

```mysql
SELECT username, post_amount, follower_count FROM users WHERE username = 'Rene';
```

#### Hledání pomocí `LIKE` a řazení výsledků:

```mysql
SELECT
    username, nickname, follower_amount
FROM
    users
WHERE
    username LIKE 'Ren%' OR username LIKE 'ren%'
ORDER BY
    follower_amount DESC;
```

---

## Operace Join

Joins kombinují řádky ze dvou nebo více tabulek na základě souvisejících sloupců. Běžné typy zahrnují:

### **INNER JOIN**

Vrácí řádky, kde existuje shoda v obou tabulkách.

```mysql
SELECT
    a.id AS left_id,
    a.name AS left_name,
    b.id AS right_id,
    b.name AS right_name
FROM
    table_a a
INNER JOIN
    table_b b
ON
    a.id = b.id;
```

_Příklad použití: Spojení objednávek s jejich příslušnými společnostmi._

```mysql
SELECT
    orders.id AS order_id,
    orders.name AS order_name,
    company.id AS company_id,
    company.name AS company_name
FROM
    orders
INNER JOIN
    company
ON
    orders.company_id = company.id;
```

### **LEFT JOIN**

Vrácí všechny řádky z levé tabulky a odpovídající řádky z pravé tabulky. Neodpovídající řádky z pravé tabulky budou `NULL`.

```mysql
SELECT
    a.id AS left_id,
    b.name AS right_name
FROM
    table_a a
LEFT JOIN
    table_b b
ON
    a.id = b.id;
```

### **RIGHT JOIN**

Vrácí všechny řádky z pravé tabulky a odpovídající řádky z levé tabulky. Neodpovídající řádky z levé tabulky budou `NULL`.

```mysql
SELECT
    a.id AS left_id,
    b.name AS right_name
FROM
    table_a a
RIGHT JOIN
    table_b b
ON
    a.id = b.id;
```

### **FULL JOIN**

Vrácí všechny řádky, pokud existuje shoda v některé tabulce. Neodpovídající řádky budou mít hodnoty `NULL`.

```mysql
SELECT
    a.id AS left_id,
    b.name AS right_name
FROM
    table_a a
FULL JOIN
    table_b b
ON
    a.id = b.id;
```

---

## Další poznámky

- Používejte **AS** k vytváření aliasů pro tabulky nebo sloupce pro lepší čitelnost.

    ```mysql
    SELECT
        a.id AS left_id,
        b.id AS right_id
    FROM
        table_a AS a
    INNER JOIN
        table_b AS b
    ON
        a.id = b.id;
    ```

