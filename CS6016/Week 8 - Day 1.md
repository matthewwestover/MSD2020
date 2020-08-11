## Week 8 - Day 1
### LINQ
If Models has two classes: Players and Games

```
query =
from p in db.Players
join g in db.Games
on p.PId equals g.BlackPlayer select p;
```

This creates a C# Object: IEnumerable<Players>  
This depends on what is being selected  
LINQ starts with “from"

```
query =
from p in db.Patrons
join c in db.CheckedOut
on p.CardNum equals c.CardNum select p.Name, c.Serial;
```

This creates IEnumerable<Temp NameSerial>  
NameSerial{string Name, int Serial}.  
This is bad form. 

```
query =
from p in db.Patrons
join c in db.CheckedOut
on p.CardNum equals c.CardNum
select
new {name = p.Name, serial = c.Serial}; // this is an anonymous inner class
```

This allows usage on the query object

```foreach(var x in query) WriteLine(x.name);```

### JSON

```
var query = from p in db.Patrons select p;

Json(query.ToArray());

[
    {“Name”: “Joe”, “CardNum”: 1}, 
    {“Name”: “Ann”, “CardNum”: 2}, ...
]
```

### Quiz
Write LINQ to return JSON array containing: {Title: “...”, Serial: …}

```
var query = 
From t in db.Titles
Join i in db.Inventory
On t.ISBN equals i.ISBN
Select new {title = t.Title, serial = i.serial};
Return json(query.ToArray());
```

### JSON Nested Data

```
[
{Title: “Harry Potter”, Serial: 1001}, 
{Title: “Harry Potter”, Serial: 1002}, 
{Title: “Hyperion”, Serial: 1008}
]
```

However we might want this differently formatted. 

```
[
{Title: “Harry Potter”, Serials: [1001, 1002]}, // Single Title Line with a nested array of Serial numbers
{Title: “Hyperion”, Serials: [1008]}
]
```

In order to do this, we have to use a nested select statement

```
var query =
from t in db.Titles select new
{
    Title = t.Title, 
    Serials = 
    from i in db.Inventory // Second Select Statement
    where i.Isbn == t.Isbn 
    select i.Serial
};
```

### Navigation

```
from t in Titles
    select new{
        title = t.Title,
        serials = from i in t.Inventory 
        select new {i.Serial}
    };

Returns {string, IEnumerable<uint>}
```

This returns the same information as the nested Query  
t.Inventory results in a join at some point, just abstractly hidden by LINQ and C#  
Some avoid due to the hidden abstraction. While shorter it isn’t as explicitly clear in cost

Titles with no Serials

```
from t in Titles where t.Inventory.Any() // Only returns rows where there is data in this column
  select new{
    title = t.Title,
    serials = from i in t.Inventory 
    select new {i.Serial}
};
```

### Insert

```
Patrons p = new Patrons(); 
p.Name = “Erin”; 
p.CardNum = 5;
db.Patrons.Add(p); 
db.SaveChanges();
```

### Remove

```
Patrons p = new Patrons(); 
p.Name = “Erin”; 
p.CardNum = 5;
db.Patrons.Remove(p); 
db.SaveChanges();
```

### Update Rows

```
var query =
from p in db.Patrons 
where p.CardNum == 1 
select p;
query.ToArray()[0].Name = "Joe”; 
db.SaveChanges(); 
```

This one can have issues if it doesn’t return a proper json object.  
Better:

```
var query =
from p in db.Patrons 
where p.CardNum == 1 
select p;

Patrons x = query.SingleOrDefault(); 

if(x != null)
x.Name = “Joe”; 

db.SaveChanges();
```

### Users and Passwords
In Theory it is easy:

```
if(!Users.Contains(name))
    // unknown user
if(Passwords[name] != pwd) 
    // invalid password
```

Store them in a UserInfo database of UserNames and Passwords

```
SELECT Username, Password from UserInfo WHERE Username=... and Password=…

if(result.Count() != 1) {invalid…} // Don’t log them in    
```

Storing Passwords is NOT SECURE AND BAD - Injection Attacks, Physical Breaches, Social Engineering  
Users can’t directly access tables - in theory not in practice.  
Server has database password – controls access to tables  
Unfortunately tables get stolen  
Most tables have harmless information, But passwords are very dangerous  
This happens ALL THE TIME

Safest option: don’t store passwords **anywhere**  
Nobody knows your password – **not even the server**  

Creating an Account - pass password into a scrambler  
“Password” -> Scrambler = “db7fa8c0”  
“db7fa8c0” is what is stored in the database

Scrambler must be consistent - Same input always gets the same output  
If database is stolen, attacker only gets garbage  
It cannot be unscrambled - cannot be reversible 

Scrambler function must:  
1. Produce same result given same input i.e. a hash function  
2. Be un-reversable  
3. Be unlikely to produce collisions  

This is a called a cryptographic hash function

Example: Set every other bit to 0  
Password: 158 (10011101)  
Coverts to: 136 (10001000)  
This is consistent, 158 always converts to 136  
It isn’t reversible as you can’t tell if a bit was originally a 1 or a 0  
It can be brute forced however.  
Also 221 ((11011101) also converts to 136, this will let you in.  

Finite number of things to check, higher likelyhood of collisions. 

A good cryptographic hash function:  
1. Must be very difficult to find any string that produces known hash  
2. Collisions must be very unlikely – even for similar passwords  
Collision is still possible but highly unlikely. Impractical to brute force yourself in.  
SHA1, SHA256, SHA512, MD5, etc.  
MD5 has a lot of vulnerabilities recently discovered. 

In C#

```
using System.Security.Cryptography; 
new SHA1().ComputeHash(pwd);
```

Store the hash in the database

From SQL

```
INSERT INTO ... SHA1(pwd)
```

To combat this issue. Add **SALT**  
salt = random_string(); → “a748cf73a”  
add salt to password: “a748cf73abadpassword”  
hash(salt + password); → b34df90c  
Since salt is random the rainbow table becomes useless  

Issue arises with how can you generate the same random salt when a user wants to login. They need the same salt  
Table has to save the salt string as well.  
User + Salt + Hash(salt+password)

For each stolen salt, combine with common passwords = Much bigger rainbow table  
This is hard, but still possible to hack through with brute force  
Best way to “hurt” a brute force algorithm is to slow it down

Server uses a computationally-intensive hash function  
Should take ~1s to compute. 
Fast enough to not bother users logging in  
Slow enough to make brute force rainbow table comparisons take months/years

```
hashed = SHA1(salt + password);
for(i = 0; i < 1000; i++) hashed = SHA1(hashed);
```

Chaining hash functions can actually make them less secure.  
Intuitively, input to first hash function can be arbitrarily long...  
But inputs to subsequent hash functions are typically fixed length!  
Space of possibilities is much smaller. Could collide!

To handle this use **PEPPER**  
Similar to salt, added to the original password before hashing  
Pepper is stored in a separate DB

Salt: must be unique per user  
Pepper: single pepper per system is fine  
Point is it’s secret, whereas salt is stored in user table  
Do not reuse it for a different system!

Key Derivation Function  
Generates salt, uses a pepper and hashes them: KDF  
A good KDF protects bad users from themselves.  
Some KDFs also intentionally use a ton of memory  
Make brute force infeasible on a GPU

Biggest vulnerability is a bad password – devs try to protect you from yourself

If your hash does get stolen, it may take months for an attacker to do anything useful with it  
Change your password once in a while!

Storing Passwords. 
Let .NET do it for you  
Configure MVC project with “identities”  
Create separate database to store user credentials