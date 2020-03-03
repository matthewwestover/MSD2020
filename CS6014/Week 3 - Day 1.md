## Week 3 - Day 1
### Transport Layer Congestion Control
The sliding buffer window should hopefully be at the same size as the receiving buffer.  
However the network connection itself might not be able to handle that many packets in flight at once.  
Network connection can have a lot of packets from different connections going through it. It has a max speed/capacity  
This can lead to network “congestion” on the transport layer  

High congestion should shrink the size of the window of the sending buffer
Three phases of congestion control exist these change based on the cwindow (congestion window)  

Overall size of the sliding window is min(rwindow, cwindow) - which ever is the limiting factor for how big/many packets can be in flight at once.  
Bigger sliding window = more data for faster completion  
MSS = Maximum segment size - as big of a packet as possible

### Slow Start
Start small and grow the size of the window as rapidly as possible. Assume network sucks and grow from there. Starts by sending a single packet of the maximum size. Cwindow = 1 mss; This grows very fast in that for every ACK it increases by mss. So 1 mss in flight becomes 2 mss in flight becomes 4 mss inflight as the ACKs come back in to the sender. This is exponential  
Because this grows so fast, it would quickly overload the network.

### Congestion Avoidance
Once we think we are nearing the capacity of the network (cwindow > slow start threshold) this growth becomes linear. Instead of adding MSS in size on receiving an ACK, the sender starts sending a fraction of the MSS to become a linear growth.

When we start losing packets we know we are around the max limit for the network.  
Duplicate ACKs (3 of same packet ACK responses) -  fast recovery mode  
Timeout of request for an ACK response - goes back to slow start (1 MSS packet) and ramps up with a smaller slow start threshold (in half) 

### Fast Recovery
Set the cwindow size back to where we hit the slow start threshold. Return to the linear growth amount. 

For TCP the slow start threshold is 64 kb

Additive increase multiplicative decrease.  
The graph of this starts exponential, turns to linear. Becomes a saw tooth shape as fast recovery occurs, drops and grows linear. If we time out it goes back to the start. 

The upper limit on these with multiple connections on the same network is they all tend to even out at the same high point  
On average this hits about 75% of the max rate of the network

TCP has been around for 50 years now. It is the backbone of the internet.   
It also has issues.  
Google decided to develop their own version called **QUIC** that is based on UDP

### QUIC
HTTP request, hand shake (3 way), encryption set up, actual website text data
But the sites needs images/videos/adds etc that have to be requested  
Each of these needs to have a new request, handshake, encryption set up, and the actual data  
This is a lot of requests. 

We ask for things in parallel - we have to limit TCP connections at once 6-10 give or take.  
This only helps slightly. Still a lot of back and forth  
Often it will keep a TCP connection open to handle multiple requests  
Still has issues  
Google developed SPDY (HTTP 2.0) that lets a bunch of things stream through the same connection at the same time, not just have multiple connections in parallel.

Google still isn’t happy with it, so they made QUIC (which is being standardized into HTTP 3.0)

Middle nodes look inside the TCP packets (even though they dont really have to)
Ossification - TCP has a lack of flexibility. The end points should be able to be updated and allow changes. But the middle nodes typically can’t be updated. Google estimates it would take up to 15 years to update all the middle nodes to change TCP. 

Google decided to build on UDP to create a new form of transport. UDP data is application layer data, the middle boxes dont look inside the packets. 

QUIC is a bit hacky implementation to fake a TCP type connection  
Handshake - once a connection has been made, it can just jump to the encryption in the subsequent connections. Handshake only needs to happen once  
TCP has no encryption because it wasn’t a necessary thing when it was developed
QUIC has encryption built in. Everything including data in header is encrypted.  Harder to snoop on endpoints, the middle boxes cannot modify or look at this data as the packet moves along the network.  
QUIC has streaming. Each independently handles ACKing etc. so one stream drops a packet, the other streams dont delay or slow down.  
Handles the "parking lot problem” - connection drops from wifi to mobile network. TCP drops because the IP and port changes.  
QUIC uses a user id as part of it, so even if the IP address changes, the connection can remain. This is part of the initial handshake

QUIC is used in chrome and google data servers  
Being standardized into HTTP 3.0  
It can update at server and client, but the network doesn’t need to know updates.