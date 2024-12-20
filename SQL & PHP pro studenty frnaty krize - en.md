### Basic SQL Commands

#### Let's start by listing a few commands we will need at the beginning:

- **CREATE** - creates a new database or table  
- **DROP** - deletes databases or tables  
- **USE** - switches the "active repository" in the work environment  
- **ALTER** - alters the properties of tables and databases, also used to alter data  
  - **READ ONLY** - a Boolean property of a database that, if set to 1, prevents alterations  
- **SELECT** - selects specified data from a table  
- **FROM** - a specifier for other commands, allowing us to address a specific table or database  
- **RENAME *keyword* TO** - renames tables, databases, and their columns; used together with the ALTER command  
- **MODIFY** - used to change the datatype of a column and other properties of columns; used together with the ALTER command  
- **ADD** - used together with ALTER to add a new column to a table  
- **DATABASE** - keyword specifying the addressed item is a database  
- **TABLE** - keyword specifying the addressed item is a table  
- **COLUMN** - keyword specifying the addressed item is a column in a table  

#### Basic and Common Variable Types:

- **DECIMAL(size, d)** - used to create a decimal number. The size is a number specifying how long the number itself will be (e.g., 1.25 has a size of 3, 123.456 has a size of 6). The "d" argument specifies how many digits can be after the decimal point (e.g., **d = 2**, **size = 5**... 1.23 will work, 1234.3 will also work, but 12.345 won't).   

---

### First Steps: Creating a Database and a Table

SQL is written differently than the programming languages we have encountered so far, as it is primarily written as commands in a command-line environment. It is similar to BASH scripting that we covered in OS classes.  

To create a database, we use the **CREATE** command and the **DATABASE** keyword to specify that we are creating a database. Then, we set it as our working "repository" using the **USE** command.

```sql
CREATE DATABASE DB_name;
USE DB_name;
```

*Note: As you can see, each line ends with a semicolon. Similar to JavaScript or PHP, this indicates the end of the line. Also, note that I use uppercase for commands, but this is just a stylistic choice as SQL is not case-sensitive in its commands. This means the following code is also valid:*

```sql
create database dbName;
Create Database DBname;
```

---

### Deleting a Database or Table

To delete a database or a table within it, we use the **DROP** command.

```sql
DROP TABLE table_name;
DROP DATABASE db_name;
```

*Note: Yes, deleting a database also deletes all the tables contained within it.*  

---

### Renaming Tables and Databases

To rename a table or database, we use the **RENAME TO** command. However, note that **this method does not work for columns**. We'll cover that later.

```sql
RENAME TABLE students TO children_who_study;
RENAME DATABASE myDB TO DB;
```

---

### Creating a Table

After creating a database, we add a table inside it. If the command doesn't work, try running the **USE** command again and ensure the database name is correct. Creating a table uses a command similar to creating a database. Afterward, we specify the columns and their datatypes. In this example, we'll create a table for a school database.

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

To view the columns we created, use this command:

```sql
SELECT * FROM students;
```

*Note: The "*" character addresses everything in the specified table using the **FROM** clause.*

---

### Using the ALTER Command

#### Adding a New Column

To add a column, we use the **ALTER** command with **ADD**. We must specify the altered item using a keyword and its name. Then we follow it with the **ADD** command, the new column name, and its datatype.

```sql
ALTER TABLE students
ADD class VARCHAR(5);
```

*Note: Here, I split the code across multiple lines for better readability. You can also write it on one line if preferred:*  
```sql
ALTER TABLE students ADD class VARCHAR(5);
```

#### Renaming a Column

To rename a column (but **not a database or table**), use the **ALTER** command with the **COLUMN** keyword.

```sql
ALTER TABLE students
RENAME COLUMN email TO student_email;
```

#### Modifying a Datatype

To modify a datatype or adjust its properties, use the **MODIFY** command with **ALTER**.

```sql
ALTER TABLE students
MODIFY email VARCHAR(100);
```

#### Moving a Column

To move a column, use the **MODIFY** command with the **AFTER** keyword. Specify the column with its **exact** datatype (even the brackets must match), and then indicate the column after which it should be placed.

```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
AFTER last_name;
```

To place a column at the first position, use:

```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
FIRST;
```

#### Deleting a Column

To delete a column, use the **DROP** command with **ALTER**.

```sql
ALTER TABLE students
DROP COLUMN parent_phone;
```

---

### Inserting Data into a Table

To insert data into our tables, we use the following commands:

- **INSERT INTO (*columns*)** - allows us to insert data into tables (we will now refer to this as "creating rows"). The argument in parentheses is optional.  
- **VALUES (*values*)**  
- **UPDATE** - updates information in rows, in this regard its similar to ALTER
- **SET** - used with UPDATE command to add or replace data in a specific cell
- DELETE
- ROLLBACK
- COMIT
- **WHERE** - a specifier for selecting data, acting similarly to an if statement 
- **ORDER BY** - used with the SELECT command, allows us to display the information in certain order, cam order alphabetically or numerically
	- **DESC** - can be placed after the ORDER BY argument to achieve descending order
	- **ASC** - can be placed after the ORDER BY argument to achieve ascending order
- **NULL** - used for "deleting" information from a specific row
- AUTOCOMMIT - 

---

### Comparison Operators for WHERE:

Before moving forward, let's list all comparison operators that can be used with the WHERE clause. 

---
*Note: to get in the same situation I'm in to have the same output as me use the following code*
```sql
CREATE DATABASE myDB;
USE myDB;
CREATE TABLE students (
	studentID INT,
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	starting_year DATE,
	class VARCHAR(5),
	mark_average DECIMAL(3, 2)
);

SELECT * FROM students;
```
To add information to the table we are creating we will use INSERT INTO
```sql
INSERT INTO students
VALUES (2, "antonin", "sonka", "2020-09-01", "3.C", 2.5);
```

To add only some information specify it in the brackets after the name of the database
```sql
INSERT INTO students(studentID, first_name, mark_average)
VALUES (1, "vladimir", 1.2);
```

To add multiple values in one command do this
```sql
INSERT INTO students 
VALUES  (3, "vojtech", "kotlik", "2020-09-01", "3.C", 2.7),
		(4, "lucie", "zemanova", "2020-09-01", "3.C", 1.5);
```

Since we added the information that isn't in a proper order we can add the ORDER BY statement to the SELECT command, to order them by their studentID, or we can do that by name, average etc...
```sql
SELECT * FROM students
ORDER BY studentID;
```
That will give us this output:

| studentID | first_name | last_name | starting_year | class | mark_average |
| --------- | ---------- | --------- | ------------- | ----- | ------------ |
| 1         | vladimir   |           |               |       | 1.20         |
| 2         | antonin    | sonka     | 2020-09-01    | 3.C   | 2.50         |
| 3         | vojtech    | kotlik    | 2020-09-01    | 3.C   | 2.70         |
| 4         | lucie      | zemanova  | 2020-09-01    | 3.C   | 1.50         |

The student with ID of 1 has some missing data so we need to update it to not leave it empty. For that we will use UPDATE and SET command. To specify that the data we are changing we will use the where we want to change this information.
```sql
UPDATE students
SET last_name = "shusharin",
	starting_year = "2020-09-01",
	class = "3.C"
WHERE studentID = 1;
```

So now we have this output:

| studentID | first_name | last_name | starting_year | class | mark_average |
| --------- | ---------- | --------- | ------------- | ----- | ------------ |
| 1         | vladimir   | shusharin | 2020-09-01    | 3.C   | 1.20         |
| 2         | antonin    | sonka     | 2020-09-01    | 3.C   | 2.50         |
| 3         | vojtech    | kotlik    | 2020-09-01    | 3.C   | 2.70         |
| 4         | lucie      | zemanova  | 2020-09-01    | 3.C   | 1.50         |

If we want to add and delete more data there is a possibility to make mistakes, for that we will use keyword AUTOCOMMIT and the commands COMIT and ROLLBACK. This will serve as sort of a undo and redo button. But first lets explain the AUTOCOMMIT.
AUTOCOMMIT is a Boolean set to true by default, to set it to false we will use the SET command
```sql
SET AUTOCOMMIT = 0;
```


*i know there is stuff missing so far but this is actually important*

### PHP and SQL together

calling a database - i use xampp at home coz the school one didnt want to work and i was desperate
```php
#databse information  
$db_server = "localhost";  #also can be 127.0.0.1:3306 sometimes
$db_user = "root";  #users can be made in myPhpAdmin but i think this one is default  
$db_pass = "";  # my root didnt require password, but i was using mySQL from XAMPP and its very much possible that school ones will have password
$db_name = "users_db";  # this is a database i made in myPhpAdmin, but im too lazy to describe that (for now, coz its almost 5am)
  
#try to set up connection  
try{  
    $conn = mysqli_connect($db_server, $db_user, $db_pass, $db_name);  
}catch(mysqli_sql_exception $e){  
    echo "connection failed coz $e";  
}  
  
  
#check connection  
if($conn){  
    echo "Connected successfully <br>";  
}
```

to call SQL commands create a variable with the code and use *mysqli_querry(connection,command)* preferably in a try catch in case your SQL fucks up. You will manage the database with mySQL workbench (probably) and phpMyAdmin.
```php
$sql = "SELECT * FROM users";

try {  
    $result = mysqli_query($conn, $sql); #result is an object, that is important coz it defines how we work with it  
    echo "Query successfully executed! <br>";  
}  
catch (mysqli_sql_exception) {  
    echo "Couldn't execute query <br>";  
}  
#if we get an error our try block will catch it
```

in this code we can also use variables, i only had a problem when i used variable for a table, idk why. To use them do this.
```php
$userID = 5;

$sql = "SELECT * FROM users WHERE id = '$userID'";
#... rest is same
```

in the example im using SELECT but you can execute any command you have the permissions to do so (they are set based on your user)

to get output from SELECT command we have to make the result into associative array. That is very similar to normal one but works like this

	normal array: 1:"value 1", 2: "value 2"...
	associative array: "word" => "value 1", "word2" => "value 2"...
	*Yes the arrows are important* 

this is because the "word" will represent the column that the data is stored in. To create this array from $result and print it out do this
```php
if (!strpos($sql, "SELECT") && mysqli_num_rows($result) > 0) {  
    #$row = mysqli_fetch_assoc($result); #returs next available row in a associative array  
    while ($row = mysqli_fetch_assoc($result)) {  
        foreach ($row as $key => $value) {  
            echo $key . ": " . $value . "<br>";  
        } # so proud I'm learning here coz the while isn't mine but the foreach is  
    }  
}  
else {  
    echo "SQL output isn't printable <br>";  
}
```
*Note: the if statement prevents the code outputing empty table, for that is the command mysqli_num_rows(output), that will return ammount of rows in output. The strpos(str, searchedStr) is basically just cool and i wanted to show it off.*

you can see i used the mysqli_fetch_assoc(output) function, that will return the associative array. I also created the local variable $row in the while cycle, that serves two purposes. First is condition for while, because when row cant be created it will stop. Second is for the foreach cycle that just helps me write it down. Foreach will repeat for each element in array, in the brackets i used:

	$row as $key => $value

that can be translated to: "repeat for every key in row and in the value variable store element that comes after key"

to do this in normal for loop you would write:
```php
for($i = 0, $i <= $row.lenght(), $i++) {
	echo $row[i] . ": " . $row[i+1] . "<br>";
	$i++;
}
```
*i think... idk i didnt use php enough yet to know if there exist .lenght().*

more here
[https://www.w3schools.com/php/php_looping_foreach.asp]
