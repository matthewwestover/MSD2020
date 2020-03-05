## Week 5 - Day 2
### Cryptography
We can move between points in a network  
We should not trust ANYTHING along the network  
Things have to be encrypted so the network nodes cannot spy on the data being transported  
Can mean creating ways to encrypt data or break down what is encrypted  
**Cryptography** - securing the data  
**Cryptoanalysis** - decoding the data  

_**Secure Communication in the Presence of Adversaries**_

We want:

* Confidential - only us and the end point know what it is
* Data-integrity - data stays the same from start to end
* Authentication - no one pretending to be us
* Non-repudiation - no take backs. Prove someone said what they said

#### Alice and Bob
Alice talks to Bob  
Eve - Listens to what is being sent between Alice and Bob (literally all nodes between Alice and Bob)
Trudy - Attempts to change what Alice tells Bob  
Mallory - the man in the middle - can send and receive messages meant from/to Alice and Bob  

Adversaries can hear and send whatever messages they want along the network  
If network is physically secure youdon’t need crypto (single link between ends)  
Internet has many links and we dont know what is between Alice and Bob

Alice sends a message - **plaintext**  
Alice puts plaintext in an envelope - Encrypting plaintext is the **cybertext**  
Bob takes plaintext out of the envelope - **decrypt**  
Bob and Alice have **secret keys** that only they know about  
They key is how the plaintext is put in the envelope and taken out of the envelope

If ```Key(Alice) == Key(Bob)``` this is symmetric key crypto  
If ```Key(Alice) != Key(Bob)``` this is asymmetric key crypto - most common for us is public key crypto  
Public Key - everyone has two keys a public one broadcast to everyone, and a secret one only they know  
Public key is from the 70s - requires computing power - one of the most important network advancements ever

Symmetric key crypto - hard to have both ends know what the key is in the first place  
Public key crypto is slow, do as little as possible then switch to symmetric for the bulk of stuff

### Historic Crypto
Caesar - rotate letters. ```x -> x + k % 26``` (modulo so it wraps around)  
Basically just offset in a loop around alphabet. ```Ex: a -> d, b -> e, c -> f … z -> c```

```
Encrypt(“hello”, 3) = khoor
Decrypt = encrypt(-k)
```

Pig Latin - move first letter goes to end + ay  
```Hello -> ellohay```

These illustrate two core building blocks for really crypto schemes

Important properties of building blocks:

* Efficient/feasible (computer needs to be able to do it for us)
* Reversible (or else we can’t decrypt)

### Important Crypto Building Blocks
**Substitution**: replace parts of message with something else (Caesar)  
**Permutation**: shift parts of the message order (Pig Latin)  
**XOR** (bitwise exclusive or): slightly nicer form of addition (no carrying/rolling over)  
Bits are independent in XOR. It is its own inverse a xor b xor b == a  

Mixing these things/repeating these things can make it very complicated

How to attack the combination of these:
  
* Cipher text only - only see the cipher data  
* Known plain text - cipher + plaintext (how enigma was broken)  
* Chosen plain text - attacker their own plaintext encrypted  

#### ciphertext
Should appear random  
Small changes in plaintext should result in large changes in the cipher text (small input change == large output change)  
“Avalanche Effect”  
Possibly combining the three main building blocks in various orders

#### Perfect Substitution
Assume we have a set block (64) for a message (always). It is possible to have a “perfect" encryption using only substitution (just impossible to actually implement in the real world)  
Create a table of all possible crypto text from the plaintext. As long as table has no duplicate crypto possibilities.  
The reason this is impossible to generate - you need the table as the key, and the key to generate the table.  
This becomes needing 2^64 (every possibility for every bit) times 64 bits. That is just way too many possibilities for a table.  
Fixed size can increase speed for encrypting / decrypting