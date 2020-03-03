## Week 2 - Day 3
### TCP Continued
TCP is initialized by the client  
Client sends a SYN flag, server sends back a SYNACK, client sends back an ACK  
This handshake is for verification that both sides of the communication understand the TCP protocol  
This also initializes some state variables  
The SYN message sends a random sequence number. The server responds in the SYNACK that has a sequence number that relates to the random number  
This number is the SEQ# + 1  
The server also picks a random a SEQ#. The client to server ACK should also contain a response that is SEQ# + 1

This random SEQ# from both sides makes it so both sides can verify/keep track of each connection. This allows the same client to have multiple TCP connections to the same server at once

So even if there isn’t data inside the message we have a SEQ# and a ACK#  
ACK# is always the next byte we expect to see  
SEQ# of the first byte in our packet  

Same ACK# can be seen where data has not been seen to update its number  
The same SEQ# can be seen where it is sending the same data over and over

These are inverse based on what side is sending/receiving the constant stream but not sending/receiving from the other side. 

TCP keeps track of the first byte that has not been ACKed as received by the other side. It also keeps track of a limit of how many packets can be “in flight” at a time  
This limit is based on flow control and congestion control. This prevents the other side from being overwhelmed with data, and prevents the network itself from being overloaded with data

There is a send buffer and receive buffer, we write and read to/from memory based on this.  
The TCP packet includes info on how many bytes are free in the receive buffer. 

TCP Header includes the Sequence#, Ack#, Port #s, and the receive window (buffer freeness)  
So if receiver is receiving faster than it pulls from the receive buffer, the receive window response to the sender gets smaller and smaller

As the ACK# increases, the send buffer can move forward across the full data of what should be sent (sliding window of the data stream)

It is possible to have a sliding window of 0. You can have positive ACKs that move the starting byte forward, but the receive window gets smaller to size 0.  That means the receiver isn’t reading from the receive buffer  
If it is at 0, we never would get a response back after that saying the receive buffer has emptied.  
The sender sends 1 byte sized packets until the receiver sends an ack with a open receive buffer

If things are received out of order, the receive buffer will stick the packet data in the right place, but ACK the highest in order. If this fills up through the out of order one, it can jump the ACK# to after it

TCP can detect dropped packets before a timeout is reached.  
As packets after the dropped one are received, ACKS are sent back repeating the same ACK position.   
Repeated ACKs means a packet was lost, the sender sees this it triggers a resend for what packet should be after the ACK#  
This is **fast retransmit**.  
3 ACKs with same # trigger it

#### Delayed ACK
Rather than response immediately with an ACK, the receiver processes slightly then sends an ACK. This lets it include data in the pack back with the ACK - up to 500 msec  
If the receiver gets a second packet during this delay, it immediately sends the first ones   
If the data received is out of order, it also triggers the ACK immediately

#### Timeout
Timing out has to be careful. To small a window, it will time out every time. Too long and it slows it down a lot  
This time out changes as packets are sent.  
This is the round trip time + 4 (standard deviate of the round trip time)  
RTT has to be measured with a moving average  
RTT average = (1-1/8)currentRTTaverage + 1/8RTTcurrent packet  

This means old packets drop to 0 exponentially fast. This keeps the average with more weight on the most recent measurements. 