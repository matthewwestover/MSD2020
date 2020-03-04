## Week 7 - Day 1
### Sharing Keys
For Crypto we have certain goals:  
**Confidentiality** - Block Cypher, Stream Cypher  
**Authentication** - HMAC (Hash Functions)  
**Message Integrity** - HMAC  
**Non-repudiation** (“no take backs” if something was sent can’t say it was never sent) - sender had to have the key to send something. It is sort of achieved due to the block cypher

These all require a shared secret key between two points  
Making a key is pretty straight forward. Encrypt something, hash something, pseudo random generator, etc.  
Sharing the key is hard. WWII they would literally share books and keep them close, destroy if enemies were over taking

### Diffie Hellman Key Exchange
One of the most important advancements in history  
Developed in the 1970s. 

Alice and Bob need to have a shared key  
There are two publicly known “data” **g** and **N** (numbers agreed upon by everyone)  
Alice and Bob shout out numbers based on this - **Ta** and **Tb**
These are g and n mixed with personal secret data.  
**Ta = g^SA % N**(SA is a random number only Alice knows)  
**Tb = g^SB % N**(SB is a random number only Bob knows)  
By modding by N there is no way to calculate what SA or SB is which at least one is needed to break the key  
%N loops around by how big N is.  
If %N is not known it is possible to logg(Ta) to get what SA was or logg(Tb) thus making it useless

Once both hear each other, Alice and Bob has a shared secret key while no one knows what it is even though the initially known letters were known, and what was shouted was heard.  
Both share **Key(ab)**  
Alice’s KeyAB = Tb^SA = (g^SB)^SA = g^(SA\*SB) % N
Bob’s KeyAB = TA^SB=(g^SA)^SB = g^(SA*SB) % N

As soon as KeyAB is known by both, SA and SB are forgotten. If SA or SB is every leaked then anyone can generate the key  
Sa or Sb HAVE to be known to get the key. They are the only private things, and each side only knows one of them.  
Everything else is publicly. Sa and Sb are never sent/shared itself. 

Bad example:
N = 13 ~11 total keys due to modulo 0 and 1 are both kind of useless values
G = 3

| i | g^I % N |
|:-:|:-------:|
| 0 |    1    |
| 1 |    3    |
| 2 |    9    |
| 3 |    1    |
| 4 |    3    |
| 5 |    9    |
| 6 |    1    |

Only three different keys are generated, this is fairly easy to crack  
Use g and N that are recommended/studied by other sources to get good values

T is the public key, S is the private key.  
Public key is shared, private is kept

### RSA - Async Crypto
Encryption and Decryption keys are different, not the same one for both  
It is more math dependent  
One person:  
**P,Q** = big prime numbers  
**N** = P\*Q  
N is public, assuming no one can figure out what P and Q are  
**Phi(N)** = (P-1)(Q-1)  
Pick a number E that is < phi(N)  
Find D such that E\*D%phi(N) == 1 // This requires knowing P and Q to calculate

Private Key is <D, N>  
Public Key is <E, N>  
P and Q get thrown away - End result is two public numbers and one private number

Have a message M that has to be shorter than N  
Cypher Text (encryption): C = M^D % N  
Plain text (decryption): M = C^E % N = M^(DE) % N  
Because E and N are public, anyone can read it.  
C is really a a digital signature. Only the sender could generate that specific Cypher Text

This is way more complicated then the Diffie Hellman calculation 
DHKE didn’t care what the key was  
RSA does care what the key is  

RSA is  considered a “munition” and illegal to ship implementation. People protested by printing the code to a shirt and wearing it around  
In Perl:

```
#!/usr/local/bin/perl -s
do 'bigint.pl';($_,$n)=@ARGV;s/^.(..)*$/0$&/;($k=unpack('B*',pack('H*',$_)))=~
s/^0*//;$x=0;$z=$n=~s/./$x=&badd(&bmul($x,16),hex$&)/ge;while(read(STDIN,$_,$w
=((2*$d-1+$z)&~1)/2)){$r=1;$_=substr($_."\0"x$w,$c=0,$w);s/.|\n/$c=&badd(&bmul
($c,256),ord$&)/ge;$_=$k;s/./$r=&bmod(&bmul($r,$r),$x),$&?$r=&bmod(&bmul($r,$c
),$x):0,""/ge;($r,$t)=&bdiv($r,256),$_=pack(C,$t).$_ while$w--+1-2*$d;print}
```

To keep message confidential back to sender (Alice)  
Bob has to send the M ^ E from the public key.  
Alice solves from C^D = M  
Only Alice knows D and it is the only way to decrypt back. 

Two way communication means Alice does M^Bob’s E + her own  
Bob does M^Alice’s E + his own  
They can decrypt on their end. 

You can digitally sign by computing S =M^D % N  
Key can be verified by M = S^E % N

There are practical issues with this  
Finding P and Q isn’t as hard as you would think. A 100 digit number about 1/230 of them are prime numbers, so it can be easy to guess primes  
There are fast probability checks to see if a number is a prime or not (verifying is slow) these are like 99.999999% sure it is a prime  
This computes large exponents which is slow 

Exponents can be optimized to run in log time.   
Ex, power is a a 2^K value. 

```
x^(2^k) = x^(2^(k-1))^2 = X^2^2^2^2^2
Res = x
For I = 0 …. n
Res *= x
```

This can be done for things not a power of 2  

```
x^10 = x^(1010) binary value
x^(1) = x = a
x^(10) = x^2 = a^2 = b
x^(101) = x^5 = b*b*a = c
x^(1010) = x^10 = c^2
```

Find the left most 1 digit = S

```
Result = X
For I = S-1….0
    Res *= res
    If bit I == 1
        Res *= x
```

### Elliptic Curves
Both DHKE and RSA are “slow” lots of exponential stuff. Elliptic curves can help speed up  
y^2 = x^3 +ax + b

For DHKE  
g^X becomes x*g. G is no longer a number but a point on an elliptic curve  
Elliptical curve multiplication is like addition of points on a line  
If you don’t know what X is due to shape of the curve. Even if G is known, X cannot be solved.  
It still works to get a secure key but it does additions instead of ^ powers.

### Issues with DHKE
Alice and Bob want to talk. Trudy is in the middle listening  
Everyone is using the same G and N values  
Alice yells out her Ta.  
Trudy yells out a Tt using her own secret code. Alice and Bob generate keys based on what Trudy yelled, not what the other yelled.  
Basically Alice has a key with Trudy, Bob has a key with Trudy.  
Trudy can decrypt the message from Alice, then encrypt with her secret key shared with bob. 

There needs to be authentication about who you are talking to is the person you want be talking to so a key can be generated.  

Alice has to sign her Ta with her Key. But Bob has to know what Alice’s public is to get that value to validate.

### Signed Certificates
Certificates have info about an entity.  
It also has their public key  
This is signed with their digital signature  
But anyone could sign their own thing claiming to be someone else.  
Signing has to be done by someone who is more trust worthy. 

We look at the signers information, look up the certificate for the signer and repeat.  
Your computer stores “trusted” signers that is built where this constant look eventually stops at someone you know you can trust. 

cs.utah.edu is signed by Utah.edu is signed by .edu which you trust, so now you trust all levels there  
There are handful of “top-level” certificate authorities that your computer/browser trusts built in.  
Certificate public keys can be used for verifying the sender is correct, but not how to do the encryption/decryption needed. 

To communicate:  
Ask for a signed certificate  
Check signature if valid  
repeat process for their signer  
Eventually you hit one of the top-levels in your trust store

If anyone gets compromised, the system breaks.  
This HAS happened before  
Some signers are now blacklisted and not seen as valid.  
CAs charge a lot of money to basically validate your ID once then key a certificate on file

### Block Cyphers in Use
Messages are pretty much never exactly 128 bits.  
The “Mode of Operation” is how a block cypher sends a long message.  

### Electronic Code Book (ECB)
Take long message, break up into blocks.  
Encrypt each block, send them one after another using the same key  
Blocks are independent, blocks can be lost, blocks can be swapped out of order.  
New blocks cant be made, but they can be duplicated.  
Possible to do bad stuff based on this - tampering  
Can be seen in the ECB penguin . Penguin image is encrypted but shape can still be seen.

### Cypher Block Chaining (CBC)
Still use the same key to encrypt like in the ECB  
However before encrypting, XOR with the cypher text of the previous block  
First block is XOR with a initialization vector  
This can help detect tampering. Blocks have to be in order or it breaks.  
The slightly opens it up to be attacked by bit flips  
This breaks the previous block however, so it can be noticed. 

### Output Feedback Mode (OFM)
Turn the block into a stream cypher.  
Make a random block with an Initialization Vector  
Encrypt to get pseudo random data, repeat to get the stream.  

### Counter Mode
Same as OFM but use IV + 1 for first block, IV+2 for the second one

### Galois Counter Mode
Common on the internet. Works the same as the counter mode. But it also uses the MAC address