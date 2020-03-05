## Week 8 - Day 3
### TLS
Client -> Server  
\> “hello”, supported ciphers, Rc >  
< “hello”, chosen cipher, Rs, Certs <  
\> E(PreMasterSecret, Spub) >  
< MAC <  
\> MAC >

The TLS is about the server proving it is what it is. The client just goes “I know TLS here is a random number”  
As a User we see a little padlock on a browser showing the server authenticated itself  
Logging into a bank(something secure) the bank proves itself first, then the user does to the server.  
The client authentication to the bank doesn’t have to be super clever
The TLS establishes confidentially and message integrity by the time the client is ready to authenticate back  
Once server is authenticated, all handshake info is sent back in the MAC from client

**Forced Downgrade**  
Trudy can remove all the supported ciphered but the easiest to crack  
The final MAC (HS Info) lets both sides verify that the messages were the same throughout

Both sides then generate 2 keys (4 total):

* Kc - Encrypt
* Kc - MAC
* Ks - Encrypt
* Ks - MAC

### TLS Record
This is a TCP segment - but the data field is encrypted and contains extra header info based on the TLS  
```[TCP Header[TLS Header[Encrypted Data[MAC(Data, header, sequence#]]]]```  
TLS Header and Data is the TCP “data” field:

* Type - what data is being sent - TLS can set control stuff to trigger new key generation, etc  
* Version
* Length - How much in the data is encrypted. This is important for the block as a block might not be totally full with data to decrypt

After Encrypted Data, it also contains a MAC of data/header/sequence# for more verification and ensuring data isn’t going missing or dropped or added

Because the TLS header is included in the MAC of data, the end side knows if the public TLS header had been changed or not. Meaning it can tell if it can trust the message was sent from the right person.

This does not give anonymity. The TCP header shows who is talking. All provides is that what is being communicated is confidential. 

VPNs give semi anonymity. It shows a user communicating with a VPN server, but not what the VPN is forwarding to and from.  
True anonymity comes from TOR which is more complicated. 

SSL was the older version of TLS, from the 90s.  
Transport layer and TCP means reliable data transfer  
It can go onto of the network layer instead  

**IPSec** - > TLS but built on the network layer  
```TLS:TCP :: IPSEC : IP```

Basically adds this security to IP: confidentiality, authentication, data integrity, etc.

We want spread out internal networks (two office buildings) that look internally as on the same thing as one virtual office  
Do this without hardwiring a direct connection between the buildings. Send this securely over the internet 

Office 1 -> Gateway 1 -> internet -> Gateway 2 -> Office 2

A in office 1 wants to send to B in office 2  
A sends like any normal packet - not worrying about the encryption.  
It hits the Gateway 1, it gets wrapped up, hidden, including that A is sending to B  
It now shows as From G1 and going to G2  
At G2 it unwraps and sends on to B like it was a normal not encrypted packet

This is built on an “Security Association” -> one way IP “connection”/“transmission”  
Need one for each direction  
Contains:

* An ID - the SPI - Security Parameter Index
* IP addresses of the endpoints (sender + receiver)
* Encryption algorithms used + key
* Message Authentication algorithm used + key

This basically wraps and IP header inside another IP header. The inner is the one from A, and encrypted.  
IPSec packet: 

* sequence number and SPI
* The whole original packet (header + data)
* Message Authentication Code

```[New IP Header with G1 and G2[IPSec packet with encrypted stuff]]```