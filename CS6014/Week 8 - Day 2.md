## Week 8 - Day 2
### Session Keys
The longer the shared key(Kab) between Alice and Bob lives the worse security becomes:

The longer we use a key, the more ciphertext + plaintext pairs an eavesdropper could collect and use to try to mount an offline attack  
If the key is leaked/stolen all conversations using that key can be decrypted!  
If we need to communicate with untrusted software, we shouldn't give them a long term key  
Alice and Bob shouldn’t trust they are talking to each other continually or can trust each other forever
Session keys are temporary, never written to disk. Each “session” of a connection is unique  
Perfect Forward Secrecy - even if an attacker has access to all sent messages, old messages cannot be decrypted, and future ones cannot be decrypted with a single session key

***“Best way to keep a secret is to never tell anyone”***

### Non Diffie-Hellman Key Establishment
We have as several messages in authentication phase. We should be able to use these messages to create a session key as well.  
Authentication establishes a shared Kab, public/private key pairs, and random nonce(s).  
Bob challenges Alice with R and she responds with f(R,Kab).  
Bad choice f(r+1, Kab) - trudy sends R+1 to Alice responds with f(R+1, Kab)  
Good f(r, Kab+1) - No way to get either end to send Kab+1  

With public key crypto, Alice can choose the session key (S) and send bob encrypt(Sa, Bpub).  
Trudy can hijack this however by sending encrypt(St, Bpub) to Bob as his public key is public. Bob wouldn’t know it isn’t Alice.  
Alice could fix this by sign(encrypt(Sa, Bpub), Apriv). Trudy cant forge the message, but bob’s private key could be stolen and old messages could be decrypted. This also involves certificates for Bob to trust Alice to begin with.

The “Almost Diffie-Hellman”. Alice and bob send random numbers encrypted  with the public keys. Alice sends encrypt(Ra, Bpub) and Bob sends encrypt(Rb, Apub). The session key would be computed based on these two (Ra XOR Rb). Trudy doesn’t know either. 

There is no Perfect Forward Secrecy with these. If Kab or either of Alice’s/Bob’s secret keys it can crack all the messages sent. 

### Diffie-Hellman
Alice and Bob send Ta and Tb encrypted using their partners public key (just like before)  
```Ra = Ta = g^Sa, Rb = Tb = g^Sb```  
Bob sends  Sign(g^Sb % N / Tb)
Alice sends Tb(verifies this is Alice), Sign(g^Sa % N / Ta)
To solve for Sab

```
Alice = Tb^Sa % N
Bob = Ta^Sb % N
```
Ta and Tb are DH public keys
Sa and Sb are DH private keys

As long as Sa and Sb are forgotten after the handshake, Trudy cannot compute this without the shared secret session key. Only saved to RAM, never to disk  
G and N are public and agreed upon by protocol

### TLS
Transport Layer Security - how this is used in practice  
This is newer than SSL. This allows for things like HTTPS  
Lives between the Application and Transport layer

Consists of two parts:
 
1. A handshake that establishes any necessary session keys (and agreement about which ciphers to use)
2. Record format (for actually sending data)

Handshake is typically used to authenticate the server, S, with the client, C, (like talking to google directly) and then set up the session keys. It happens just after the TCP handshake. This is a slow process:

* C to S: Client hello, contains list of supported "cipher suites" and a random nonce (Rc)
* S to C: Server hello, server certificate (includes server public key), chosen cipher suite (which of the ones listed by client), random nonce (Rs)
* C to S: Encrypt(PreMasterSecret, ServerPublicKey) - PMS is based on Rc and Rs
* S to C: MAC(all handshake messages + "SRVR")
* C to S: MAC(all handshake messages + "CLNT”)

MAC is a keyed hash. Combo of hash function with data and a key  
At this point they can start talking. 

### Cipher Suites
TLS lets clients + servers choose which ciphers to use for the different parts of the connection.  
A cipher suite specifies: 

* key exchange algorithm
* bulk encryption algorithm
* MAC algorithm

An example suite is TLS_ECDHE_RSA_AES_128_GCM which means:

* TLS: we're using TLS
* ECDHE: Elliptic Curve Diffie Hellman Ephemeral, the key exchange algorithm. In this case it's DH using elliptic curves (the ephemeral part means we get PFS)
* RSA: RSA is used to identify the server certificate
* AES_128_GCM: Describes the bulk encryption AND MAC algorithms in this case. AES_GCM does authenticated encryption. AES_CBC_SHA256 would mean using AES in CBC mode for encryption with SHA256 for MAC codes.

Letting clients/servers negotiate which cipher suite to use balances backwards compatibility and deprecating old/vulnerable ciphers.

### TLS Record
An TLS record is a TCP segment, but the data field is encrypted + contains extra header info for TLS:

* Type
* Version
* Length

We compute an MAC of the record header, data, and SSL sequence number (which isn't actually sent). The header is sent unencrypted, followed by encrypt(data + MAC + padding).  
Each side gets 2 keys, one for encryption, one for message authentication, so a total of 4 keys are used.