## Week 10 - Day 2
### Graphs
Graph Terms  
A graph G(V,E) consists of a set of vertices V (also called nodes) and a set of edges E (also called links) connecting these vertices.  
Graph and Network are often used interchangeably  
A simple graph G(V,E) is a graph which contains no multi-edges and no loops.  
A directed graph (digraph) is a graph that discerns between the edges A -> B, or A <- B  
A hypergraph is a graph with edges connecting any number of vertices.   
Unconnected graph: An edge traversal starting from a given vertex cannot reach any other vertex.  
Articulation point: Vertices, which if deleted from the graph, would break up thegraph in multiple sub-graphs.  
Biconnected graph: A graph without articulation points.  
Bipartite graph: The vertices can be partitioned in two independent sets.  

Tree: A graph with no cycles - or: A collection of nodes  
A tree contains a root node and 0-n subtrees subtrees are connected to root by an edge

Degree: Node degree deg(x)  
The number of edges being incident to this node. For directed graphs indeg/outdeg are considered separately.  
Degree is a measure of local importance 

Betweenness Centrality: a measure of how many shortest paths pass through a node. good measure for the overall relevance of a node in a graph

A Path is route along links  
Path length is the number of links contained  
Shortest paths connects nodes i and j with the smallest number of links.  
Diameter of graph G: The longest shortest path within G.

#### Graph and Tree Visualization
How to decide which representation to use for which type of graph in order to achieve which kind of goal?  
Two principal types of tasks: attribute-based (ABT) and topology-based (TBT) 

Localize – find a single or multiple nodes/edges that fulfill a given property  
ABT: Find the edge(s) with the maximum edge weight.  
TBT: Find all adjacent nodes of a given node.   

Quantify – count or estimate a numerical property of the graph.  
ABT: Give the number of all nodes.  
TBT: Give the degree of a node.   

Sort/Order – enumerate the nodes/edges according to a given criterion  
ABT: Sort all edges according to their weight.  
TBT: Traverse the graph starting from a given node.  

Graphs can be represented: Explicit (Node Based), Matrix, Implicit

Explicit Graph Representations  
Node-link diagrams: vertex = point, edge = line/arc

Criteria for Good Node-Link Layout:  
Minimized edge crossings  
Minimized distance of neighboring nodes  
Minimized drawing area  
Uniform edge length  
Minimized edge bends  
Maximized angular distance between different edges  
Aspect ratio about 1 (not too long and not too wide)  
Symmetry: similar graph structures should look similar  

Some of these conflict: Minimum number of edge crossing vs. Uniform edge length, Space utilization vs. Symmetry

Force Directed Layouts:  
Physics model:   edges = springs,
 vertices = repulsive magnets
Algorithm:
Place Vertices in random locations
While not equilibrium
calculate force on vertex 
sum of pairwise repulsion of all nodes + attraction between connected nodes 
move vertex by c * force on vertex

Properties:  
Generally good layout  
Uniform edge length  
Clusters commonly visible  
Not deterministic  
Computationally expensive: O(n3), n2 in every step, it takes about n cycles to reach equilibrium  
Limit (interactive): ~1000 nodes  
in practice: damping, center of gravity

#### Matrix Representations
Instead of node link diagram, use adjacency matrix
Well suited for   neighborhood-related TBTs. 
Not suited for    path-related TBTs  
They are Order Critical!

#### Implicit Representations
Pros:  
space-efficient because of the lack of explicitly drawn edges: scale well up to very large graphs   
in most cases well suited for ABTs on the node set  
depending on the spatial encoding also useful for TBTs  
Cons:  
can only represent trees  
since the node positions are used to represent edges, they can no longer be freely arranged (e.g., to reflect geographical positions)  
useless to pursue any task on the edges  
spatial relations such as overlap or inclusion lead to occlusion

#### Networks and Attributes  
Attributes can influence topology  
Path can be slow / blocked  
best route when driving depends on traffic biological network depends on many factors

Large number of values.  
Large datasets have more than 500 experiments  
Multiple groups/conditions Different types of data  
Two central tasks: A Explore topology of network & Explore the attributes of the nodes   (experimental data)  
Need to support both!
