## Week 6 - Day 2
### Stream Cypher
We almost never want to send a single block of data exactly. Data is in different sizes.  
Sometimes we send longer data (like TCP transfer) or send smaller than a block of data

To generate the stream take your bits of the plain text  
XOR with a pseudorandom bits of data (key stream)  
The quality of our cypher depends on the quality of the keystream  
If we xor two streams with the same key stream there are issues.  
Cypher 1 XOR K and Cypher 2 XOR K = Cypher 1 XOR Cypher 2   
This completely removes the keystream - encryption is then compromised  
If attackers detect the same key being used this can be figured out  
Attackers can generate the table to start to guess combinations to figure out what 1 or 2 are. Its not immediate to do, but it is much easier than just cracking with a single key  
Keystreams cannot be duplicated for this reason  
Keystreams are cryptographically secure pseudo random number generator (**CSPRNG**) 

Both ends need to generate the same keystream if one end wants to understand the other  
No one else should be able to generate the keystream  
Like block cyphers they share a secret key (usually in 100s of bits in length)  
Truly random number sequences cannot be shared   

Pseudorandom numbers appear “random”:  
K bits in the output it should be impossible to know what the next bit will be
**PRNG** will have to keep track of an internal “state”. Even if the state is compromised it shouldn’t be possible to know what former “states” were (should be impossible to reverse the state)

States have a finite size, this means eventually state will repeat which means the keystream will eventually repeat  
This is the “period”. The number of random #s that can be generated before the state starts repeating.   
The goal is to have the period be long and unpredictable  
Old common algorithm was Ron’s Code 4 (RC4) - developed by Ron Rivest  
RC4 had a period of ~>10^100

### RC4
RC4 was a simple secret algorithm that leaked online in 1994  
State is initialized with a secret key  
The first few bytes leak some info about this key. Using RC4 typically means throwing away several hundred first bits  
RC4 was part of the SSL/TLS standard but was dropped as they do not allow for throwing away bits this means any workarounds for fixing it is useless

#### Algorithm
Short and Simple  
Internal State s is an array of 0-225 initialized to where the numbers are shuffled. It has two index pointers I and J  
These get shuffled as well in every manipulation  
256 from I and J each, and for the different spots for each number. But that is factorial (As 1 cannot be placed in the spot 0 is located)  
Total states is 256^2 * 256!  
That is a lot of states  

Initialize:

```
Array is set to identity (0-255 in order) = s[]
J and I = 0
For (I =0 … 255) 
J = (j+s[I]+key[I % key length]) % 256
Swap s[I] and s[j]
```

This creates a shuffled array

```
Reset I and J to 0
To generate the next byte of the keystream
I++
J += s[I]
Swap(s[I], s[j])
Return s[ s[I]+s[j]) % 256
```

### ChaCha20
This is modern algorithm. It is pretty well used (Google is a fan)  
Uses XOR, Rotate, and unsigned int addition  
In addition to a key, it also uses a nonce and stream position parameters to prevent stream reuse

### Stream Attacks
Even if someone in the middle doesn’t know the key specially but knows some of the plaintext they can change the data inflight and trick the receiver  
Example:  
Trudy wants to send ```N```. Alice is sending ```M```  
Alice sends ```C = M xor K```  
Trudy wants to send ```C’ = N xor K```  
```K = C xor M, can now create C’```  
This is a bit flipping attack  
So there is data confidentiality but not data integrity  

We need a way to authenticate that message hasn’t been tampered with. 