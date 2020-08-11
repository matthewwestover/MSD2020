## Week 8 - Day 2
### Security Misc.
Cost dominated by making the rainbow table  
Without salt: (cost of hash * #common passwords)  
With salt: (cost of hash * #common passwords * #stolen salts)  
With pepper: can’t do anything unless pepper stolen separately  
The KDF handles all of this

The KDF is run on the server. Password not secure until hashed  
Password can be intercepted before reaching the server

Encrypt the password before sending:  
HTTPS(ecure)  
Essentially: HTTP, but use an encrypted socket  
Encrypt before sending and Decrypt after receiving  
Completely separate from KDF (hashing passwords)  
TLS - Successor to Secure Sockets Layer (SSL)  
Multi-step handshake to exchange keys 

Other forms of attacks don’t require stolen data e.g. brute force login attempts  
Lockout after X attempts.  
e.g. 5 attempts, then locked out for 1 hour (or indefinitely)  
Makes brute force login impossible

Two-factor authentication  
Even if pwd stolen, need second device  
Similar to pepper (need to steal 2 things instead of 1)

Changing password requires email  
Without access to your email, nobody can change your pwd

### Testing
Testing software that accesses a database has its own interesting challenges  
Unit Testing - One of the benefits of MVC: logic code independent of UI code i.e. controllers. 
Treat each controller as a unit – generate fake input

```
[Test] 
TestCheckOutBook(...) {
HomeController.CheckOutBook(1005);
Assert... }
```

Problems: 

1. Tests must have known outcome – database contents always changing
2. Tests should not affect live database – some controllers modify it

Solution? Make a test MySQL database with same schemas, Reset to known state before running test suite  
Downsides: Unit tests should be fast, but db access is slow, Test inputs not visible in test code

Solution? Separate logic from db access – don’t test db access

```
Controller(){
GetData(); // Don’t test 
DoLogic(); // Test with fake data
}
```

Downsides: db queries can be buggy too (LINQ/SQL)

Solution? In-memory “mock” database. Test only with LINQ – not connected to real SQL  
Downsides: Places a lot of trust on LINQ → SQL translation. 
LINQ is a mature system already tested. Not our job to test it.

### Mock Database
A “mock” database:

* Is not actually running on the MySQL server
* Has the same schemas as the real db
* Works with the same source code (e.g. LINQ queries)
* Is usually small
* Can reside in-memory (very fast)
* May not be relational (referential integrity not enforced)

Generally prefer:  
Mock database for logic testing  
Actual test database for SQL testing  
These tests only need to run when schema changes or query changes

Another issue: most controllers return anonymous JSON  
Thus, unit tests have no type safety  
The most practical approach here is to use knowledge of the return JSON type to write unit tests.  
Check if the returned JSON is an array. Check if it has the correct fields.

Normally we do this:

```
using (db = new LibraryContext())
```

Connection string is hard-coded to live database

We need something like this:

```
using (db = new LibraryContext(mockDB))
```

Use mock “connection” instead

Controller should not know whether it’s being tested or not  
Don’t create the db context for every method  
Save a persistent context in the class 

```
class HomeController{
LibraryContext db = new LibraryContext();
}

AllTitles(){ 
from x in db… // member of the class, no using
}
```

If we need to change the db (for testing) 

```
class HomeController{
    LibraryContext db = new LibraryContext();
    UseContext(LibraryContext newdb){ 
        db = newdb;
    } 
}

[Test]
void TestAllTitles() {
    mock_db = mock library context
    controller = new HomeController();
    controller.UseLibraryContext(mock_db);
    Assert(controller.AllTitles(), ...)
}
```

Making a mock database:

```
var optionsBuilder =
new DbContextOptionsBuilder<LibraryContext>();
optionsBuilder.UseInMemoryDatabase();
LibraryContext testDB = new LibraryContext(optionsBuilder.Options);

//Put in some test data 
//Assert some stuff
```