## Week 9 - Day 1
### Wireless Security
“Wired Equivalent Privacy” - **WEP** - BAD  
This was the standard from 1997 for wireless encryption  
Encryption at the time was seen as a “weapon” and needed a license to be exported  
WEP was designed weak to avoid legal issues with the government  
Built on a 40 bit key - 10 hex digits  
Had to remember a 40 bit key - ex. ```40A8C1 ```   
Couldn’t convert a password to the hex key

Connection to wireless between Client and Router  
Client says “connect”  
Router responds with a Nonce  
Client responds with a E(R) with the 40 bit key.

Biggest issue was how the packets of data was encrypted after connection  
Packet would include an extra 24 bits of unencrypted data tacked on to the 40 bits of data.  
```[24 bits of random(IV) [encrypted packet data]```  
Each packet Key = shared key + 24 bits of random (IV)  
Encryption uses RC4

To encrypt new RC4(key)  
Ciphertext = Message XOR RC4 bytes  
C1 = M1 XOR K  
C2 = M2 XOR K  
C1 XOR C2 = M1 XOR M2. 

RC4 once set and runs can last a long time  
Making new RC4s every time is bad. There is also shared bits every time  
The shared key is the same always, 24 bits change every time. Only 2^24 options = 16777216 keys  
These are per packet. Lots of packets made for a single data request  
After 12,000 packets of data there is a 99.9% chance of a collision of same keys (birthday paradox)  
The 24 bits always NOT encrypted as it is needed to decrypt. Trudy can record all 24 bit sent, easy to record and look for dupes  
RC4 quality also depends on the shuffle made based on the key. A bad shuffle = weak encryption  
First few bytes out of RC4 make it easy to get info about the key. This is why RC4 usually throws away several 100 bytes  
WEP uses a fresh RC4 every packet, it is ALWAYS using those first bytes

In 2001, Adi Shamir (the S of RSA) published an attack that could crack the WEP key in about a minute through eavesdropping only.

Anyone on the network knows everything to easily break it. 

### Wifi Protected Access - WPA
WPA, AKA 802.11i, was a pretty big improvement over WEP.  
**Cipher suite negotiation**: The AP broadcasts what types of encryption and authentication is supports, like we saw in the TLS handshake. This means it can be updated and supported.  
**Separate Authentication Server**: The AP can act as a mediator between the client an a separate authentication server (the AP could also act as the AS, in a home network, for example). This allows centralized management that all APs talk to when authenticating clients  
**Flexible authentication**: The particular authentication method (like in TLS) is negotiable and can include public key techniques to generated a shared session key  
Stronger encryption (AES since WPA version 2, for example)

### TOR
We have encryption between end points, and can verify that no one can tell what the conversation was between points  
We have not covered how mask who is talking  
Confidential conversations vs anonymous conversation  
Strives for true anonymity  
TOR (the onion router) is a protocol for transporting data anonymously. Like TLS, it is built on the transport layer (TCP) and presents a transport layer-like API.

Because packets contain data for end points. We know who is talking.  
Add a VPN in the middle, we change from Alice to Bob to seeing both Alice and Bob communicating with the VPN  
Possible to compromise the VPN  
Can see bursts of traffic on either end of the VPN to make guesses as to who is communicating.  
Solution is to add multiple nodes between things:

``` 
A -> X -> Y -> Z -> B
A only knows about X
X knows about A and Y
Y knows about X and Z
Z knows about Y and B
B only knows about Z

B needs to know about the data. E(data, KpubB)
Z needs E(E(data, KpubB) + B, KpubZ)
Y needs E(E(data, KpubB + B, KpubZ) + Z, KpubY)
X needs E(E(E(data, KpubB + B, KpubZ) + Z, KpubY)+Y, KpubX)
```

X is an entry node, Y is an internal node, Z is an exit node  
TOR typically goes through 3+ internal nodes  
There is a service that lists all connections between internal nodes.  
The client chooses 3 random nodes in 10 minute intervals to send across. It needs to know XYZ  
X and Z is legally dangerous as it can be connected to something illegal  
Push for public libraries to host internal and exit nodes  
All nodes are run by volunteers. 

Tor works a bit like TLS: the plaintext data the client wants to send is encrypted an encapsulated in a TCP segment before being sent. This process is repeated several times making a multilayered onion-like network.  
The client is directly connected with an "entry node," their traffic is passed through a network of "middle nodes," and eventually hits an "exit" node where the TOR network connects to the rest of the internet.  
The client message takes a long windy path through middle nodes of the TOR network. For each step in this path, the message and its origin/destination are encrypted such that a middle node only knows about the nodes directly before and after it.

As an analogy:

* The user write a letter to the exits node, puts it in an envelope with the return address of node N-1.
* Then it wraps that up in an envelope addressed to N-1, with a return address of node N -2
* Puts in an envelope addressed to N-2, return address N-3

Given enough hops, entry nodes, and exit nodes it should be extremely difficult (hopefully impossible) to determine who is responsible for what traffic is sent.