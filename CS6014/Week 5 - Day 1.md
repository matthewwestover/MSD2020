## Week 5 - Day 1
### Link Layer
Between Network and physical. It has different interfaces between the two of them.  

```[Ethernet/Wifi Header [IP Header[UDP Header[DNSMessage]]]]```  

UDP has port info for which programs are sending/receiving  
IP has address for the specific machines  
Ethernet/Wifi is just a node to node, not end to end. But it still requires some info about the two nodes other either side

These are called MAC Addresses aka a Physical Address  
MAC addresses are 6 bytes long and mostly static/permanent ~10,000 devices per person on earth. More than IPv4 not as much as IPv6  
They are part of the hardware itself, can’t have similar addresses close to each other  
They are assigned by manufacturers - they are assigned from a chunk. There is no routing/hierarchy/organization  
Can be thought of as a hashcode identifier  
Usually written as a hex code  
They can be spoofed. Android connected to untrusted networks assigns a fake address to prevent tracking  
Need end point address for header, don’t have enough information to get there. 
Like a DNS request - host name not an IP address  
In this case we have an IP address and need the MAC address

This uses ARP - **Address Resolution Protocol**  
Broadcast type message  
ARP Query = “Who has this IP?”  
ARP Response = “IP belongs to MAC”  
Before a packet is sent, ARP requests are get sent out around network to know where to send

Header has in addition to sending/receiving MAC addresses + CRC for data corruption it also includes info for physical link  
Header start with the preamble, alternating 1s and 0s: 
```0101010101010101```  
This is a square wave - clock signal  
The timing of the 1s and 0s allows for syncing with the physical layer. Analog to digital connection to start off 

### LAN - Local Area Networks
Using local connections between multiple hosts on a link layer level  
Level lower than network/router connections.  
These connections are switches and virtually invisible. It just automatically happens in connection

A switch is the central link layer connection point between all hosts on the LAN. It stores MAC addresses, and uses ARP to establish the table but can then quickly communicate. 

MAC address of all 1s is a broadcast message. When received by a switch, it gets sent to all nodes connected to it except the one that sent it. 

Switch learns about what MAC addresses are which as messages come in. The SRC address gets stored, and the message then broadcasts out to everyone. When a destination is found in the switch table, it can send it without broadcasting again. In two packets worth of data the switch can learn where to go, and the two direct communication nodes don’t even really know the switch is there. 

ARP table does need to update based on IP changes, even though MAC addresses don’t change.

Because the switch doesn’t initially know the right end address, every device hears the initial message broadcast.  
Switch poisoning happens when hosts try to add invalid entries to the switch ARP table  
Can have multiple connections, but no loops. If there is a loop with multiple switches it would broadcast to each other multiple times. 

### Switches vs Routers
Switches have no set up, they just are. However more connections added the more ARP traffic generated.  
ARP queries go to everyone, tables would be huge, there is no hierarchy to quickly sort where things go.  
Cannot have loops, so set up has to be more careful.  
Throughput is higher in a switch as they only look at the link layer, not the network layer. Switches getting two packets at the same time going to different places can process both at the same time.  
Switches work well for small subnets that connect to routers to other subnets

### VLANs - Virtual LANs
If you don’t want the LAN layout to be dictated by the physical connections you can fake it.  
Two separate groups close to each other, connected via the same switch. Adding bits to the header, the switch can say things get sent to group A vs group B even though they are connected.  
Switches separated gateways (the internet) can be virtually linked by sending data over IP, unwrapping it then do the switch on either side of the physically separated switches 