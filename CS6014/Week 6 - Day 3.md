## Week 6 - Day 3
### Cryptographic Hash Functions
Normal Hash Functions:

* take bytes and return an int
* Deterministic - same input should always get the same hash value
* Avoid collisions between different inputs 
* H(data) -> fixed sized int
* Typically one way - cannot convert hash int back to original data

Cryptographic Hash Functions have additional goals than normal hash functions. 
These are related to message integrity  
Don’t want to convert original message with a hash as we can’t get it out to decrypt at the end, but can be used to validate the message that was converted in a different way  
**Checksumming** - verify what you received is the right size you expected  
**Passwords** - don’t want to store passwords, but want to make sure the user knows the password. Hash the password and the input has to hash to the same value stored by the site  
**“Naming things”** - git uses to name commits  
**Referring to an object** - shorter names - useful for caching  

#### Important Properties 
Needs to be similar to a block cypher  
Quick to compute  
Deterministic  
Given a hash it should be impossible to compute the data. Should be impossible to pick a message that has a particular hash. hash(m) == h, we shouldn’t be able to know what m would be to guarantee that.  
“Forge resistant” can’t at will generate a message to get that specific hash so the checks can’t be compromised  
Small changes to the data cause massive changes to the hash. 
Very difficult to find to sets of data that generate the same hash (no collisions)

No collisions is difficult due to the **birthday paradox**  

```
~25 people to get shared birthdays out of 365 days in a year. 
N people in a room.  
2 people in a room 364/365 probability  
3 people in a room ^ * 363/365  
4 people ^ * 362/365  
```

D possible hash values from a function  
N number of hashes run  
A 50% change of collision sqrt(2d*log2). 
Encryption we expect 2^k-1 guesses to find the right key  
Hashes is 2^(hash length/2)  
128 AES key lengths are good. 256 length hash values to be good enough to match the key

#### How it works
Break data into blocks +plus any padding to offsized blocks  
Each block gets mangled - not reversible. Takes two inputs. Generates a F block that is smaller than the data block.  
Use the input of the prior F block as the second input to generate the next F  block’s mangler  
The initial input IV - initialization Vector  
Often there is a finalizer step to final process the hash that is returned 

IV is typically a mathematical random set of bits (like section of 1000-2000 of pie)  
This is a **Merkle Damgard Hash (MD)**

### MD5
MD5 was the standard for a long time, still used but not for security purposes  
Collision attacks have been feasible for a while now (2 messages with same hash)  
But also has “chosen prefix” attacks which permits document forgery - these are more scary  
Uses a set beginning part of the message, but allows additional stuff after the prefix to go through no problem.  
Basically creates a fake document that the hash still validates as accurate.  Used to spoof certificates (how people prove they are who they say they are)  
It should be avoided for anything new. 

### SHA-1 (Secure Hash Algorithm)
Was just broken recently  
Used by Git  
160 bit output  
2017 - had collisions found two pdfs that collided  
2019 - fall had a chosen prefix attack  
A MD style function  
SHA-2 (256-bit output) and SHA-3 (512-bit output) are what will be used going forward  
SHA-3 is a completely different format than SHA-1 and SHA-2 not standardized and not being pushed for  
Developers are kind of hedging bets as both are still secure to see which breaks first

### Hash Message Integrity
A -> message, hash(m) -> B  
Attacker wants to send m’  
B will compare m’ to the provided hash and know it doesn’t match  
Everyone knows the hash(m) method, attacker could send m’, hash(m’) and B wouldn’t know  

To ensure this isn’t done a secret key has to be used between A and B that only they know about 

Most common is the **Hash Message Auth Code (HMAC)**  
Hash(key + hash(key + message))  
If attacker changes message, not knowing the key they won’t know the final hash to validate  
The addition here is concatenation 

### Passwords
Dont store passwords, validate user knows the password  
Store a hashed version - cant say “this is your password” but can validate that the incoming password matches the hash  
This is why it is password resets, not “this is your password"  
Still can get compromised - with a rainbow table  
Basically guess a lot of password and hash them and compare the stored hash to the table to find the real password.   
Store a salted hash value  
When a user signs up they provide a password.  
Each user is given a random 16 byte “salt” for seasoning.  
Store the HMAC(Password, Salt)  
When user logs in, look up their salt, compute HMAC(P,S) and verify  
Usually HMAC(P,S) is run many times on itself 40k+ times

This means any attacker has to guess each password independently. Per account attack instead of per database  
We actually want this hash function to be SLOW.  
Make it take time to compute so hackers cant quickly brute force  
Most common is a bcrypt - real world usage. Always just use a secure library that recreated this instead of implementing itself

### Authentication
A wants B to verify that it is B  
Both A and B know a secret key  
A sends a random number (Ra) to B  
B sends back hash(k || Ra) and a challenge response (Rb)  
A sends hash(k || Rb)   
Both A and B now have verified each other