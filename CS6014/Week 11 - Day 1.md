## Week 11 - Day 1
### SQL Injection
Common instance of malicious users tricking a server to run unauthorized code  
Any time user input is (even partially) interpreted as code, we’re entering very dangerous territory! We’ll look at a couple other examples  
Nothing particularly special about SQL aside from its ubiquity, and that it’s often used in a way that makes it unsafe without care  
SQL - **Structured Query Language ** 
SQL - A way to communicate with a relational database  
Like a series of spreadsheets, with columns and rows  
SQL is how you can search/sort/input/retrieve data  
SQL does CRUD actions for databases  

A relational database is a structured way of storing data  
A database consists of many tables  
A table stores a list of records which contain fields  
You can think of a table like a spreadsheet. The table is described by the “header row” across the top which lists which columns are present  
Each record in the table is like a row of a spreadsheet

**SELECT**: lets us search/retrieve records from a table. Usually called a query   
**INSERT**: sticks new stuff in the database  
**UPDATE**: updates existing records  
**DELETE**: deletes the record

Can also create a new table, drop a table (delete), alter table (new columns types etc)

```
CREATE TABLE products(id int, price float, description text); 
SELECT * FROM products WHERE price < 20;
INSERT INTO products VALUES(20, 52.50, “hello kitty alarm clock”);
```

Basic structure is ```<operation> <table> <“where clause”>;```

### Injection Attacks
Example in real life - run a web store and a user wants to find hats

```
results = sqlQuery( “SELECT * FROM products WHERE name CONTAINS ‘” + userInput + “‘;”);
SELECT * FROM products WHERE name CONTAINS ‘hats’;
```

This is an issue as people can “inject” other sql code into the search.
Instead of hats if they search for 

```
whatever‘; DROP TABLE products; -- //double dash is a comment in SQL
```

This produces 

```
SELECT * FROM products WHERE name CONTAINS ‘whatever’; 
DROP TABLE PRODUCTS; 
- -‘
```

And the entire database is deleted

Another example storing username and passwords

```
loggedIn = sqlQUERY( “SELECT * FROM users WHERE name = ‘“ + username + “‘ AND password = ‘“ + providedPassword + ‘;”);
```

Attacker inputs ```username = administrator, password = ‘ OR 1 == 1 ; --```

This produces ```SELECT * FROM users WHERE name = administrator and password = ‘’ OR 1==1; --';```

Password doesn’t match, but 1==1 is true which gets past the password check  
The issue is we are treating the input as code. SQL requires user input and developers are lazy in scrubbing data

### Partial Solution
Don’t let the sqlQUERY to run multiple commands. This prevents the DROP TABLE command above.  
Basically once we hit a semicolon nothing else can happen after that

### Medium Solution
Sanitize all user input  
This uses string operations to change the string before running the QUERY  
‘ Becomes \’ a literal quote prevents code input  
Setting this up correctly is VERY hard. Don’t use your own.  
This use to be the major method for preventing but just has too many ways to get around 

### Modern Best Practice
Use a “prepared statement”  
You provide the structure of your query to the database and leave placeholders for user input values.  
The DB compiles this ahead of time, so the structure is fixed.  
You can safely insert user input into the query later and it CANNOT change the structure of the query

```
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM products WHERE name = ?”);
stmt.setString(1, userInput);
ResultSet rs = stmt.executeQuery();
```

No matter what the userInput, the database will just search for that name in the database  
The ? is a placeholder  
Using a prepared statement means that an attacker cannot modify the structure of your query  
Worst case now is user provides bad data

### Beyond Just SQL
HTML (what happens if a user gives us input containing \<tags>? \<form> is especially nasty! )  
JS, and other languages provide an exec() function: ```exec(“console.log(‘hello world’);”)``` would run the string as code. 
JSON parsing used to be done with result = exec( jsonString); This was super dangerous! Some JSON messages from google start with while(true){} to thwart this, even though modern JSON.parse() is not vulnerable.

