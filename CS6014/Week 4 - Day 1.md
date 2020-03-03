## Week 4 - Day 1
### Network Address Translation - NAT  (Lie/Share about Addresses)
As far as the internet is concerned, multiple devices connected to a router are all seen as just the router  
Individual devices are “subnets” known only to the router. These typically are 10.0…. Or 192.168…. formated ip addresses.  
These are known to the router/connecting device. That connecting device has a “public” IP address that the internet knows  
This means that the IP header has to change. Servers cant send back when there are millions of devices with “local” addresses. The router changes the IP header.  

Example:  
Local network IP is 10.0.0.1 and use port 9999 to connect.  
Routers IP is 5.6.7.8.  
Router will use NAT to change the header to an IP of 5.6.7.8 and change port from 9999 to 7777  
When server sends packets back to 5.6.7.8 on port 7777, the router forwards its to 10.0.0.1 on port 9999  
Each local network IP address has a chunk of ports stored NAT device  
So an IP of 10.0.0.1 port 9999 gets translated to 5.6.7.8 port 7777, that the router translates back to send the packet 

It is still possible to overload the network if there are too many devices stored (~65,000)  
Router keeps a table of active connections to look up for translating the IP header. This comes at a cost  
This is affordable as you dont need to buy as many IP addresses.  
There is a bit of security as if incoming packets don’t match the table they can drop  
You have to stay in sync with the NAT table  
Have to handle the translation to/from NAT on local IP network

### IPv6 (Make more addresses)
IPv4 is limited to about 4 billion addresses. NAT is a bandaid to fixing this issue.  
The “real” solution is to make new addresses, using a different numbering system  
IPv6 started in the 90s, using a 128 bit address instead of 32 bits  
It also fixes some other issues, IP headers are fixed sizes. There are also no fragmented data, if a packet is too big it is dropped.   
IPv6 has been slow to roll out as all nodes in the network need to know about it.  
Many newer routers are “dual-stacked” in that they know both IPv4 and IPv6 addresses  
There is also “tunneling” when a packet finds a IPv4 only node, it gets wrapped in a IPv4 header, then unwraps once it back at a IPv6 capable node. The wrapping and unwrapping has to be done by the dual stack prior / after.  
This is a complicated system

In deployment since 2008, still only at ~25% internet traffic is IPv6.  
Because NAT has worked as a good bandaid on IPv6 there has been a slow growth to push IPv6. No one is going to develop a IPv6 only device.   
Not good incentives for people to upgrade IPv6. It will just be a long long slow process. 

### Match and Action
Routers still have to decide where to send a packet of data.  
Look up in a prefix table - longest possible prefix and send it in the right direction.  
With NAT it looks up in the NAT Table, translates it, then looks up where to send it. 

Modern Networks do REALLY fast match and action, and just build new action implementation