## Week 6 - Day 1
### Room - Local Databasing
Why use a local database?  
Your app may need to fetch data regularly from the internet. Imagine if internet access breaks down.  
The app should still be responsive, and give the user some kind of access to the data.  
A local database helps you cache downloaded data.  
Workflow could be:  
Get data from internet.   
Cache in database.  
Show in app. 

### SQLite
Writing SQLite code in Android was a pain. You’d use the android.database.sqlite package.  
This would expose a low-level API that lets you read/write to SQLite databases.  
Drawbacks:  
No compile-time verification of SQL queries.  
Must update SQL queries manually if database graph changes.  
Must write lots of code to convert between SQL queries and data objects.  

Room was introduced to fix this.  
Provides an abstraction layer over SQLite.  
This layer allows easy and automagic conversion of SQLite queries to Java objects.  
Key idea: Room allows you to annotate Java objects as SQLite entities.

### Room Components
To get Room into your app, you need 3 components:

* Database: a class that is the access point for the app’s relational DB. Must be annotated with @Database.
* Entity: a table within the Database.
* DAO: data access objects. These let you annotate Java methods with SQLite queries, or let you annotate methods as insert/delete methods.

Note that the queries in the DAO are verified at compile time.

### Room Database
You annotate a class with @Database.

* This class must be an abstract class that extends RoomDatabase.
* Must include all the entities associated with the database within the annotation.
* Must contain an abstract method with no arguments and returns a class annotated with @Dao.

It’s common to have only one database corresponding to a single type of object.  
You enforce this using the Singleton pattern.

### Room Entity
A class containing data.  
This is the data that gets stored in your database.  
You have to denote one of the variables as a primary key.  
You can annotate with your own column name, or use the variable name itself as a column name.

### Room DAO
You give function signatures for executing SQL queries.  
You annotate each function with the appropriate SQL statement.  
Typical signatures are insert, delete, get all data, etc.  
The interface is implemented by Room! We don’t need to define the functions inside the DAO.