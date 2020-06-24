## Week 7 - Day 1
### MVC Web Servers and Scaffolding
Separation of Concerns - Most important SE principle  
Complex software would not be possible without decomposition

#### Model View Controller
MVC -Formalization of separate concerns for graphical programs. Especially the web

**Model** – where the data comes from  
**View** – either the GUI or command line to interact with the data (or filtered data)  
**Controller** – moves stuff around between Model and View.

Expose two different views to same model. Separation of concerns in unit testing. Divide programmer workload

As a webserver  
Model: the database and C# objects representing it  
View: html, css, javascript  
Controller: database, queries, logic. 

In the true spirit of separate concerns  
View (V) should not need to know the implementation of M or C  
e.g. C# field names, method names

#### Structured Data
Unstructured: Jane Doe is a Film major with a GPA of 3.7, and her ID is 12345  
Structured:

* Name (string): “Jane Doe”
* Major (string):  “Film”
* GPA (float): 3.7
* ID (uint):12345

#### JSON
JavaScript Object Notation  
Generic syntax for structured data  
Names and values only – no types. 
Plain text

```
{“Name”:“Carlsen”,”ELO”:2843, ”Status”:”World Champion”}
```

JSON can be nested  
Pass plain text (JSON) instead of an actual object  
Engineer designs an “API” specifying names and structure of fields in JSON data  
Controller – code that converts actual model into agreed JSON form  
View – code that renders the agreed JSON fields  

Every modern language has built-in or library support for JSON  
Automatically convert to/from JSON/Object

#### Rename Objects Automatically
```
Class ChessGame {
[JsonProperty(PropertyName = “wElo")] private int whiteElo;
...
}
```

#### Model
How will you prevent two classes from overlapping in location and time?  
How will you convert your DB query to JSON?  
Useful to have C# representation (mirror) of database  
Overlap logic is trivial in C#

```
void AddClass(loc, time, ...){
    LMSClass c = new LMSClass(loc, time, ...); 
    foreach(LMSClass t in Classes){
        if(c.Loc == t.Loc && ...)
            reject 
}
```

JSON conversion is trivial with C# Model

```
GetClass(...){ 
LMSClass c = …; 
return Json(c);
}
```
Couldn’t we just write helpers to convert to/from MySqlDataReader?  
GetStudentsJSON, GetCoursesJSON, GetAssignmentsJSON, etc.  
Yes, but it gets ugly/messy  
This all assumes that our C# classes are mirrors of DB tables  

Design Database or Code first?  
DB-First: Design using relational model, Convert from tables to class  
If your main concern is the data and its relations  

Code-First: Design algorithms + class first, Convert from class to tables
If your main concern is software performance

Good news: it’s easy to convert both ways. 
There are tools to automatically generate C# code that mirrors a DB
This is called scaffolding