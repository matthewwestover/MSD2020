## Week 4 - Day 2
### Network Control Plane
There are a lot of central nodes in the network  
There is no overall control point.  
Comcast, CenturyLink, etc connect to the backbone.  
Backbone nodes communicate with each other, but have different owners and agreements to allow traffic to flow through  
All controlled by different people so someone at one node can’t dictate what another node does  
Management is independent but have to work together. 

Each collection of owned nodes are autonomous systems (AS).

### IntraNode Communication
Within a set of nodes all owned/managed by the same group - within an AS
Network can been seen as a “graph”  
We can use Dijkstra’s Algorithm to find the shortest route through the nodes from point A to point Z  
This is the “Link State” Algorithm in networks as we need to know data about all the nodes and weights in the network.  
Knowing this info is the “state” of the network

There is also the Bellman Ford Equation which can be made into an algorithm to find the shortest route
```
dist(x, y) = min_{n in neighbors}( cost(x, neighbor) + dist(neighbor, y)}
```

With this each node keeps tracks of a distance table. It knows the distance from itself to a neighbor.  
This table is shared with its neighbors.  
When the table is updated, this update is shared. The neighbors can then update their own table based on the received table from each of its own neighbors  

```
When my distance table changes
    Tell my neighbors
When I hear from neighbors
    Recompute distance table. 
```

To start, tell neighbors about our neighbors  
Lots of back and forth, but by the end, using local communication all nodes can know about how to get through in the quickest fashion  
This happens before we send any packet. This is a preprocess that updates independently of any packets of data being sent  
When this is done, every nodes knows distances inside the network. It can update periodically and always knows the shortest route to get from A to Z  
There are issues, like if a node breaks the tables don’t automatically update. 
This is also decentralized as each node fills in its own distance table. It is a historic method to this  
Modern networks have a central control point that calculates these things and sends it out to all the nodes in its network space

### InterNode Communication
Communication between different AS locations we can’t just store every individual connection in a table  
Uses prefixes to keep track of what the AS can communicate with directly, and the costs to get there.  
Border/Gateway routers at the edges of the AS have a persistent connection with each other  
They keep track of this information.  
They use their own special protocol to do this  
**“Border Gateway Protocol”** (BGP).  
This is how the border nodes in each AS know which prefix it can get to.   
Border nodes talk IntraNode so things within its AS know which border node to direct to.  
This creates a eBGP - external communication and iBGP - internal communication

BGP messages are “advertisements” that say “I can access this and here are the costs”   
None backbone AS sections don’t broadcast all of their connections to other backbones. They only show their endpoints.  
BGP can be selective in this manner so they don’t have a backbone pass through it to another backbone  

One common thing is the “hot potato” algorithm. Get rid of a packet out of your AS as fast as possible.   
AS need to balance giving fast connections, not overloading equipment, not wanting to pay a lot of money to other ASes to pass traffic.  
Backbone to Backbone has peer agreements to minimize the costs of these, most assume that same amount of data being sent is the same being received through. 

### IP Anycast
Using BGP it is possible to assign the same IP address to multiple nodes. This lets them advertise the shortest route to one of these nodes with that IP address. This gets traffic to go to the closest server possible. This is great for things like TCP and DNS. UDP doesn’t work well as if the device changes the new node would get confused. 