## Week 8 - Day 1
### Crypto building blocks
* Block cipher
* Steam
* Hash functions
* DHKE
* RSA

None of these automatically mean security however.  
Two parties need to authenticate each other and use these building blocks to create a secure communication format

### Authentication Protocols 
Alice and Bob want to talk and have to use these building blocks to establish they are in fact talking to each other, and not Trudy who wants to interject into it

Bob can examine the packet source IP to confirm it is Alice. This only works if Alice’s IP is known and fixed. Trudy can spoof Alice’s IP as well. 

#### Simplest Authentication Scheme - Just saying who you are
“I’m Alice” - username. 
This is literally how the earliest computer networks did it.  
But Trudy can also say “I’m Alice” which breaks it  
Alice can send a password with her username  
Trudy can pretend to be Alice only after the first time Alice logins  
Alice has to use crypto stuff to hide her password so Trudy doesn’t see it.  
Alice can used crypto to hide her password - but Trudy can send the same crypto’d password to Bob and there isn’t any difference in what Bob sees.  
This is a **replay** attack

Alice and Bob can use DHKE to establish their shared secret key. Trudy cannot know the secret key. Key = Kab

#### Time Verification
Alice can send the time. Bob and Alice know an approx time for things to be sent between them.  
Alice sends Encrypt(timestamp, Kab)  
Bob decrypts and makes sure he got a recent timestamp  
Clocks aren’t synced perfectly, there can be hiccups along the networks so there is a “slop” off time for margin of error in the time.  
If Alice uses the same key for multiple servers (authenticated with google who has many servers for example) an attacker can replay attack if the time difference is acceptable to closer/further servers. Alice can add the name of the server with the timestamp to prevent this.  
Another tweak would be for Alice to send timestamp + hash(timestamp + Kab).  
This allows Alice and Bob to use high resolution clocks without using reversible encryption.

#### Random Numbers
When Bob receives an “I’m Alice” message and sends out a random number - R#  
Alice replies to the random number with Encrypt(Kab, R#) - or HMAC (hash functions)  
Bob gets that encrypted random number and Decrypt(Kab,R#) to verify that it is the same number - thus verifying Alice is Alice.

R in this case is known as a nonce - R should only be used ONCE and thrown away. Trudy cannot replay as new Random numbers are used each time. Each login should use a new nonce

Nothing from this proves that Bob is Bob from Alice’s point of view. Trudy can just ignore Alice’s response to R and communicate as Bob  
If Trudy sees a lot of these logins, she has R and E(R, Kab) and thus could begin to guess what the Kab value is - offline guessing  
If either side of the communication got hacked and Kab gets out. Trudy can pretend to be either side  
Once Alice is authenticated with Bob, Trudy can cut off Alice and pretend to be her - a **session hijack** attack

### Random Numbers - Tweaked
Instead of Bob sending R# - Bob sends Encrypt(R#, Kab). Alice responses with R#  
This has to be encryption as R needs to be able to be retrieved by Alice  
Alice can be more confident that she is actually talking to Bob

#### Public Key Crypto
1. Bob sends back R to Alice.  
Alice signs R with sign(R, Private Key)  
Bob can check by decrypting the signed R with Alice’s public key

2. Alternatively Bob can Encrypt R with Alice’s public key  
Alice can decrypt with her private key and reply with R to verify it is her

##### Two major issues here unrelated to authentication
1. Only Alice can sign. Signing also proves someone did something  
R can be something like “Allow money transfer” or something that Alice literally signs

2. Trudy can send a message to Alice that Alice then broadcasts the unencrypted version of it

#### Mutual Authentication
Alice shouldn’t assume Bob is Bob either. Both need to verify they are who they say they are  
Alice sends “I’m Alice”  
Bob replies with R  
Alice responds with E(R, Kab)  
Alice then sends R2  
Bob replies to R2 with E(R2, Kab)  
They then know they can trust each other  
Bob shouldn’t send anything beyond the original number until Alice proves she is Alice  
The initiator has to prove themselves first

If Alice sends I’m Alice + r2  
And Bob replies with R1 + E(R2, Kab)  
Trudy can do a reflection attack.  
Trudy sends her own number to Bob  
Bob replies with a Number and E(Rt, Kab)  
Trudy tries again with sending bob’s number back at him, so Bob then will encrypt his own number he send as a challenge  
Trudy can then pretend to be Alice by sending that response back to the original challenge  
If there is preagreement that R from Alice and R from Bob have to be different (Alice is always odd, Bob is always even) this can be avoided

#### Mutual Authentication with Public Keys
Alice sends "I'm Alice", Encrypt(R2, KBpub)  
Bob replies with Encrypt(R1, KApub), R2  
Alice replies R1  
Order of authentication matters
