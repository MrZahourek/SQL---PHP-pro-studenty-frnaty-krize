
Data selection

SELECT - selects all values that are specified. Basically acting as output command. This command cant work on itself however. 

FROM - specifies the source for the data to be selected. To be more exact we have to specify the table that we will use as the source. The table has to be in the database we are currently using.

WHERE - acts as a sort of a if statement for the SELECT command, selecting only data that has some specific value. The commands and characters below are the comparison operators. 
	**the ones we are used from programming (=, <, >, <=, >=) work the same, but only the = works on words, while all of them work normally on numbers**
	**<>** - not equal (sometimes can be written as !=)
	**AND & OR** - work the same as in if conditions, AND allows multiple WHERE statements that have to be met, OR makes multiple WHERE statements that can be met
	**LIKE** - for trend search (for example if we want all the data that starts with s)
		% - represents some space before or after (%r - r has to be at the end, r% - r has to be at the start, %r% - r has to be inside the)
		 _ - single character, so its literally one missing character (\_\_ll can be wall, hall, yell, etc..  )
	**BETWEEN** - for looking into range of numbers
	**IN** - works the same as the = but for multiple values (WHERE *arg* IN *(1, 2, 3)*
	
ORDER BY - adding this command to the end of the SELECT syntax, will make the data organised. By default it will be in descending order. The values will be organised by the column we will put in as an argument, if the column is INT it will make numerical order, if the column isn't numerical the order will be alphabetical. 
	DESC - will make the order descending, is set by default
	ASC - will make the order ascending

now lets actually write some code with the new commands

lets start with selecting all data from a table called *users*
```sql
SELECT * FROM users;
```
*Note: the * will select all data*

Now lets pick specific columns, we will do that by actually providing arguments for the SELECT command (we will select columns *username, post_ammount* and *follower_count*)
```sql
SELECT username, post_ammount, follower_count FROM users;
```

Now lets make it more specific and select from these columns only one specific user called *Rene*, by using the WHERE command
```sql
SELECT username, post_ammount, follower_count FROM users WHERE username = "Rene";
```

To show next command we will need more users we will do that by simulating a user search in search bar. This means that we will assume that we typed *Ren* and order it by relevance, we will use follower count as measure of that.
```SQL
SELECT username, nickname, follower_count FROM users WHERE username = "Ren%" OR username = "ren%" ORDER BY follower_count;
```



Data creation

