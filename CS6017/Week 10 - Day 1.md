## Week 10 - Day 1
### Graph Analysis
The existing techniques we've covered have treated data points as (mostly) independent.  
Pretty much all of our data can be represented in a tabular data frame with rows that can be examined in isolation. 
For graph data, the relationship between data points is the most (or at least AN) interesting aspect of the data  
How can we quantify and visualize (more on that tomorrow) these relationships in our data?

#### Example graph datasets
Web pages (nodes) with links (edges) between them  
Alternatively Web pages are trees with nodes (dom elements) in hierarchical relationships  
Transactions between businesses/countries (finance, trade, network, etc) Travel routes (roads or plane routes)  
Hierarchical relationships/containment (file systems, federal/state/local governments) -- generally trees, not arbitrary graphs  
Note, nodes (and often edges) have data associated with them, which might be stored as something like a data frame, so we may still want to use some of the techniques we've discussed previously even on graph data

#### Connected Components
A connected component is a set of nodes which are "connected" by edges. You can reach any node in a CC from any other node in a CC by following edges in the graph  
CCs are basically "islands" in the graph. To get from one to another, you need a plane or boat (ie, there are no edges between islands)  
For a directed graph, it's possible to have connected components where you actually can't get from every node to every other in a CC because the edges are one-way  
A "strongly connected component" in a directional graph is a set of nodes that are reachable from each other by following edges in the right direction  
There is a super cool data structure for computing connected components called a "union find" or "disjoint set" data structure

#### Super CCs: Cliques
A Clique is a "fully connected component"  
A subset of the graph where each node in the clique has an edge to every other node in the clique  
Cliques are parts of the graph that are as connected as possible  
Finding large cliques in large graphs is generally computationally infeasible (it's an NP complete problem)

#### Degree
The degree of a node is the number of edges it has  
In a directional graph, we have the 2 "degrees:" the "indegree" (# of edges pointing to this node) and "outdegree" (# of edges pointing away)  
To understand the graph as whole, we might look at min, max, median, average degrees across all nodes  
A graph with a high median node degree is "more connected" than one with low median degree  
For more context, we might plot a histogram of node degrees

#### "Betweenness Centrality”
BC is another way of measuring the importance/connectedness of a single node It's somewhat expensive to compute...  
We first compute the all-pairs shortest paths for the graph (the shortest path between each pair of nodes in the graph). 
The BC of a node is the fraction of shortest paths that include it  
A "bottleneck" node will have a high BC. A node on the periphery will have a low BC  
This is sort of coarse/sensitive measure... imagine 2 nodes who are at a bottleneck, but one paths can be slightly shorter by going through one rather than the other.  
There are other measures that we'll see which consider all the paths, rather than just the shortest

#### PageRank -- the google measure
PR is "the google" algorithm: http://ilpubs.stanford.edu:8090/422/1/1999-66.pdf  
It was developed for the directional graph of the web (if page A links to page B, there is an edge from A to B)  
Basically, PageRank imagines an infinitely patient websurfer who clicks random links forever  
PageRank measures the probability of the surfer ending up on each page after an infinite period of time  
Google used this idea to rank search results (pages that are linked to more often should end up higher on the list)

#### Searching
The measure in the previous slides were mostly about describing the entire graph, or one node in detail  
Searching is a more local operation. We want to find a path between two nodes  
We'll look at a few algorithms (which I think you saw in the fall) and a new one that's pretty cool

#### Depth First Search

```
dfs(G, target):
    mark all nodes as unvisited
    return dfsRecurse(G, some node in G, target)
dfsRecurse(G, n, target):
    if n is the target: return [target]
        mark n as visited
        for all neighbors of n:
            if neighbor is not discovered:
                path = dfsRecurse(G, neighbor, target) 
                if path: return [n] + path
        return []
```

Nodes to explore are pushed and popped from a stack (in the implementation on the last slide, we used recursive calls as our stack)  
We explore a path as far as we can, then backtrack and try another No guarantees that the path we find will be short  
Doesn't require the whole graph and memory usage is pretty low

#### Breadth First Search

```
Procedure BFS(G, root) is
    Let Q be a queue
    Label root as discovered
    Q.enqueue(root)
    While Q is not empty do
        v := Q.dequeue
        If v is the goal then
            Return v
        For all edges from v to w in G.adjacentedges(v) do
            If w is not labeled as discovered then
                Label w as discovered
                w.parent := v
                Q.enqueue(w)
```


Algorithm structure almost the same as DFS, but we use a Queue instead of a stack  
Guarantees we find the shortest path  
Works by finding the shortest path to all nodes closer to the source than the target  
Can think of it as a "floodfill" type algorithm

#### Dijkstra's Algorithm
Dijkstra's algorithm is basically BFS when we have edge weights of different cost (in other words, BFS is Dijkstra when all edge weights are the same)  
In BFS our Queue kept track of the "next closest" node which we would examine next  
With Dijkstra's algorithm, we need a Priority queue because we might find a path to a node that has more edges, but is shorter to a node than previously seen paths

What's wrong with BFS/Dijkstra  
BFS/Dijkstra expand outward from our source node in a circular shape But half of that circle is going the wrong direction (away from the target)  
As humans we can "see" that or "know that" (for small, low-D datasets), but how does an algorithm know?  
The key is to have a cheap way to estimate how far apart 2 nodes are  
Then, we can priorities nodes based on how far away we think they are from the target

#### A*
A* is the optimal search algorithm!  
We need a graph and a "heuristic" which is a function that estimates the distance between 2 nodes (there are some rules about h's behavior)  
No matter how smart/clever you are, if you have a graph and a heuristic function, your algorithm MUST look at as least as many nodes as A* to be sure that it found the shortest path between 2 nodes  
We often have lower bounds in algorithms that limit how well we can do. A* is nice because the lower bound comes with a good algorithm to actually achieve it!

A star tracks 2 things for each node  
g-cost: distance from start to n (this is computed as we explore nodes starting from the start)  
h-cost: heuristic estimate from n to target.  
Like Dijkstra, A* adds/removes elements from a priority queue to decide which nodes to explore next  
The PQ for A* is keyed on the "f-cost" which is g-cost + h-cost. In other words, pick the node that's closest to the target if the heuristic is accurate  
In order for this to work, the heuristic has to underestimate the true distance. For example, if we're searching for the fastest driving route, we could use the euclidean distance (as the crow flies) as a heuristic because we always have to drive at LEAST that far if we stay on roads and don't cut through buildings...  
The closer H is to the true distance, the better we'll do