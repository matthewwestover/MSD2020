## Week 13 - Day 1
### Firewalls
It’s often important to keep some separation between a network and the greater internet  
For example, it’s probably not a great idea to expose your networked printer to the internet  
For business/government networks, it’s especially important that some things stay inside the network  
A “firewall” is the controlled door through with traffic enters/exits such a network

A firewall monitors traffic entering/exiting a network and blocks traffic that violates network policies  
A firewall can operate at various levels of the networking stack (lower is simpler/cheaper)  
If it’s possible to get traffic into/out of the network without going through the firewall, they’re basically useless. Example: a user sets up a wireless network whose signal reaches outside the building...  
We won’t mention it going forward, but firewalls can also do logging so that traffic can be analyzed/audited later

#### Packet Filtering
This type of firewall inspects packets entering/exiting  
Based on the given policy, it either forwards them, or drops them  
At the lowest level, these devices look at IP headers (and maybe TCP headers to determine things like TCP port)  
Policies look at source and destination address (and often TCP/UDP port), and whether the packet is ingoing/outgoing to decide what to do

#### Example Policy
Assume we have a firewall that wants to block all incoming traffic, except HTTP, but should let any user traffic in/out might look like this:

* Incoming traffic to port 80: allow
* Incoming traffic to any other port < 1024: deny (these are the other “server” ports) 
* Outgoing traffic to any port: allow
* Incoming traffic to any port > 1024 allow

This might still allow outgoing traffic to connect to high-ported servers!  
The policy can specify that incoming traffic to port > 1024 much have the ACK flag on in the TCP header, meaning that it was initiated from inside the firewall (and this is a response)

#### Packet Filters
The simplest type of firewall  
Some are capable of looking at higher levels of the network stack (called “deep packet inspection”), at higher cost  
It is possible to store some state in the firewall which can support higher- level policies  
Example: the firewall can track the status of TCP connections (track handshake status, sequence numbers, etc)

#### Application Proxy Firewalls
In this case, the firewall is the only source of traffic into/out of the network
Users send the parameters of their request to the firewall (“hey, make this http request to server X”). The firewall, then performs the request and reports back to the user  
Any requests not made in this manner (eg a client tries to connect to a server directly) are blocked  
These firewalls are secure, but obnoxious to users, and expensive. 
Any applications not supported by the firewall are blocked, and all traffic is inspected/forwarded at the firewall

#### Circuit Level Gateways
Basically, users make a TCP connection to the firewall, who then makes a TCP connection to whoever they wanted to contact  
The firewall just copies the data between these 2 connections  
Often these devices act as “application level proxies” for incoming traffic. 
These are lower overhead than ALPs because they just forward traffic rather than processing/inspecting it

#### Host Firewalls
Pretty much all OS’s support “host firewalls” that run ON the host machine  
These perform packet filtering or other operations before the packets reach applications. 
MacOS comes with 2: an “application level” firewall, and the lower level pf (packet filter) utility that comes from BSD Unix  
These are a sort of last-line defense, and can help mitigate the effect of malware and slow its spread

#### Authentication and Access Control
Authentication:  
One party, proving their identity to another  
Some means of ID:

* Something only the user knows (password, private key)
* Something only the user has (access to an email account/ phone/“token”)
* Something only the user is (fingerprint) Something only the user can do (voice ID

#### Password Based Authentication
Ubiquitous - (almost) All devices support this type of user input  
All software can deal with strings  
Flawed - Users should have unique, strong passwords, for each service they use and be able to remember them!

#### Token Based Authentication 
Relies on a user having a physical object that no one else has  
Examples:

* Pseudorandom number generator: service asks user what the currently displayed random number is. Some secret is used to generate numbers. Secret known by verifier, and stored in hardware on device
* Chip + Pin (modern credit cards). Card will do crypto stuff to the transaction description and your PIN
    * If using symmetric crypto, a secret key is stored on the device and is used to compute a MAC which is checked by the bank
    * If using public key crypto, the private key is stored on the card and used to perform signatures 
* Your student ID (RFID device, probably using public key crypto)

#### The User “Is” Authentication
Use static property of the user to authenticate  
Fingerprint or Retina Scan  
These are not revocable, so their use can be problematic!  
What happens if someone lifts your fingerprint, or photographs your retina?  Probably better to think of these things as usernames and not passwords

#### The User “Does” Authentication
Voice recognition, signature  
This could be considered a non-cryptographic challenge/response protocol  
Difficult to balance cost/accuracy of authentication  
Difficult to balance risk of authenticating imposter vs denying access to a legitimate user

#### Mixed Token/Password
User has only copy of private key  
Private key can be encrypted with a passphrase. 
Service challenges user to sign something with their private key to authenticate

#### Authentication Attacks
Client attacks: password guessing/similar  
Host attacks: stealing password file, or fingerprint database  
Eavesdropping  
Replay  
Denial of Service. 

#### After Login
How does the system know who the user is?  
After I log in, how does the OS know that “Matt” is asking to open a file, etc?  
There are multiple users operating on our laptops as we speak! How to the OS keep them straight?

#### Remote Authentication
If I ssh into a machine, after I’m authenticated, both client and server trust that all subsequent packets are legitimate. The SSH protocol is carefully designed to guarantee this (defense against replay, eavesdropping, etc)  
But what about a stateless protocol, like HTTP(S) ?  
Once I log in to google, how does maintain my authentication across requests and websites?  
After authentication, the remote service sends back a token. The user sends this token with each request. The server checks the token they sent and verifies that they are an authenticated user  
On the web, this token is stored in cookies

#### Traditional UNIX Access Control
Permissions describe the operations users can perform on files  
The operations are read, write, and execute (rwx)  
Each file stores the permissions for 3 types of users: the owner, users in the same “group” as the file, and “everyone else”  
Type ls -l to see these permissions are. Any granted contain a wrx, any denied are filled with -  
This requires 2 bytes per file (3 x 3 bits). The extra 7 bits are used for storing other info  
You can manipulate permissions with the chmod system call/program  
It’s common to use octal (base 8, 3 bits per digit) to describe permissions in this way  
chmod 777 myFile # 7 = 111_binary, all bits set, all permission for everyone  
chmod 755 myFile #7 = 111 101 101: owner can do everything, group and read + executed, everyone else can read + execute  
The “execute” permission for a directory means, a user can cd into it

#### Limitations
This is a pretty coarse granularity!  
Each file belongs to a group, although users can (and do) belong to many groups  
This is limited to files which mostly works pretty well, since (as we discussed in systems 1), devices are files! But it doesn’t cover everything. Can a user shut down the system? Open a socket?

#### Access Control Lists
For finer grained access control for files, we can associate an “Access Control List” (ACL)  
This allows more than 1 group/user being associated with a file  
Most Unix-like OS’s use the traditional mechanism by default, and files can use ACLs if desired

#### Role Based
Role Based Access Control is based on the idea behind “groups” in the traditional Unix model  
Users are assigned “roles” — jobs/responsibilities in the organization  
When a user requests access to a resource, the access control system examines the roles of the user, and which roles the object grants which permissions to, and allows or denies the request  
Example: one user might have the database_admin role, and is allowed to run any command in the database  
A user in the sales_analyst role may have read only access to the sales database only. A user might have roles added/removed as their job requirements change