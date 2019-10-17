## Week 1 - Day 2
### Network Archtecture
Two types of things that are communicating with each other  
Clients and Servers  
Client contacts server to initialize  
Web browser is a network client and the one we are most familiar  
Other end of communication is the servers that host the various webpages  
Laptop connection to wifi router is another one of these network connections. It allows other client/server connections  
Email client like Outlook talks to email server to manage emails  
Bittorrent is an example where everyone is a client and everyone is a server at the same time  

Socket - open connection to another machine  
Open connection with one other program  
Is possible to “snoop” on it, not necessarily private  
We communicate through sockets  
Read and Write to sockets  
Client contacts server to open socket  
Server is listening for client requests to open on its end of the connection  
Once open socket is bidirectional and full duplex. Both sides can send and receive at the same time  
Often they don’t, they take turns to not interfere with each others signal of data  
Sockets guarantee that data will be received in order on the other side without corruption  
We send bytes back and forth  
Other programs have to agree on what bytes mean  

Protocols are the rules for communication  
Allow for translating what bytes mean  
What we can send and what we can mean  
There are many different kinds of protocols  
Can be text-based or binary  

* Binary is more compacted. Text is easier to debug


Can be state-ful or state-less  

* Is anything remembered between connections
* State-less needs context data to remember things
HTTP is a text based state-less protocol - web protocol
* Once communication, server can forget everything about the client
* Web cookie is stored on the client that lets it remind the server with context


SSH is a secure binary protocol  
FTP is a file transfer protocol, similar to HTTP but more state-ful. More of a constant connection  
TCP - lower level protocol, allows sockets to work, reliable data transfer, connection/congestion management  
UDP - like TCP but un error checked program to program. Doesn’t do any reliable checks. Is fast. Often used for live gaming  
DNS - converts sites to numeric value for connection  
IPP - internet print protocol  
TOR - lets two machines talk but the end points can’t be seen  
DHCP - computer talks to router for wireless connection   
Most Important is IP - internet protocol  

* Two versions used IPv4 and IPv6
* IPv4 has been sunsetting for ten years
* IPv6 has been in use for 20+ years just been slow to be adapted
* Address for where data goes and how to route it there

Protocols are layered. HTTP and FTP rely on TCP to function  

Lowest layer to highest: **OSI Model**  
*Physical Layer* - the waves of energy (radio waves, electrical waves, light pulses) that is transmitted between client and server.  
*Link Layer* - rules for two physical devices communicating with each other. Wifi and ethernet. (Laptop and router)  
*Network Layer* - multi-hop machine to machine communication (home to the most important protocol IP)  
*Transport Layer* - program to program communication, each program is given a port number to identify what process is communicating. Uses TCP and UDP. Also QUIC which is built on UDP but similar to TCP. Transport is the socket layer
Encryption Layer is more modern addition, half step between transport and application layer  
*Application Layer* - rules for specific applications. HTTP, FTP, TOR, are all specific to the type of application that use them 

*DNS* - Domain Name - name for a location on the internet (like google.com). It translates between the “name” and the IP address. Application level but without it, the network layer wouldn’t work  

MAC address is a combination of the link and network layers. 

These layers get wrapped.  
Application layer is wrapped by transport layer is wrapped by network layer  
Layers are added and removed as data transfers until it finally reaches destination then they are stripped off to transport/application layers.