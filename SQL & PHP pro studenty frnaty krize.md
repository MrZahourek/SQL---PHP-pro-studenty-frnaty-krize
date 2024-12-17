
First lest list out few commands that we will need for start
- CREATE - creates a new database or a table
- DROP - deletes databases or tables
- USE  -  switches the "active repository" in the work environment
- ALTER - alters the properties of tables and databases, also used to alter data
	-  READ ONLY - Boolean property of a database that if equal to 1 will prevent us from altering it
- SELECT - selects specified data from a table
- FROM - specifier for other commands allowing us to address a specific table or database
- WHERE - specifier for selecting data, acting similar to an if statement
- RENAME *keyword* TO - renames tables, databases and their columns, used together with the ALTER command
- MODIFY - used to change the datatype of a column and other properties of the columns, used together with the ALTER command
- ADD - used together with ALTER to add new column into a table
- DATABASE - keyword specifying the addressed item is a database
- TABLE - keyword specifying the addressed item is a table
- COLUMN - keyword specifying the addressed item is a column in a table 

We will also list out basic and most common variable types
- DECIMAL(size, d) - used to create a decimal number. The size is a number specifying how long will the number itself will be (so 1.25 has size of 3, 123.456 has size of 6). D argument is a number that will say how many digits can be after the decimal point (d = 2, size = 5 ... 1.23 will work, 1234.3 will also work, but 12.345 won't).


Last before we will truly start we will also list out all comparison statements for WHERE

Now we can start by creating a database and making table inside

SQL is written differently than languages we are used to so far, as we are only writing it as commands in a command line. Similar to BASH scripting we did on our OS classes. 

To create a database we will use the CREATE command a DATABASE keyword to specify that we are creating a database. After that we will make it our working "repository" by using the USE command.
```sql
CREATE DATABASE DB_name;
USE DB_name;
```

*Note: You can see that every line ends with semi-colon, similar to JS or PHP this indicates end of the line. You can also see that I'm using uppercase for commands, but that's just stylistic choice and SQL isn't case sensitive in its commands. so this means that this code is also working and valid.*
```sql
create database dbName;
Create Database DBname;
```

If we wish to delete a database or a table in it we will use the DROP command.
```sql
DROP TABLE table_name;
DROP DATABASE db_name;
```
Just to specify, yes you do delete the tables inside when you delete a database that contained them.

To rename a table or a database we will use the RENAME TO command, but don't forget **this method wont work for columns** , we will look into that a bit later.
```sql
RENAME TABLE students TO children_who_study;
RENAME DATABASE myDB TO DB;
```

After creating a database we will put table inside. If the command wont work try running the USE command again and make sure that the name of the database is correct.
To make a table we will do very similar command to the one we did to create new database. But after that we will specify the columns and their datatypes. In this example I will make a table belonging to some school database.
```sql
CREATE TABLE students (
	studnetID INT,
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	mark_average DECIMAL(3, 2),
	parent_phone INT,
	email VARCHAR(45)
);
```

To look at the columns we created we will use this command:

```sql
SELECT * FROM students;
```

Later when we will actually insert data inside we will use a bit different syntax but for now remember that the * addresses everything in the specified database from the FROM command.

Now lets look at few usages of the ALTER command:

To add more columns we will use the ALTER command together with the ADD command. We have to specify the altered item by the keyword and its name. Then we will use the ADD command follow it by the name of new column and that follow by the column datatype.

```sql
ALTER TABLE students
ADD class VARCHAR(5);
```
*Note: Here you can see I didn't write the code on one line but on the first line there is no semicolon. The code will work like this and I'm doing it just to make it more pretty and readable. If you don't want to do that you also can write*
```sql
ALTER TABLE students ADD class VARCHAR(5);
```
*we did the same thing before with the CREATE TABLE but that one can be written on one line too.*

To rename a column, but **not a database or a table** we will write similar syntax to renaming a database or a table but with a bit of change. Don't forget to add the COLUMN keyword since without it the code wont work. 

```sql
ALTER TABLE students
RENAME COLUMN email TO studnet_email;
```

If we need to change the datatype or just something in the brackets of the datatype we will use the MODIFY command together with the ALTER command. 
```sql
ALTER TABLE students
MODIFY email VARCHAR(100);
```

To move the position of a column we will have to use MODIFY command and AFTER keyword. After we specify the edited table we will write the column with its **exact** datatype (even the brackets count and can lead to an error), after all this we will specify the column after this will be. 
```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
AFTER last_name;
```
To put a column at the fist place write
```sql
ALTER TABLE students
MODIFY email VARCHAR(100)
FIRST;
```

To delete a column we will do again similar thing to deleting a table or database.
```sql
ALTER TABLE students
DROP COLUMN parent_phone;
```

Time to add data into our table.

For that we will use these commands
- INSERT INTO (*columns*) - allows us to insert data into our tables. From now on we will call it creating rows. The argument in the brackets isn't 
- VALUES (*values*) 

==**DISABLE THIS FUCKER**==

![Pasted image 20241217114336](https://github.com/user-attachments/assets/b69197d2-c696-4df9-ac5a-34e5fd534c8a)
