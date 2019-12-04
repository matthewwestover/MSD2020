## Week 4 - Day 1
### Graphs
Trees are a subset of Graphs and uses its own terminology  
Nodes in a Graph can have multiple parents vs only one  
Links can go between nodes (bi-directional)  
They in general don't get noted as parent/child relationships  
They are “vertices” the node and “edges” the connections  
Graphs are VERY common in CS  

Graph (V, E) - V is a set of vertices, E is a set of Edges  
Edge(v1,v2) - these two vertices are connected. We don’t know what direction based on this  
Edges can be objects not just pointers, they can contain data themselves  
Airport example - flight time and distance between airports. This data wouldn’t be stored by any individual airport. Depends on the edge between them  

Directed Graph - **Digraph**  
Edges are directed - indicated with arrows  
Edges tend to get labeled with numbers - called Weights  
These are the “costs” of traveling from vertices  
It can be cheaper to visit another vertex before heading to a destination vs directly going there  

**Cycle** - a path from a node to return to itself - this cannot occur in a tree  
**DAG** - *Directed Acyclic Graph* - important subclass of a graph. Guarantees there are no cycles found in the graph

Graphs have no root nodes  
Just a collection of vertices and edges  
To represent this in code the graph must contain a list of all nodes/vertices  

```
class Graph<E> {
  Set<Node> vertices;
  // various graph methods
}
class Node {
  E data;
  Set<Node> neighbors; // what it connects to
}
```

The order of neighbors is unspecified  
Neighbors are all nodes that the current node points directly at  
A different order of neighbors is the same graph but can return different results  
A path is a sequence of vertices you travel through to get from a start point to an end point  

This is how GoogleMaps routes things. Takes nodes of intersections, weights travel time between them it calculates the fastest route to get from point A to point B

There can be more than one path between nodes  
We are often interested in path length, and more interested in finding the shortest path possible  
Depth First - DFS  
Breadth First - BFS 

These algorithms guarantee that if there is a path from A to B, it will return it.  
This does not mean it will return the same path between the two types  

```
DFS(Node curr) {
  print(curr);
  for(Node next : curr.neighbors){ 
    DFS(next);
  } 
}
```

This prints every possible node reachable from the starting point. This is incomplete however  
Graphs can have cycles - repeat through itself over and over.  
To do this, flag a node as visited.  

Full code:

```
DFS(Node curr) {
    print(curr);
    curr.visited = true;
    for(Node next : curr.neighbors) {
        if(!next.visited) {
            DFS(next);
        }
    }
}
```

This looks at the first edge coming out of the first node  
It recursively searches for the next node  
Upon returning, take the next edge  
If no edges exist - return  
When visiting a node, mark it as as visited so it doesn’t get called again  
For each node we visit, save a reference to the node we came from so we can reconstruct the path there  
To save out the path  

```
DFS(Node curr, Node goal) {
    curr.visited = true
    if(curr.equals(goal)) 
        return
    for(Node next : curr.neighbors) { 
        if(!next.visited) {
            next.cameFrom = curr
            DFS(next) 
        }
    }
}
// Path is now saved in nodes’ .cameFrom
```

To get the path, call each nodes .cameFrom. as you call it, you get the nodes in reverse order back to the origin point  
DFS guarantees finding a path between nodes if they connect  
It can find all the possible connections between start/end  
You need to remember to unflag visited history to see all the possible outputs  
This can get complicated.  
We can’t predict which path it will do off hand.  
It depends on the order of neighbors stored in each node.  

To find the **shortest** path DFS is not the best. It finds A path, and with some tweaks it can return ALL possible paths.  
BFS can find the shortest path. Instead of going down to a node’s neighbor’s neighbor, visit all neighbors first.  
You use a queue to do this search  
Queue the start, Queue the neighbors  
When you dequeue a node, enqueue that nodes neighbors  

**BFS** is **NOT** recursive - the only reason not to use queues and make it recursive is if the system doesn’t allow queues.  

Start at a, mark it as visited. Add its neighbors. Those neighbors, update their cameFrom and visited markers  
This visits nodes closest to start point first  
This finds the shortest route first, as the end goal cameFrom will link through to the start point

```
BFS(Node start, Node goal) {
    start.visited = true 
    Q.enqueue(start) 
    while(!Q.empty()) {
        Node curr = Q.dequeue() 
        if(curr.equals(goal))
           return;
        for(Node n : curr.neighbors) {
            if(!n.visited) 
                n.visited = true 
                n.cameFrom = curr 
                Q.enqueue(n)
        }   
    } 
}
```

Visited flag is important for both DFS and BFS as Graphs can cycle  
Flagging prevents endless loops.  

When edges have weight, you have to do something special to find the “shortest” path. Shortest becomes least “expensive”