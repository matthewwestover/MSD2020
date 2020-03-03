## Week 1 - Day 3
### Application Layer
Application Layer is built on the transport layer. Utilizing TCP and UDP

**Client - Server Model**  
One host (the client) initiaites requests to another host who is awaiting connections (the server)  
Server sits on waiting for connections  
Client sends connection request  
Connection is made  
There can be back and forth between/constant communication or communication is a one and done  
One and done is like HTTP

This works for many things, but not all things  
Often things act as client and server (C and S together)  
Peer to Peer - where everyone is both host and client. You request other clients to send stuff, other clients request you to send it  
This is common for torrenting  
Skype used it until Microsoft bought them

Things can be like a Client, connecting to a client/server node to a server  
This is like a proxy  
You can’t directly talk to the end goal, you go through the proxy  
Client talks to proxy saying I want to do this, it makes the request to the end goal. It receives the response and then sends that response back to the original client

Because the application layer is built on the network layer we MUST use TCP or UDP. These both different set of specific protocols  
Both identify the hosts using their IP address and a port number  
IP allows network layer to route the packets to the right location  
Ports allows the host to have multiple request/things running

TCP layer guarantees its data is received in order. UDP does not. TCP is slower than UDP because of this  
Streaming and Gaming uses UDP because its better for faster transfer with some loss. Basically a higher throughput for possible data loss. 

### Example protocols
**HTTP** - seen a lot specific header, response from server of all files.  
**Mail** - has several protocols

* SMTP - Simple Mail Transfer Protocol
* POP - Post Office Protocol
* IMAP - Internet Message Access Protocol
* This is how mail messages are sent. There are a lot of restrictions as many devices cannot use it. You have to talk to a mail server that will send it to another mail server
* A mail server will listen for SMTP connections and either store the messages it receives in a user's mailbox or forward it to the mail server for the user that is receiving it
* It is up to the user receiving the email to use POP or IMAP to request the mail server to provide any of their email. 
* Mail uses TCP to guarantee all data in the email is sent/received

**Bittorrent** - special Peer to Peer protocol (P2P) 

* A user has a list of peers with whom they are okay communicating with
* Users request chunks of data from peers and peers request chunks of data from the user. 
* Instead of hosting on a single server with high capacity for many clients, all clients become the server together
* File starts at one place. As more users get the file, the more spread out the file is and the cost of getting the file is less. 
* It then possible to get the file even if the original source is gone
* That means there is also single point of failure, if one is removed the data is still out there
* People get prosecuted for uploading files. 
* A client connects and asks its peers what part of the file they have. Client then asks for chunks it doesn’t have.
* A client can see their neighbors’ neighbors
* There has to be a “tracker” server that knows the starting neighbors. Basically it would list who was in the “swarm” of the peers
    * This centralized thing only says these are the addresses of people that have this file. Nothing about the file itself
    * Leads to many legal issues so trackers are less common now

### DNS: Domain Name System
* This is an application layer protocol that allows the network layer to work for humans.
* Network layer identifies machines via IP numbers (32 bit or 128 bit numbers)
* Humans suck at remembering IP numbers but are good at remembering names.
* This is a “map” between the IP address and the Host name (ex: http://www.google.com)
* Host name can have a list of available IP address
* This system is a client - server set up typically all a client is requesting is the IP address it needs to go to
* Client sends a DNS query to a DNS resolver, which replies with a DNS answer
* This is not a persistent thing. Single request, single response. Done all over a UDP connection
* A DNS resolver has to have access to all DNS/IP maps. They change all the time, so trying to update all DNS servers all the time is impossible
* DNS resolvers have a cache of recent IP addresses that it thinks clients will request soon. 
* If the IP is not in the the cache, it just asks another Server
    * Recursive Approach - first just checks next resolver has it. Which can then ask the next one if it has it.
        * If found it is forwarded back on to the client
    * Iterative Approach - It looks in chunks. Each chunk is responsible for telling you the next location is stored.
    * Second highest level is the Top level domain (.com, .edu, .uk, .dev)
    * Root DNS server knows who to ask to find the top level domain. They are spread out over the world and know all the top level domain locations
    * Example: shell.cs.utah.edu
        * DNS resolver asks root server where .edu servers are
        * Asks .edu servers where .utah.edu domains are
        * Asks .utah.edu servers where .cs.utah.edu domains are
        * Asks .cs.utah.edu where shell.cs.utah.edu is and returns it to the client
* DNS records have a name, value, type, and TTL
    * Name is the domain name
    * Value is the IP address(es)
    * Types:
        * An "A" type record is a domain to IP map.
        * An "NS" record is a nameserver request, and gives the authorative server for the name.
        * A "CName" type record gives the "canonical name" for a hostname. This is used for "aliasing" a host
        * "MX" records are for mailserver information
    * TTL = Time to Live - typical short for how long the info of the IP is in the DNS server cache