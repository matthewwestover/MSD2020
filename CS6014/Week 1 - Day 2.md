## Week 1 - Day 2
### Network Delays
There are incoming and Output Queues. There is a process switching in between the two.  
Queues store the packets that are transmitted  
This is the typical design, a “store and forward” 

Delays in network communication are important to understand for designing networks, debugging network issues, or write programs that require fast communication.

Packets have no fixed size, must contain the header info, but can have no actual data. Just like mailing an empty envelope 

### Processing Delays
Delays caused by the device deciding what to do with the packets. This is usually in microseconds. These are specialized device to device. This involves looking at the packet header and possibly looking up data in table

### Propagation Delays
Delays caused by physics. Distance it has to travel and the speed limit (typical speed of light) data can transfer  
This does not depend on the size of the data, it is literally just the physical time it takes for the data to move from point A to point B.

### Transmission Delays
The time it takes for the packet to go from the memory to the physical medium that travels. Data to electrons, fiber optics, radio waves, etc.  
Bits literally converted to these different forms  
This is what is typically assigned to modems. High speed 1 GB/s. Cabled Modem 100 MB/s.  
Packet size and transmission rate becomes size over rate. 

### Queuing Delays
Time packets are stuck in the queue. Received faster than can send, etc.  
Delay depends on the amount of stuff that is going through it  
The less things in the queue the faster it goes.  
Traffic in Queues typically goes in bursts. Lots of data at once that lowers as it is sent. 

Packets of the same size to the same place, most of these delays are constant, except for the queuing delays.  
Propagation delays can change in practice as they can take different routes between packet to packet. 

### Measuring Delays
We can “easily" measure end to end (round trip).  
Pinging lets us send a small packet that forces the end node to send a “pong” back for a response.  
Ping lets you set a packet size and receive same size back.  
We can measure time to send and get back this way  
This includes time for the end point to process the ping to send back. Its not an instant response. 

None of this lets us know what is happening in the middle.  
There is no guarantee that there and back take the same route  
We have to take it as a reasonable assumption that there and back are roughly the same

Average time for delay is number of nodes the message goes along times each of the delay types above.  
```Nodes * (dProcessing + dPropagation + dTransmission + dQueueing)```

We can estimate the propagation delay as distance/speed of light  
This should be small as speed of light is fast

Queuing Delays can be estimated by repeating the request many times. Most other delays are constant. Any variation in time would come from the Queuing Delays. 

Assume the fastest time done is the one with no queuing delay. Subtract this amount from the total on all others and averaging lets you find just the queuing delay.  
More numbers that are constant minimum times is a good indicator it is the true no queue delay. 

If delays are mainly transmission, get a faster device.  
If delays are mainly queuing, get more paths/devices to spread out the packets

A lot of the time we just need more information than a end to end:
Use ```traceroute```

This command asks every node along its path to give data. It sends packets with time to live to get this info, with longer times to live. When the time it is up, the packet is dropped and returned back. A lot of the time routers block this request and send nothing back. It by default tries each time to live request 3 times for more clear results. 

### Throughput
The measurement of bits/second that information is transmitted between ends/hosts.  
Latency can impact things like gaming time, we need data quickly. Same for Phonecalls/Video calls  
Lower latency is okay for things like streaming videos and we care more about throughput, a constant set of data once it starts arriving

Devices have different throughput capacities. It is usually limited by the transmission rate.  
The practical throughput is normally affected by the amount of people trying to use it.  
Usually there ends up being a bottleneck node, while others can handle more data.  
This historically was the home modem that had a lower transmission rate. 
