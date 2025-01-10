## Data Selection

### **Basic Commands**

- **SELECT**: Retrieves data specified in the query. It cannot function alone and requires other commands like `FROM`.

- **FROM**: Specifies the source table from which the data will be selected. The table must exist in the current database.

- **WHERE**: Filters results based on specified conditions. Common operators include:

	- `=`: Equal to (works with numbers and words).

	- `<>, !=`: Not equal to.

	- `<, >, <=, >=`: Less than, greater than, and their inclusive forms.

	- `AND, OR`: Combine multiple conditions (all must be true for `AND`, one must be true for `OR`).

	- `LIKE`: Pattern matching (e.g., search for values starting with 's').

		- `%`: Represents zero or more characters (`%r` ends with 'r', `r%` starts with 'r', `%r%` contains 'r').

		- `_`: Matches a single character (`__ll` could match 'wall', 'hall', 'yell').

	- `BETWEEN`: Filters within a range of numbers.

	- `IN`: Filters based on a list of values (e.g., `WHERE column IN (1, 2, 3)`).

- **ORDER BY**: Sorts query results by a specified column.

	- `DESC`: Descending order (default).

	- `ASC`: Ascending order.

- **AVG**: Calculates the average of numeric values (works only on `INT` columns).


---

### **Examples**

#### Selecting all data from a table:

```mysql
SELECT * FROM users;
```

_Note: The_ `*` _selects all columns._

#### Selecting specific columns:

```mysql
SELECT username, post_amount, follower_count FROM users;
```

#### Filtering results using `WHERE`:

```mysql
SELECT username, post_amount, follower_count FROM users WHERE username = 'Rene';
```

#### Searching with `LIKE` and ordering results:

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

## Join Operations

Joins combine rows from two or more tables based on related columns. Common types include:

### **INNER JOIN**

Returns rows where there is a match in both tables.

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

_Example use case: Match orders with their respective companies._

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

Returns all rows from the left table and matched rows from the right table. Unmatched rows from the right table will be `NULL`.

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

Returns all rows from the right table and matched rows from the left table. Unmatched rows from the left table will be `NULL`.

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

Returns all rows when there is a match in either table. Unmatched rows will have `NULL` values.

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

## Additional Notes

- Use **AS** to create aliases for tables or columns for better readability.

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

---
## Practical use example

First of all, this is the only way to export values from SQL but let me show you just one (of many) examples in PHP. For this example I will just do elementary example where you need to find a password and username that user wrote. I will show you less effective but valid way and more effective one.

**Less effective**

```php

// connection
include 'connect.php';

$sql = "SELECT username, password FROM users"; // sql command to get all usernames and passwords

// this will give us the output from form

$password = $_POST['password'];

$username = $_POST['username'];

// in try catch just in case
try {
	$result = mysqli_query($conn, $sql); // push sql command and get the output into the $result variable
}

catch (mysqli_sql_exception) {
	echo "command failed";
}

// checks
$correctPassword = false;
$correctUsername = false;

// "unpack" the result as a associative array

while($row = mysqli_fetch_assoc($result)) { // so if there are no rows the while ends
	foreach($row as $key => $value) { // this will go through the array element by element (how cool)
		if($key == "username" && $value == $username) { // if the key will be username (first repeat) and matches the user input set the check to true
			$correctUsername = true;
		}
		if($key == "password" && $value == $password) { // the same but for password
			$correctPassword = true;
		}
	}
}

// check for the checks

if ($correctPassword && $correctUsername) {
	echo "user found!";
}

else {
	echo "user not found";
}

```

now the **more effective**

```php

// connection

include 'connect.php';

// this will give us the output from form
$password = $_POST['password'];
$username = $_POST['username'];

$sql = "SELECT username, password FROM users WHERE password = '$password' AND username = '$username'"; // sql command to get only the user input values

// if there is no result the user doesnt exist

try {
	$result = mysqli_query($conn, $sql);
	if (mysqli_num_rows($result) > 0) {
		echo "user exist";
	}
}
catch(mysqli_sql_exception) {
	echo "search failed";
}

```
*Note: you can write alternative code that can be easier to read by using the UNION command that allows you to run multiple SELECT commands at once. So the code could look like this*
`SELECT username FROM users WHERE username = '$username'`
`UNION`
`SELECT password FROM users WHERE password = '$password'`

---

# Data creation

Before we truly start, let me just quickly show you how to make a database, use it to create a table and fill that table with some values. After that I will go a bit deeper into it but this is just a quick course if you don't want to scroll too far.

```mysql
CREATE DATABASE database_name; -- create a database

USE database_name; -- choose it as a "active repository" for our work

CREATE TABLE table_name( -- create a table and put columns in it
	column_one INT, -- this column is called column_one and will have numbers inside
	column_two VARCHAR(50), -- this one has up to 50 characters inside
	column_three DATE, -- this one will have a date writtien in YYYY-MM-DD
	column_four DATETIME -- this one will have date and time written like YYYY-MM-DD HH:MM:SS
);

INSERT INTO table_name
VALUES (1, "value","2000-01-01", "2000-00-00 00:00:00"); -- put in the example value

SELECT * FROM table_name; -- select all data from the table
```


So the actual first chapter of data creation:

- **CREATE** - command used for creating databases and tables
    - **DATABASE** - storage for our tables
    - **TABLE** - storage for the actual values in our databases
- **PRIMARY KEY** - 
- **FOREIGN KEY** -
- **h
- **NOT NULL** -
- **UNIQUE** -
- 
- **USE** - this command will select the "active repository", basically until we choose what database are we working in we cant make and fill tables
- **DROP** - used for deleting stuff (by stuff I mean a lot of things so it will remain stuff until I get to it)
- **DELETE** -
- **ADD** -
- **INSERT INTO** -
- **VALUES** - 
    - **VARCHAR** - used to store string values, pay attention to the fact that if we put a number here it won't be a number, or have its values, just a symbol for it (this is important for the `WHERE` command)
    - **TEXT** - for bigger and only text values, I never understood what's the difference between this and `VARCHAR` but my bet is on maximum size
    - **BLOB** - for big values, sounds weird but together with `BIGBLOB` can store stuff like pictures, audio maybe even video
    - **INT** - stores numerical values, but not decimal values (works the same as the one we know from programming)
    - **BIGINT** - stores very big numbers (so if your number doesn't work try this one)
    - **DECIMAL** - stores decimal values, to be specific, exact decimal values, because the first number decides the full length of the number (amount of numbers, not maximum size) and second argument will set a limit of the numbers after the decimal point `DECIMAL (4, 2)` can do number (12.34 but not 123.4)
    - **FLOAT/DOUBLE** - unlike DECIMAL these two store undefined decimal numbers, in brackets we only specify the complete size of the number
    - **DATE** - stores only date `YYYY-MM-DD`
    - **DATETIME** - stores time and date `YYYY-MM-DD HH:MM:SS`, the CURRENT_TIMESTAMP will fill it up automatically with current timestamp (that being the time of adding that value to the table)
    - **TIME** - stores only time in format `HH:MM:SS`
    - **BOOL/BOOLEAN** - sets the column to store only Boolean values
    - **JSON** - will store JSON data so the data has to be in JSON format(`{'key': 'value', 'key2': ['value2', 'value3'] ...} `)
    - **ENUM** - acts as a list of possible values in the column, used when exact values are required `ENUM (ver1, ver2, ver3, ...)`

Now when we have commands, lets start with the actual database creation. As I said before, the database will actually just contain our databases (I don't think i will ever start to talk about permissions and users here so lets stick with that).
To make a database use CREATE command together with the DATABASE keyword followed by the name of the database itself. If you wish to use this database follow this line with the USE command with the database name as its argument.
```mysql
CREATE DATABASE database_name;
USE database_name;
```
If you wish to delete this database use the DROP command.
```mysql
DROP DATABASE database_name;
```

Same process will be followed for creating and deleting tables. But with special addition for creating tables.
```mysql
CREATE TABLE table_name (
    column_one INT,
    column_two VARCHAR(45),
    column_three JSON
    -- etc...
);

DROP TABLE table_name;
```
So to kinda explain what i did there. After the commands I defined the columns for our new table.