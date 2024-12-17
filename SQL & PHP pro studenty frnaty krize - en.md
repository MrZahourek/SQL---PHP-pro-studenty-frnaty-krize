### Basic SQL Commands

#### Let's start by listing a few commands we will need at the beginning:

- **CREATE** - creates a new database or table  
- **DROP** - deletes databases or tables  
- **USE** - switches the "active repository" in the work environment  
- **ALTER** - alters the properties of tables and databases, also used to alter data  
  - **READ ONLY** - a Boolean property of a database that, if set to 1, prevents alterations  
- **SELECT** - selects specified data from a table  
- **FROM** - a specifier for other commands, allowing us to address a specific table or database  
- **WHERE** - a specifier for selecting data, acting similarly to an if statement  
- **RENAME *keyword* TO** - renames tables, databases, and their columns; used together with the ALTER command  
- **MODIFY** - used to change the datatype of a column and other properties of columns; used together with the ALTER command  
- **ADD** - used together with ALTER to add a new column to a table  
- **DATABASE** - keyword specifying the addressed item is a database  
- **TABLE** - keyword specifying the addressed item is a table  
- **COLUMN** - keyword specifying the addressed item is a column in a table  

#### Basic and Common Variable Types:

- **DECIMAL(size, d)** - used to create a decimal number. The size is a number specifying how long the number itself will be (e.g., 1.25 has a size of 3, 123.456 has a size of 6). The "d" argument specifies how many digits can be after the decimal point (e.g., **d = 2**, **size = 5**... 1.23 will work, 1234.3 will also work, but 12.345 won't).  

---

### Comparison Operators for WHERE:

Before moving forward, let's list all comparison operators that can be used with the WHERE clause.  

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

---
