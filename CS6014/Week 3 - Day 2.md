## Week 3 - Day 2
### Network Layer
Ex DNS Request: ```[Link Header[IP Header[UDP Header[DNS Data]]]]```  
DNS Data = Application Layer  
UDP Header = Transport Layer  
IP Header = Network Layer  

Network layer does path finding from start point to end goal. A packet at any node in the network needs to push it towards the final goal. 

For now assume that every node knows what the best direction is towards the end goal. 

Basic component of the network layer is the router. These are the nodes
Routers have an input port (queues of packets to come in), output ports (queues of packets to leave)  
What connects input -> output is the “switching fabric”  
Switching fabric takes from input port, does stuff, places it in outport
Routers also have a processor that tells the switching fabric what port things need to go to  
Physically input/output is the same connection (ethernet cable, etc) but better to think of as separate

For the router to send a packet, it needs to know where the packet is going (destination IP address)  
To keep track of all destinations, using IPv4 to ports it would take a map of around 4 billion nodes to have ALL ip addresses. IPv4 is 32 bit, IPv6 is 128 bit and would be impossible to store that many nodes/output links  

A router organizes IP address to use less storage. The internet gets to decide what an IP address is. It deliberately assigns IP address for where they are

A piece of mail first uses the ZIP code in the address to sort and send it to a rough location. Then it parses down to smaller chunks. 

Network layer does the same with IP address.  
IP addresses starts with an IP prefixes then differ at the end
These prefixes are mapped to ports in the router.  
IPv4 = w.x.y.z each can be a number of 0-255 = one unsigned byte  
IPv4 is basically 4 unsigned bytes, we turn to decimal for readability  
Router stores a forwarding table that is a map of IP prefixes to the output links  
This uses a longer prefix algorithm - tries to uses the longest version of the IP address it can to forward. If its not there, uses a smaller prefix  
If the table has:  
1.*  
1.2.*  
1.2.3.*  
Sending to 1.2.3.4 would use 1.2.3.* output port as it is the longest most specific one that matches the target IP 

x.y.192.0 is / 18 - there are 18 fixed bits with 14 free bits that can change

Switching Fabric handles the queueing. Factors in how long it takes on the output as well as how long a packet has been sitting in the input port queue  
Two input ports can also collide as they might need the same output port. It handles that  
The processor and fabric are highly optimized special purposed. They get very complicated

### IPv4
IPV4 is the major network layer protocol in use today. IPV6 is increasing its market share, but fairly slowly.  
An IP packet has a header with the following important fields:

* version (today, 4 or 6)
* packet length
* fragmentation info - designed so a large packet can be fragmented to send across link layer and pieced back together. This was bad and is dropped in IPv6
* time to live (number of hops before the packet must be dropped) this changes every hop, has to go down every hop
* protocol (TCP or UDP or some other options)
* header checksum (for error detection) this changes every hop, has to be recalculated based on the updated TTL
* source + destination IP addresses

The IP address on a large scale, is purchased. Request a chunk of IP addresses.  Say we need 10,000 addresses, IP prefix provides a chunk of free bits for that amount  
These addresses are purchased by the bit for free space  
For an individual address on any given device is a negotiation with the router.  This uses the DHCP

### Dynamic Host Configuration Protocol
A client doesn’t have an IP address. The client can’t know where to send things, or how to set a return address.  
There is a special message that is a broadcast   
The client sends a message to 255.255.255.255 looking to find a DHCP device.  This touches everyone in the subnet. 
The DHCP sends a broadcast back to all machines in the subnet with an open IP address it has. (These have a “lease” time to live). 
The client responds saying “I would like this IP address” and the DHCP sends back an ACK

There are 4 billion IPv4 addresses. There are more than 4 billion devices connected at any given time.  
Two solutions:  
**Network Address Translation (NAT)** - sharing addresses  
**IPv6** - More address (enough for like every atom to have a unique address)