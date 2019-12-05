## Week 5 - Day 2
### Priority Queues
Option 1: a linked list. 

* add: O(1)
* findMin: O(N)
* deleteMin: O(N)

Option 3: a self-balancing BST. 

* add: O(logN)
* findMin: O(logN)
* deleteMin: O(logN)

Option 2: a sorted linked list  

* add: O(N)
* findMin: O(1)
* deleteMin: O(1)

Option 4: a binary heap

* add: O(1)
* findMin: O(1)
* deleteMin: O(logN)

Given a set of items there are multiple ways to make a “heap”  
*Williams' Method* - Naive Solution  

```
For each item
    heap.add(item)
```

This requires 2x memory storage - the list of incoming items and the created heap

*Floyd’s Method* - Faster Solution  
This requires a contiguous array  
The array can then be considered as a complete tree  
That is all levels are filled save for the bottom, left to right  
This is one of the heap properties already satisfied  

Reinstate the heap order by percolating each node of the complete tree down to where it needs to go  
Basically a lot of swaps  
Leaf nodes cannot percolate downward  
Start at the last non-leaf node to swap  
The last leaf node is at index (size/2) - 1  
Compare node with child and if it is larger, swap it  
Then go to the next last item (current index -1) 

You have to move all the way down at each node. If a grandchild is smaller, swap with child, then swap with grandchild.

Creating a new heap add is O(1) on average, but in worst in O(logN) in worst case  
Williams’ adding N items is O(N log N) complexity  
Using Floyd’s method to build a heap inside the array is O(N) in worst case (as in every node has to move)  
This is cheaper to do memory wise and lower complexity  

To return a sorted array from heap, remove the root  
This is a O(N log N) complexity as deleting a min value is O(log N), we need to delete every item to have it sorted  

### Heapsort
Take an array, build it into a heap using Floyd’s method O(N) and then call deleteMin for everything  
Becomes O(N log N + N) = O(N log N)  

We need to account for removing in place without making a second array  
When we remove the root, add to the size+1 index value  
deleteMin takes the very last leaf node and points to null. We know it is free at that point  
Take the root and place in that leaf node.  
The heap then is 1 node smaller for when you percolate that node back down.  
Call delete min again on the smaller heap section, and place the new root into the next null point  
This creates a reverse sorted array - largest to smallest   
To get a sorted array from smallest to largest - use a maxHeap instead of a minHeap (root is largest value)  

Worst case Heapsort is O(N log N)  
It is done in place without creating a secondary array for storage  

Mergesort is O(N log N) but requires 2x memory  
Quicksort can hit O(N^2) in worst cases.  

Heapsort avoids both of the downsides of those two sorting algorithms  
In practice Heapsort has a higher constant than Quicksort  
Quick sort also rarely hits its worst case unless the pivot selection is REALLY bad

### Balanced Trees
Perfect Balance every node either has 0 or 2 children. All leaf nodes are on the same level. Last level is completely full  
Good Balance - the depth between any pair of leaf nodes  should differ by no more than 1 - This can be defined depending how you need it it to be. A depth of 1,000,000 a difference of 10 might be okay for balance  
That is all leaf nodes are within 1 level to all other leaf nodes  
A tree that only goes in one direction isn’t balanced even though the single “leaf node” has no height difference. There is no sibling to compare to  
This is because the other direction node on each parent node is implicit
Basically the empty node has a difference of -1

A self balancing BST is one that reorganizes itself on insertion/deletion   
Still needs to be a O(log N) complexity for these operations  
Balancing is not free so it might act as a constant factor for the O(log N) operations 

### AVL Tree
Named after Adelson-Velskii & Landis  
Tree rotations are done whenever the tree becomes unbalanced (leaf node depth difference > 1)  
Find the node that needs to pivot up.  
Take the node that is moving “up” and find its most left leaf node, it gets placed to the left of the node the node that moved down.  
Rotate right can occur if the left most branch becomes unbalanced.  
Swap the opposite way  
You can remove a node and attach at same level to opposite side once rotation of node is made  
These swaps are quite cheap in comparison to generating a whole new tree using Floyd’s method  
There can be double rotations as well  
You have to know the height of every node to know if a tree is balanced.  
These trees maintain the balance rule and rebalance is O(N log N) and guarantees tree height is O(log N)  
Java uses a red-black tree set  
Complexity of red-black and AVL are the same  
Runtime of insertion/deletion of red-black is slower than AVL  
Runtime of search of red-black is faster than AVL  

### Dijkstra’s Algorithm
Sometimes there is a cost with moving from node to node - traveling along an edge  
We can weigh edges to give a relative value of what that cost is between any two nodes  
Weighted path length is the sum of all edge weights on the path.   
This is not the same as the path length (how many nodes to get from A to B).  
A to B can be more expensive than A to C to B  
When weighted paths are around we want to minimized the weighted path length for efficiency  
To keep track of cost of an edge create an edge class  

```
class Node {
    E data;
    List<Edge> neighbors;
}
class Edge { 
    Node otherEnd; 
    double weight;
}
```

Nodes contain lists of edges it is connected to not the node it is directly connected to

Edsger W. Dijkstra - Dutch computer scientist (1930 - 2002)  
Made major contributions to parallel programing, structured programing, how to program well, software engineering  

He also is responsible for the algorithm for finding shortest (cheapest) paths in graphs  
Algorithm keeps track of the total path cost so far from start node to current node  
Cost of path to next node is total cost so far plus weight of edge to next node  
Instead of traversing nodes in order they are encountered, go in the route of the cheaper cost first  
This uses a priority queue to handle it  
Keep track of cost so far and cameFrom  
Only mark a node as visited when removed from the priority queue  
Starting node has a cost of 0.  
Each node tracks costSoFar for itself. So its value to get there from A.   
This means if a shorter path is found to get to a node, it’s costSoFar needs to be updated within the queue and its comeFrom variable needs to be updated as coming from the shortest path  
Nodes get updated in the priority queue  
If a node is reached in a more expensive path, it isn’t updated. Only the shortest one get placed. 

```
Dijkstra (Node start, Node goal) {
initialize all nodes’ costs to infinity 
start.cost = 0
PQ.enqueue(start)
while(!PQ.empty()) {
curr = PQ.dequeue()
if(curr == goal) {return} // done! curr.visited = true
foreach unvisited neighbor ‘n’ of curr:
    if(n.cost > curr.cost + edgeweight)
        PQ.enqueue(n) (or update position)
        n.cameFrom = curr
        n.cost = curr.cost + edgeweight`
```

This does not work with edges that have a negative weight associated with it  
Without using a priority queue this is O(V^2)  
With priority queue it is O((E+V) log V)  
