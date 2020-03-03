## Week 2 - Day 2
### Reliable Data Transfer
Prior discussion on reliable data transfer involves a lot of wait time for individual packets and waiting for ACKs to ensure it was received. We have to find a way to set a timeout wait time as well. 

This is analogous to having 4 loads of laundry and washing ever load, then drying every load. A better system is to pipe line - when one is done washing, put it in the dryer and start the next washing load while that is drying

In practice for network. As the sender you have to keep track of all the stuff you send and are waiting for ACKs.  
We need to set a limit for how many “in flight” packets there are with pending ACKs  
We also need to deal with corrupted data, or lost data packets  
TCP uses a combination of two methods  
**Go-back-n method**: the receiver sends back an ACK with the last highest - in order - packet received.  
Receiver gets 1,2,3,5 it sends back 3. This can lead to having to resubmit everything as if the first packet is corrupted or lost, it doesn’t send back any highest in order ACK   
**Selective Repeat**:  involves both sender and receiver keeping an array of data for the amount to be sent/received. Receiver ACKs every received packet, and sends it back, sender tracks the good ACKS.  
This is generally a smaller set then the full data. As the beginning point of the data transfer fill in with ACKs, the sender shifts up to send the later data packets

### TCP
TCP is a reliable connection-oriented protocol. It has flow and congestion control.  
It is a point to point protocol. It can only send a message to one person. UDP can do multi-cast where one packet is sent to a bunch of places.  
TCP Segment is a header (small metadata) + payload of data  
This is sent on the network layer, this gets placed into a network packet with an IP header. Normally speaking network layer shouldn’t look at the TCP segment, but it does peek in for sending

#### TCP Header
Adds a source and destination port # - both 16 bit ints - this identifies the programs on the two ends  
Low # ports (under 1024) need admin access. This is why we normally use 8080, etc for testing and development.  
Header also contains a sequence # and ACK #  
Sequence for the data they are sending, and ACK # they have received, because TCP is a send/receive 2 point communication. Stuff comes and goes on both ends. Header has what they’ve sent, and what they’ve received. Both ends need to keep track of this  
Flags  
Receive Window - how much space they have available for data to receive

##### Receive Window
TCP has array buffers for send/ receive. Send uses syscall write, receive uses syscall read to copy data to disk  
Receive window states how much room is left in the receive buffer. The bigger/more space in the buffer the bigger the window is, the more data that can be sent by the sender. This prevents the sender from sending too much data that overfills the receive buffer

##### Flags
TCP is a relationship connection - uses a handshake to establish the connection was successful.  
TCP starts with a three-way handshake. These dictate the flags portion of the the TCP header  
Client sends first packet. It includes the syn flag  
The server response with a syn-ack to confirm it received and can understand TCP
The client then sends a response with an ack which can contain the first bit of data if the client wishes.  
Once this ack is received by server, both sides can communicate back and forth with data

At the end session, we want to close gracefully. Either side can end the connection. This sends a fin flag. The receiver of the fin sends an ack in response

The closing FIN / ACK handshake can be difficult. What if an ACK is lost, does the side that sent the FIN care? Most keep the connection open ~30 seconds to wait for the ACK response. 