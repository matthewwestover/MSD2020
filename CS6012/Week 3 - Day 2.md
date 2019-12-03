## Week 3 - Day 2
### Binary Search Trees
We want a container that can easily/quickly:

* Add Items
* Search for Items
* Remove Items
* Order all Items

**Sorted Array** - Search is fast (binary search - O(log N)), slow add/remove (O(log N) + O(N))  
**Sorted Link List** - Slow search(O(N)), slow add/remove(O(N+1))  
**Unsorted Arrays** - Fast add (O(1)), slow remove(O(N)), slow search (O(N))

Binary Search Tree is a Binary tree that has restrictive ordering of the nodes  
All items on its left side is smaller  
All items on the right is greater than/equal to the node itself  
They have to have a comparator or implement comparable to allow this evaluation  
Once set up, allow for fast searching and addition/removal 

### Searching
Compare item against root. If less go left, if more go right, if equal you found it  
Treat next node like a new root. If less go left, if greater go right if equal you found it  
This lets you discard entire branches of the tree at each comparison  
You are discarding ~half of the possible items each time you do this on average
This is O(log N)  
Once we find a leaf node that doesn’t equal the value of what we are looking for, return false  

```
boolean search(BSTNode n, String item) {
    if(n == null)
        return false; // didn’t find the item
    if(item.equals(n.data)) 
        return true; // found the item
    if(item.compareTo(n.data) < 0)
        return search(n.left, item); // recursive call for left
    else
        return search(n.right, item); // recursive call for right
}
```

### Insertion
Go through the branches just like searching. Is the item < or >= to the node  
Once you find a node with an empty pointer in the direction the value needs to go  
This always adds to a leaf node  
This is O(log N) complexity just like search  
In worst case you can end up with always branching in one direction, which becomes a linked list  
Shuffle the data as we don’t know if it is sorted or not  
This prevents “worst” case sorted data that is always in one direction  

```
insert (Node node, Item item) {
    if(item < node.data) { 
        if(node.left == null)
            node.left = new Node(item) // item is less than current node and left is null add it
        else
            insert(node.left, item) // recursive call to the left node
    }
    else {
        if(node.right == null)
            node.right = new Node(item) // item is greater than current node and right is null, add it
        else
            insert(node.right, item) // recursive call to the right node
    }
}
```

The *Average* Tree is if items are inserted in a random order it will “balanced” reasonably. Presorted data is worst case on these operations, which is always O(N)  
Best Case / Average is the expected O(log N). Sorted array is worst for a binary search tree  
This means its faster for insertion than just a sorted array, but with a similar search speed  
Deletion is far more complicated than searching or adding. 
You want the left most node on the right side of the node you are deleting  
This means everything on the left stays less than, while everything on the right stays right

### Deletion
Stop at parent of the node to be deleted, set its pointer to the deleted node to null  
(Current.left or current.right is to be deleted)  
Three cases of deletion  
Deleted node has **0 children, 1 child, or 2 children**. View them all as containing subtrees  
**0 Children** on the deleted node, parent pointer was set to null; it is now deleted    
**1 Child** on the deleted node, take that deleted node and have the parent of the deleted node point to that child  
This works because everything in that direction is already < or >= of the parent. Basically just bypass to the deleted node and point the parent of the deleted node to the child of the deleted node  
**2 Children** on the deleted node you have to account both sides  
Because everything on the right is greater than, finding the smallest node on that subtree is larger than everything on the left of the deleted node.  
Because it is the smallest on that side, everything on that side remains larger

This smallest of the right side is the **successor** node  
You delete this original node as well, and create a new node in the original deleted nodes place that points to the correct children   
Sometimes the smallest node still has right sided larger nodes. This turns into deleting the successor node as well, which is a 1 child deletion  
This means you check ```current.left.left == null```, to keep the parent of the successor node. The successor node needs to be deleted in its current location  

Finding the node to delete is O(log N)  
With no children deletion is O(1)  
With 1 child is O(1)  
With 2 children  
    Find successor is O(log N)  
    Deleting duplicate successor is O(1)  

These trees also have other useful properties:   
Smallest value is the left most node with a null in its left child pointer  
Largest value is the right most node with a null in its right child pointer  
Depth-first in-order traversal will print all values in ascending order  

Often create a helper function that can find the successor node