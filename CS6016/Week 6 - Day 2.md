## Week 6 - Day 2
### SQL in C#
\#1 rule of optimizing is don’t  
Don’t sacrifice a good design for unneeded performance  
Should uID be int or char(8)? 123456 or “u0123456”  

If uID is int  
Good: Create new user with single insert (auto_increment), int makes a better primary key  
Bad: Strip off ‘u’ from all URLs, Add ‘u’ to values returned to web server, Add leading zeroes to low uIDs, 25→“u0000025”

If uID is char(8)  
Good: No conversion from URL, No conversion for returned value  
Bad: Creating new user is something like this: 

```
SELECT uID FROM Users
ORDER BY uID DESC LIMIT 1;
result = strip off ’u’; result = result.ToInt();
result++;
result = ‘u’ + result; INSERT INTO Users ...
```

For a web server, individual client performance is not a big factor  
For uploading / refactoring an entire database, it is a big concern

Procedural SQL Commands  
Consider the following C# code:

```
string filter = Console.ReadLine();
cmd.CommandText = “select ... where Thing = ‘” + filter + “’”;
```

Typing 'hi':  

```
select ... where Thing = ‘hi’

select ... where Thing = ‘’;hi’ // this makes hi “garbage” and is code injection. Hi could be valid code to execute
select ... where Thing = ‘’;delete from Table1;’

```

Injection Attacks  
When a query/code is procedurally generated, user can “inject” malicious commands: SQL injection, Javascript injection, HTML injection  
Even buffer overflow attacks could be considered a form of injection, Inject raw instructions in overflowed input

Option 1 – sanitize input  
If input = string with “quotes” input = sanitize(input)  
input = string with \”quotes\”  

```
string sanitize(string input) {
    string sanitized = “”; 
    foreach char c in input{
        if c is “ or \ or % or ... sanitized += “\”;
        sanitized += c; 
    }
  return sanitized;
}
```

Put actual backslash characters into string, SQL will treat the subsequent characters literally

Better Option Option 2 – prepared statements + SQL parameters  
SQL 4.1 and later  
Tell server exactly which parts of input are dynamic 

```
SELECT funds FROM Accounts
WHERE uName = and password = ;

CommandText = “SELECT ...
WHERE Col1 = @val1
AND Col2 = @val2”;

command.Parameters.AddWithValue(“@val1”, myVariable);
command.Parameters.AddWithValue( “@val2”, x);
```

#### SQL Prepared Statements
Often a similar statement is executed many times.  
Only differences are the values of the placeholders  

```
command.Execute… 
command.Parameters[“@val1”].Value = 1; 
command.Execute… 
command.Parameters[“@val1”].Value = 2; 
command.Execute...
```

After the command has been created with initial params  
`command.Parameters.AddWithValue( “@val1”, placeholder);`

Change them whenever you want, e.g. in a loop:

```
for(i = 0; i < N; i++){ 
    command.Parameters[“@val1”].Value = I; 
    command.Execute…
}
```

DBMS has to “compile” every query, i.e., plan generator  
Prepared statements are compiled once then cached on the server  
Executing the same prepared statement repeatedly has much lower overhead