## Week 6 - Day 1
### Symmetric Block Cyphers 
Encryption and Decryption use the same key  
Shared secret key  
Both sides must know this key, no one else should  
This is in fixed sized chunks of data - Historically 64 bits, more commonly 128 bits now  
The longer the key the more “secure” the cypher - 256 bits is suggested these days  
Have to encrypt not used sections of the bit block if it isn’t full  
Fixed size makes these really fast  
Two main parameters to describe a block cypher is key size and block size. 

Popular Block Chains:
**DES** - Data Encryption Standard - from 1977 (don’t use this)  
**3DES** - run DES three times - from the 1990s  
**AES** - Advanced Encryption Standard - standardized in 2001 - developed from a scientific contest

**Ideal**  
If the cypher is perfect, the only way to crack it is brute force.  
If the key is k size, there are 2^k possible keys to try  
On average you half to guess half of the keys before cracking in brute force  
You expect 2^(k-1) work required to decode without knowing the key  
Anything less weakens the cypher

### DES
Key Size: 56 bits - too small (super computer cracked this in the 90s now pretty easy to have power to do it)   
Block Size: 64 bits  
A "Feistel Network”  

* Plaintext is split into left and right halves
* Broken up into several “rounds” (basically shuffles right to left and vices-versa) 
* Each round uses a different round key - given by key schedule which is seeded with the initial key value
* F is a mangler function that isn’t necessarily reversible 
    * Two inputs -> one output (half of the entire block + a key)
* Uses XOR to mix block and permute the bits from the side being mangled
* F mangler does substitution to spread each bit out

DES repeats this this multiple times 

```
Message abcd, key mno
Ab cd -> cd ab(xor)xy; 
-> ab(xor)xy cd(xor)st;
-> repeat a bunch to make mixed enough
```

3DES is the same thing but three times. Three different keys - 168 bits of key  
```Encrypt(Decrypt(Encrypt(message, key1), key2), key3)```

Design is okay but not great.  
Inefficient in software. Bit swapping is annoying computers like working on byte level instead  
56-bit keys are too small  
64 bit block is also pretty small  
It was developed secretly - raises questions about how trusted it can be

### AES
Block Cypher of choice - in use all the time  
Developed in a public contest - it was vetted and tested openly
128 bit blocks  
128, 192, 256 bit key lengths  
The number of rounds is determined by key size  
It is software efficient, cpu has specialized AES instructions. 

128 block is seen as a 4x4 grid (octets) (128 bits is 16 bytes)

|||||
|:--:|:--:|:--:|:--:|
|  1 |  2 |  3 |  4 |
|  5 |  6 |  7 |  8 |
|  9 | 10 | 11 | 12 |
| 13 | 14 | 15 | 16 |

This grid is the “state” and is modified until it becomes the cyphertext  
Some operations act just on individual bits  
Some operations act on an individual row  
Some operations act on an individual column  

Four steps in a round
Substitute Bytes - subbytes

* There is a 256 byte table (defined by standard) 
* Look up each byte in the table and substitute
* To decrypt just substitute back

Shift Rows 

* For each row, shift row# columns left
* Row 0 doesn’t move
* Row 1 moves 1 left
* Row 2 moves 2 left

Mix Columns

* Shuffle up columns

Add Round Key

* XOR the state with current round key

Still has tricky issues

* Not trying to send just 128 bits. Often more data than 128 bits, last set of data might not reach 128 bits total
* Both sides need to know about the key

