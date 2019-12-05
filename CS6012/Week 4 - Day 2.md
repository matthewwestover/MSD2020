## Week 4 - Day 2
### Graphs 
Acyclic graphs - graphs where you can't cycle back to a previous node  
If there is a path from vertex A to vertex B, then A appears before B in the sorted area  
This is useful for scheduling tasks. If task A has to be completed before task B, then A has an edge pointing to B  
The **indegree** of a vertex is the number of edges it has incoming  
This can be saved as part of the Node class and can easily be computer as the graph is constructed  
Any time a node adds another node as a neighbor, increase the neighbors indegree.  

### Topological Sort
They are unique as order can come out different between two nodes with indegree of 0. (Ie Math 2250 and CS 1410 can be added in either order). Output is still correct for the top down ordering.  
1. Step through each node and make sure they all have indegree  
2. If a node has an indegree of 0, add it to the queue  
This means nothing precedes these nodes  
That is there is no way to get to that node, it needs to start at that location  
3. While Q is not empty
Remove first item and add it to a list for output  
Visit that dequeued node’s neighbors and reduce their indegree value by 1  
If a neighbor has a new indegree of 0, add it to the queue  
This basically is a dependency counter  
As prerequisites to reach nodes are completed they can get added to be completed  

Topological Sort complexity is O(|V|) - the absolute value of all vertices   
Often also +|E| which is absolute value of all edges  
**O(|V|+|E|)**  
This is also true of DFS and BFS  
We are not ever dividing the datasets in these algorithms, this prevents log based behavior  

### Assignment 7 - Shortest route through a maze
This is 100% pathfinding through the maze  
This uses breadth first searching  
The issue is there are walls that prevent movement  
We can only move up/down, left/right. No diagonal movement  

Think of the map as every empty location is a node  
There are edges between two nodes if there is not a wall between them. - Edges are connections to other nodes  
When reading in the input file, create a graph that is a 2d rep of the paths between the nodes  
Breadth First Search expands equally in all directions   
When you reach the end, you are done  
Every node needs to use the .cameFrom pointers  
Store node as a 2d Array  

```
Node nodes[][];  
Nodes = new Node[Height][Width]
```

Node class doesn’t need list of edges  
Neighbors are implied (up down left right)  
Upper neighbor is ```node[N.row-1][N.col]```    
If a node is “null” that means that it is a wall  
While reading input file   

```
If character is X nodes[I][j] = null
Else nodes[I][j] = new Node(..)
```

You have to handle start and end nodes