## Week 3 - Day 1
### Trees

There are not always a linear direction of data, sometimes we need *generational* across level relations  
Trees can go up and down that allow across level  
Much like a family tree, track *descendants* and *ancestors*  

*Lists* are the most basic container of data  
There is no special requirements on the structuring of the data  
Data is stored linearly, either in an array or a chain  
Trees are more intricate structuring  
This makes them more useful for certain issues  
This can also speed up algorithms  

Trees have a hierarchical structure  
They work like a linked list where a node has multiple branches (links) instead of a single forwards/backwards like normal linked lists  
Trees are drawn upside down with a **root** at the top and its branches below it  
Every node is a *sub-tree*  

There is a strict parent to child between nodes  
Links **ONLY** go from parent to child  
Nothing from child to parent (this makes it a graph)  
Nothing between sibling to sibling  
Every node has 1 parent except for the “root” which has none  
There is exactly 1 path from the root to any other node  

### Terminology
**Root node** - single node in the tree that has no parent  
**Parent** - a node’s parent has a direct reference to it. Nodes have one parent only (except for the root node)  
**Child** - a node is a child of node if the parent has a direct reference to the node  
**Sibling** - two nodes are siblings if they have the same parent  
**Leaf node** - a node with no children  
**Inner node** - a node with at least one child
**Depth** - how many ancestors a node has (how many steps from root to this node)  
**Height** - the depth of the deepest leaf node  
Children are exactly 1 level deeper than their parents  
Root node has a depth of 0  
Any node is a sub-tree itself, including leaf nodes  

### Binary Trees
Special kinds of trees where a parent node can only have up to 2 children  
These children are designated *left* and *right*  
Representing these in a linked list, each node gets a left and right pointer, defaulting to null.  
When a child is added specify left or right to point to  
There are special cases where left and right are < or > but in general it is unstructured in that regard, just left or right  

```
class BinaryNode<E> { 
	E data;
	BinaryNode left; 
	BinaryNode right;
	// methods
}
```

The BinaryTree class would have a “root” link to the root node, along with any methods the tree needs  
BinaryTrees are more complicated than a normal linked list to traverse  
We can’t just go in order, and children don’t know what their parents are to go back up in the tree  
Only Parent -> Child  

**Depth First Search** - Go to all children before siblings

```
public static void DFT(BinaryNode N) { 
  if(N == null)
    return;
  // do something with N.data

  DFT(N.left);
  DFT(N.right);
}
```

This makes them recursive in nature  
Start at Root, then go to the left, then the right. Will go down as deep as possible on the left. At the last node once left is null, return back to that nodes parent and check the right. 

It can be done without recursion:

```
create empty stack: S 
S.push(root node) 
while(!S.empty())
{
Node current = S.pop() 
visit current 
if(current.left != null)
    S.push(current.left) 
if(current.right != null)
    S.push(current.right)
}
```

There are three ways to utilize a depth first traversal  
*Pre-Order* - use the node before going to its children  
*In-Order* - traverse left node, use node, traverse right node  
*Post-Order* - use node after going to both left and right  

Expression Tree Pseudocode

```
public static double evaluate(Node n) { 
    if(n.isLeaf())
        return n.value;
    double leftVal = evaluate(n.left);
    double rightVal = evaluate(n.right);
    if(n.operator == ‘+’)
        return leftVal + rightVal;
    if(n.operator == ‘*’)
        return leftVal * rightVal;
... 
}
```

**Breadth First Search** - visit every sibling on a line before going down to the next level - use a queue  

```
create empty queue: Q 
Q.enqueue(root node) //add left and right in queue
while(!Q.empty())
{
    Node current = Q.dequeue() 
    Do something with current 
    if(current.left != null)
        Q.enqueue(current.left) 
    if(current.right != null)
        Q.enqueue(current.right)
}
```

A adds B and C. B is dequeued, it adds left and right, C is next to dequeue and it adds its left and right  
This stacks up in order for each level of depth.  