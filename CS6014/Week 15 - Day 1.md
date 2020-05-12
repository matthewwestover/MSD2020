## Week 15 - Day 1
### Course Takeaways
#### Networking
* Know what the network layers are, what they're for, generally how they work. Understand what layer you need to work at to get a job done (application layer, but maybe not always). Understand enough about how they work to debug them when they don't (DNS issue, low throughput over TCP because of packet loss or misconfigured rwnd or cwnd, etc)
* Know how to do basic network programming. Regardless of the language you write in, you'll be using some sort of socket like interface. What info do you need to specify to write a networked client/server? What are some common issues you face when writing these sorts of programs (blocking vs nonblocking APIs, formatting of data so the other end knows how much to read, etc).
* Know common protocols, what they're for, and generally how they work (HTTP, DNS, TCP, etc). Where is the code that implements them (kernel, application layer, hardware, etc)?

#### Crypto
* Know the basic cryptographic tools (cyphers, hash functions, public key stuff), their uses and their guarantees. I don't expect you all to be security experts or mathemeticians, but you should be able to point out some flaws in cryptosystems (like the example of serving a file + its hash over unsecured HTTP). When you read best practice advice, you should understand at a high level why it's a good idea (for example, password storage. "I understand why they use a salt and chose a hash function that is slow to compute").
* Know common algorithms and what they're used for (AES is a symmetric block cypher, RSA is a public key cryptosystem for signing/encrypting things, DH is for making shared secrets, RSA is an outdated stream cypher, SHA is a hash function, HMAC is a keyed hash function, etc). If someone tells you they're using AES for something, know that somehow there has to be a shared secret key, etc.
* Know the common crypto protocols (TLS, IPSec) and what they're used for and how they work at a high level. The TLS handshake is a good case study of all that needs to go into one of these systems.

#### Security
* Know the common types of attacks so you can code defensively. Be paranoid about writing code vulnerable to buffer overflows. Be paranoid about user inputs. Think about how an attacker might try to compromise yoru software and put safeguards in place.
* Consider other vulnerabilities that aren't necessarily caused by bugs (side channels, DOS). What can you do to try to mitigate those?
* Think about compartmentalizing things so if part of your software is compromised, the damage caused is limited. Use access control/privelege control/isolation to avoid escalating failures.
