## Week 4 - Day 3
### Link Layer
Link layer is below Network Layer and above the physical layer  
Transmits “frames” of data between two nodes  
A frame is composed of a Transport layer “segment” wrapped in a Network layer “packet” wrapped in a Link layer “frame”

This is a single hop communication, get from here to the next point, not the end goal  
Routing is less important, just focused on getting to the next node  
Tends to be transparent to end users. Network is usually the lowest level a user goes/cares about.  
While application layer = user level, transport = kernel/os, network = low-level software/hardware, link layer is mainly all hardware level

```[Frame [Packet [Segment [Application Data]]]]```

There are two types of link layer communication:

* Easy - Point to Point (ethernet)
* Hard - Broadcast out (wifi)

Hardware (network card on motherboard, etc) creates the frame when sent  and removes the frame when received 

### Error Detection
Link layer often provides error detection, this prevents waiting for end to end error verification (ack etc), since it is just one hop  
This can make it faster to correct and have data sent on  
The smallest amount of info possible to add to a message is a single bit  
To make sure no other bits are flipped or dropped predetermine if you want an even or odd amount of 1s in the message,  
fill in the single bit based on the message being sent.  
```100110… 01X```  
X is the new bit that makes it so total 1s in message is even.  
This new bit is the parity bit  
This tracks an odd number of flipped bits.  

Alternative is clever - break data into a grid - this is a length x height chunk of data (16 = 4x4)  
Add a parity bit for every row, and every column  

```
0110|0
1010|0
0101|0
0111|1
----
1110
```
Get a flipped bit this becomes 

```
0110|0
1010|0
0111|0 -> 1
0111|1
----
1110 -> 1100
```

This lets us know what row and what column the flipped bit was located  
This means the error can be corrected at each node without being retransmitted from origin

### Broadcast Links
Point to point is simple, multidirectional, one start to one end  
Multiple points talking to a single point (like using wifi)  
Need to account for “collisions” two nodes talking to one at the same time so neither can be understood.  
Example for simple solution, two radio signals on different frequencies don’t conflict  
But we run out of unique physical mediums for this to work. We need to have overlap on same broadcast link without colliding

Things we would like in this: 

* Max speed for transmission for 1 user
* Fair sharing speed between N users: each gets ~1/N of bandwidth

### ALOHA
There are two versions: slotted and unslotted

Slotted: 

* Break up all time into sections. Sync clocks between all users that broadcast out( when segments start and how long they are) and detect if there was a collision
* Messages are sent in a time segment
* A user sends a message, if there is no collision - great
* If there is a collision at least one sender has to wait to send again. 
* While the collision happens, “flip a coin” and the sender either waits, or sends again immediately. 
* Everyone flips their own coin, does not know what the other side does. 
* This doesn’t work out super well, ~37% of the time there will be one node sending and ~37% where no one is sending and ~26% time slices with collisions. 

Unslotted:

* Same as above but no sync of time segments. Each node decides its own time frames based on how long it takes to send a frame of data.
* This is about half as efficient as the slotted ALOHA

### CSMA
Carrier Sense Multiple Access  
This is an improved ALOHA  
Instead of wasting a full time slot with transmitting over a collision if a collision is detected, stop immediately  
When starting to send, only send data when no one else is sending data.  
When to start back up after a collision, to avoid recolliding choose a random delay time. The more repeated collisions the longer the delay becomes.  
Ethernet/cable this delay is between {0 - 2^#collisions} + time to send size of bytes  
The better nodes are at detecting collisions the closer to a ~1/N share of bandwidth is reached

### Taking Turns
Nodes explicitly communicate and agree to a schedule to send data.  
Difficult to not schedule nodes with no data to send  
Nodes can crash, if the node that is the “scheduler” crashes it, all nodes cannot transmit  
Nodes join and leave network so schedule has to be updated. 

### Case Study: Cable Internet
Cable networks are odd, due to historically being a TV communication system originally.   
Everyone on network gets all data.  
When downloading is good because communication is one direction, from ISP to a end point, this is why download speed tends to be faster than upload  
There is no coordination needed for downloads.  
Uploading is a good implementation for taking turns. The ISP can be the scheduler, as if it is down nothing could be sent anyway.  
There is a “bidding” phase where all uploads tell the ISP it wants to upload.  There are upload slots, and each random node picks a random upload slot. If no collisions, sends the data back down the line to the ISP.  
ISP sends back a schedule based on requests received.  
If a node requests a slot, but doesn’t get the schedule back, there was a collision on the random slot picked. So node needs to start from start with randomly selecting one of the random slots  
This is **DOCSIS** format