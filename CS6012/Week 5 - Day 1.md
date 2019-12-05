## Week 5 - Day 1
### Priority Queues

A data structure in which access is limited to the min/max item in the set  
Unlike normal queue, where its just the first item, this returns either the min or max depending on how it is set up  
Methods are: add, findMin/findMax, deleteMin/deleteMax  
There are multiple implementations to accomplish this  

Using a linked list:  
Add = O(1)  
findMin = O(N)  
deleteMin = O(N)  

Sorted Linked List:  
Add = O(N)  
findMin = O(1)  
deleteMin = O(1)  

Self-balancing BST:  
Add = O(logN)  
findMin = O(logN)  
deleteMin = O(logN)  

Using a complete tree type we can get:  
Add = O(1). 
findMin = O(1)  
deleteMin = O(logN)  

A complete binary tree has its levels completely filled (except possibly the bottom level  
All possible nodes on a level needs to be filled.  
This is a binary tree not a binary search tree  
This means it isn’t necessarily sorted  
The bottom level is filled left to right -  no ambiguity on where to add. Always gets added to the bottom level  
This is sacrificing the sortedness of the BST for completeness. This is always keeps the tree “balanced"  

An N-node complete tree has log N height (bottom most level has N/2 nodes, the next level has N/4, etc)  
Operations are at most O(log N)  

Complete trees can be stored as an array without issues  
The root of the tree is at array[0]  
The left child is at array[1]  
The right child is at array[2]  
For any node at index I, its children are at (I\*2)+1 and (I\*2)+2  
Any node’s parent is at (I-1) / 2 or (I-2)/2 we don’t know which off hand  
No links needed  

This stores the order of the nodes in a Breadth first order   
Create access helper methods for finding these  

```
int leftChildIndex(int i) { 
    return (i * 2) + 1;
}
int rightChildIndex(int i) {
  return (i * 2) + 2;
}
```

For a priority queue use a binary heap which is a special binary tree  

### Binary Heap
This is a binary tree with two special properties  
It is a complete binary tree  
The data in any node is less than or equal to the data of its children (not necessarily all values on the level of its children)  
This is **min-heap**  
There is a max-heap where any node is greater than or equal to its children.  
The order of the children does not matter (left to right can appear random in ordering) they just have to be larger than the parent  
This means the no matter what, the root is min value  

To be a true binary heap it must maintain the complete tree structure, as in it is filled left to right.  
This means you have to move nodes around to ensure its structure is maintained  
To do this, add any new node to the bottom level as far left as possible.  
Percolate the node upwards  
You then swap with parents until the new added value is greater in value than its parent. (As in as long as it is is less than what it is being swapped with, swap it)  
This means the added value will be < all children it gets, but still > the parent above it.  

```
While(item < parent)
    Swap
```

Add is now O(log N) as at worse it has to swap all the way to root. Average case is more complicated.  
It has been proven it takes 1.6 swaps on average for any N, and one comparison made. This means average complexity is O(1) as add more often than not terminates before reaching the root  
This is because most of the nodes are on the bottom 2 levels of the tree  
The analysis to reach this conclusion is complicated  
About half the time you are likely to be adding on the bottom level anyway, and about a quarter of the time on the level above it. This likeliness halves ever time you move up on level (1/8th, 1/16th)  
If you sum up these probabilities it looks like K/2^K this sums to 2, which is O(1)  
(1/2)\*1 + (1/4)\*2 + (1/8)\*3  
1, 2, 3 are the number of comparisons needed to be made. It is always one more than the level it is on, as it needs to compare to the level above to know to stop

Priority Queues only support removing the min/max depending on set up. This means our binary heap is always at the root  

When we remove the root there is now a hole that needs to be replaced.  
Put in the “last” item for the root (bottom-most most-right item)  
Percolate down (reverse of add percolation)  
Have to compare with both children. Swap with the smaller of the two children.  (It isn’t guaranteed to always be on left or right, can be either)  
Keep moving down swapping with smallest of the two children until that node is less than both of it’s children (if any are present) 

Worst case of remove is O(log N) - has to move all the way down to bottom level again  
Average case is O(log N) - very unlikely not to have to go down to the bottom 2 levels  
Like add it nearly is always terminated in the bottom two levels.  
Because it moves down however, this makes it O(log N) instead of O(1) like adding

*Implementation*   
To keep add cheap you need to keep the array large (empty space) - this prevents always regrowing  
Then keep track of the size (space used)  

Average Complexity  
Add = O(1) (percolate up - average of 1.6 swaps)  
findMin = O(1) - always the root  
deleteMin = O(log N) (percolate down - usually log N swaps)  
