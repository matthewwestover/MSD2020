## Week 2 - Day 1
### Transport Layer
Transport Layer is between the Application Layer and the Network layer (helps the application layer, built off of the network)  
This is the step up from host to host across the whole network  
We can send packets to somewhere else.   
Transport layer provides process to process communication and reliable data transfer (**TCP**)  
Need to identify what process being used on a computer, uses the ports on top the IP address  
A chunk of data in this layer is a segment. It is possible for a segment to be many packets, but usually they are more or less the same size

Network layer is a best effort - no guarantee that packets will be delivered or in order  
Transport layer TCP has to add this guarantee   
TCP presents as a persistent connection across to send all packets  
UDP is independant packets not a persistent connection

To communicate specifically with the HTTPS server process on google.com, not its SSH server, for example. And when Google responds, their response needs to be sent to my browser, not my terminal.

**TCP**
It includes source and destination IP and source and destination socket. 
(SIP, DIP, SP, DP) and all four have to match for data to be sent through an open connection (socket)  
Socket/connection remembers for us

**UDP** 
No socket usage just read packet and send packet  
It is the end node’s responsibility on where to send replies to. As long as DP and DIP is correct, the end point can receive anything  
Packet here includes the data, the SIP and SP for where responses go.  
We have to make sure to include this data in the packets when sent

### Reliable Data Transfer
How to reliably guarantee data transfer as complete and in order on top of the best effort network layer  
Really what it guarantees is that what is received is the correct stuff, not that everything is received. 

**State Machine** - coffee maker  
Off -push on-> Brewing -Done Brewing (out of water > warming  
Brewing - push button off -> off  
Warming - push off or timer -> off. 

State machines are a good concept for visualizing how things work. What state can it be in and how do we get there.  
TCP can be seen as client state machine and host state machine: sender and receiver.  
App layer sends reliable to transport later, which sends unreliable on network.  
Reverse is true for receiving

Easy Method:

|  Sender |                    |                     |       Receiver      |
|:-------:|:------------------:|:-------------------:|:-------------------:|
| Waiting | - send_rdt(data)-> | - send_udt(data) -> |      Waiting ↓      |
| Waiting |  <---------------- |  <----------------- | Receive_udt(data) ↓ |
|         |                    |                     | Deliver_rdt(data) ↓ |
|         |                    |                     |       Waiting       |

#### Normal Method
To make this more reliable we can send a “check sum”  
A check sum is where data is ran through an algorithm to give a value, value is sent with rest of packet, and receiving end runs algorithm as well. If they dont match check sum values then it knows there was an error.

Once Receiver receives it has two options. Check is okay and check is not okay  
Receiver replies with if it was okay or not okay. Sender will send again if receives a not okay response  
This is the acknowledgement (ACK) or negative acknowledgement (NAK)  
NAK triggers resending the data  
Data is sent one way, but packets are sent two ways as sender has to receive the acknowledgement

If the ACK or NAK gets corrupted it gets more complicated. 

#### Heroic Method
Data is given a “sequence” number. Receiving side keeps track of if it has been received or not.  
Sender receives a corrupted ACK it resends data. If receiver did successfully receive previously it will submit a new ACK on second receipt but not pass on to application  
The main thing that the sender wants is an ACK specific to the packet it sent.   
The receiver now only sends back ACKs for the last packet it received. The “everything” else portion of the sender loops back on the packets that need to get resent.  
This is slow. Every single packet has to wait for an ACK  
This also ignores packets that are dropped.  
Sender has to care about “time” when waiting for an ACK. Can’t wait forever for a response. 