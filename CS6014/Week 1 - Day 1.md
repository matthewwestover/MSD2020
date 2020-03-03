## Week 1 - Day 1
### The Internet
There are billions of devices on the internet  
Each can communicate with each other  
n(n-1)/2 amount of connections (N^2/2)  
Billions of devices means quadrillions of connections  
Not feasible to have that many connections direct one to one  
Use a min-spanning tree - a graph using the “cheapest” route of edges so each node can connect to each other   
This is closer to an N level of connections  
Just because nodes are close together, they might be far apart on the graph  
There is no redundancy on connections  
Nodes can be expensive (literally buildings by companies)  

Internet is slightly more designed than this  
Hosts - end user (laptop, iPad, iPhone)   
Connects to access networks (Comcast, UofU)  
These connect to internet backbone - really high speed high capacity fiber links  
These connect to other access networks which connect to other hosts  

### Switching
There are more possible connections than actual links  
Network links constantly have to switch between connections to account for this  
Internet uses packet switching to handle this  

This predates the internet, via the phone lines (50ish years)  
Phones provide consistent connection until hang up. Done via circuit switching  
Start a call - find a connection to the end point, and make a reservation along that line.  
While call was happening, no one else could use those parts of the phone network  
This is good for consistent bandwidth usage  
Computer network communication is “bursty” lots of data then down time between requests  
Circuit switching is bad as it reserves and blocks the line for the entire open connection even if neither side is sending data  
It is limiting as if you don't reserve enough space, you cant transmit all the data  
It is “wasteful” as you have to over state what you need vs actually use  

Modern networking is based on “packet switching”  
Instead of reserving a stream for data movement for the entire thing  
Data is split into “packets” of data that are sent one at a time  
Connection from one end to the other is “best effort” attempt to get all of the packets to destination  
This means one entire set of data can be sent in multiple different links without the end user knowing it went along different paths  
Nodes don't do anything with a packet until the entire packet is received  
Nodes have queues to store packets in while they are received/sent  

Sounds like it might be unreliable for VoIP (Skype etc) but works pretty well  
These waste much less bandwidth

Network programmers like to think of data as streams of bytes. This is broken down into the OSI model and different layers. Each layer works by sending packets.

* Application Layer -- High level, application specific protocols like HTTP, DNS, SSH, SMTP/IMAP/POP, Bitorrent
* Presentation Layer -- Not in the original OSI model, often used to provide encryption below the application layer
* Transport Layer -- TCP and UDP. Provides some guarantees on delivery and adds nice features for programmers. Process to process communication (port to port) UDP adds ports, TCP adds reliable data transfer. Stream you receive is the same stream that was sent
* Network Layer -- How are packets sent across the network? Host to Host communication. The end points of the connection not local node to node but across the entire thing. This is IP (internet protocol) 
* Link Layer -- How are packets sent to the next node in a network (connecting adjacent nodes. Laptop to router). Ethernet, wifi, how do both sides of the connection understand the data. 
* Physical Layer -- How do bits get turned into electrons or photons or radio waves, etc. (we won't spend much time here). Based all on physics.