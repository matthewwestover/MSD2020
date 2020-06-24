## Week 6 - Day 1
### SQL in C#
Using C# because its common, powerful, and something new from C++ and Java  
Don’t become a \<insert language here> developer  
It does include 

* Language integrated queries (LINQ)
* JSON
* Web servers
* Javascript

#### Namespace
Sets apart type names

```
namespace Math
{
public class Vector{…} 
}
namespace Collections
{
public class Vector{…} 
}
```

Like a module that covers multiple files to globally declare  
Was introduced to indicate class types. Above, different Vectors are defined

```
Math.Vector mathVec = … 
Collections.Vector collectionVec = ...
```

At top of .cs file  
`using Math;`
Automatically searches Math namespace for types  
`Vector mathVec = ...`

#### IDisposable
Interface with a single method: Dispose()  
Objects that need to be cleaned up, restored, unwound, ...  

```
IDisposable o = ...
try {
  code that might fail
} finally{ //always executed
o.Dispose(); }
```

This is for garbage collection. Like using free, delete, etc.

This is so common there is a shortcut for skipping finally and .dispose()

```
using(IDisposable obj = new ...) {
code }
```

Example - This automatically cleans up the connection 

```
using (MySqlConnection conn = ...) {
...
}
```

#### IEnumerable
Guarantees that something is iterable  
Allows use of foreach loop.  
Heavily tied to database queries in C#  
e.g., foreach row returned by a query  

#### XML Documentation
Type /// above a C# function  
Makes your functions “Intellisensable”  
XML comments are required in this class

#### Visual Studio Organization
Solution - Collection of Projects  
Project - Collection of source files

#### Project References
Link pieces of a program together  
e.g. link a MySql library to your program  
MySql library is a “dynamic link library” (DLL)  
Create references through Visual Studio - Sort of like a Makefile

### MySQL in C#
First step: we need info about the DB server  
`connectionString = "server=atr.eng.utah.edu; database=SomeDatabase; uid=joesmith; password=hello";`

This is horrible design. Anyone could reverse this and bork things up.  
Don’t store the password anywhere

```
String pwd = Console.ReadLine();
connectionString += “password=“ + pwd; // Still hardcoded
```

Better

```
while (true) {
ConsoleKeyInfo key = Console.ReadKey(true);
if (key.Key == ConsoleKey.Enter) break;
pwd += key.KeyChar; }
```

Second step: Create MySqlConnection

```
using (
MySqlConnection conn =
new MySqlConnection( connectString ) )
{ conn.Open();
...
}

MySqlCommand command = conn.CreateCommand();
command.CommandText =
“select Author from Titles join ...”
```

Then we need to use using again

```
using (MySqlDataReader reader =
command.ExecuteReader())
{
...
}
```

Data is returned in the reader object as a list of rows  
Each row is like a dictionary \<columnName, value>

```
// one row at a time while (reader.Read()) {
... reader[“column”] 
// OR
... reader[1]
}
```

#### Insert
Insert commands are a “non-query”  
Create command just as before:  

```
command.CommandText =
“insert into ...” command.ExecuteNonQuery();
```

That’s all... no result to read - Returns number of rows affected

#### Ponder
Read a text file containing many chess games, insert them into the DB  
Events & Players may already be in the database

You may be tempted to do your logic in C#

1. Run SELECT to check if Event already exists
2. if(!reader.HasRows) Run INSERT command
This goes from laptop to server to laptop to server just to insert. This creates a spin lock while waiting

#### Exceptions
```
try{
  INSERT that might fail
}
catch(Exception e){
  // ignore it or print something
}
```

This might not be the best. A failed INSERT doesn’t mean it broke.  
Exceptions are also SLOW

Let MySQL Do the work for you -Move as much logic as possible into SQL  
Run INSERT IGNORE command  
Reduces the number of queries - Every query is a high-latency round-trip to the server  
But aren’t we making the queries more complex/expensive? Yes, but MySQL is good at that