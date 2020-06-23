## Week 1 - Day 1
### Data
Data lives on disk, moves to DRAM, the Cache, and Registers as needed for programs.  
Often data is too big to store locally  
Data also needs to be permanent

“Big Data”  
Vast amounts of data  
Fast access/combination/filtering. 
Must be available:  

* Online
* Securely
* To simultaneous users

What is data? Suppose you want to save a bunch of Students to a file

#### Option 1:
“Jane Doe is a Film major with a GPA of 3.7, and is enrolled in CS2420, and her ID is 12345"  
"John Smith is …”  
Searching for a student in this format is a PITA. Split each sentence and scan the entire database to find a name  
Also have to know the data’s format to get the specific location of each part of data we need  

#### Option 2 (JSON-like):
```
Major: Film 
Class: CS2420 
Name: Jane Doe 
GPA: 3.7
ID: 12345
```
Still a linear scan, but data is more compact to read.

#### XML - much like JSON:

```
<Course> 
    <Name>CS2420</Name> 
    <Students>
        <Student>
          <Name>Jane Doe</Name>
          <Major>Film</Major>
        </Student>
        <Student>
          <Name>John Smith</Name>
          <Major>CS</Major>
        </Student>
    </Students> 
</Course>
```

Store a bunch of student records by name, and quickly:

* Add
* Remove
* Search
* Enumerate

Binary search tree can do all of these quickly

Imagine Storing Students, Courses, Professors
Students: 

```
Classes: CS5530, Phys2010 
Name: Jane Doe
GPA: 3.7
ID: 12345

Classes: CS3500, FILM1010 
Name: Jon Smith
GPA: 3.4
ID: 12421
```
Professors:

``` 
Teaching: CS5530, CS4400 
Name: Daniel Kopta
ID: 55555

Teaching: CS3500, CS4150 
Name: Joe Zachary
ID: 44444
```

Classes:

```
Name: Database Systems 
Num: 5530
Dept. CS

Name: Software Practice 
Num: 3500
Dept. CS
```

To answer:  

* All courses student Y is enrolled in?  
* All teachers of student Z?  
* Order courses by enrollment number?  

You have to look at different trees as they relate to each other
Data is now structured

### Structured data
Records can not have arbitrary/unpredictable fields/values  
e.g. courses have: dept, num, and name (string) (int) (string)

**Unstructured**  
Jane Doe is a Film major with a GPA of 3.7, and her ID is 12345

**Structured**

```
Name (string): “Jane Doe” 
Major (string): “Film” 
GPA (float): 3.7
ID (uint): 12345
```

But this means you now have to create binary search trees for each possible query type. A by_name tree, a by_gpa tree, etc. This also means having to handle duplicates for name/gpa/etc in the tree

Once a tree is created you then have to create a function in the language you are using to search the tree

```
getStudentsByGPA(3.5)
getStudentsByName(“Jane Doe”)
```

The best solution is to have a interpretive language that can handle this
`SELECT Name FROM Students WHERE GPA > 3.5;`

Databases store the data and manage the queries needed. 

Why use a Database? 

* take advantage of decades of research
* Availability
* Reliability
* Performance
* Concurrency
* Interface
* Don’t reinvent the wheel

### Databases
Two major components:  
Database Management System (DBMS) - Underlying machinery  
Query Language - A Common interface  

Relational Database  
Structured data storage.  
Related data are stored “next to” each other - e.g. in a table  
Non-relational databases are a thing too  
Database comprised of one or more tables (schema)  
One table represents pieces of directly-related data  
Non directly-related data are separated  
Each table is a “relation”  

Each row is a tuple – a set of data units  
Every cell does NOT need to be unique  
Ever tuple/row DOES need to be unique  
New rows can be added with same data if there single cells that differ  
We can have multiple duplicates for data this way.  
It is better to pull apart the data into separate data.  
Then create a in-between table that links the two separate sets of data. 
