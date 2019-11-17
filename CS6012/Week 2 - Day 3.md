## Week 2 - Day 3
Debugging - don’t set into code, just run to breakpoint to find where things might be breaking.  

### Slopes of Timing Data Analysis
Time vs size of array. Results in a curved line, we can’t tell if it is quadratic or exponential, etc.  
Without permuting the data you can find complexity:  
1 way is table with all N and all T values  
Between pairs in table, compute slope. 
If slope is roughly the same, T = SN+B = O(N)  

When it varies you have to find “how fast” is the slope changing  
If it is very minor increases from point to point, it is more O(N log N)  
If Time itself grows really slowly it is a log N  
(Can plot N vs Log N to compare it to the data to see if it is similar)  
If the slop increases linearly (s2 *constant to S1, s3 * same constant to s2) this is quadratic O(N^2)  
You can also calculate the slope of the slopes to see if it is linear.  
If slope increases quadratically, it is O(N^3).  

More accurate way to chart these take the log of both N and T  
Any log value works, but log base 2 is CS stuff  
N log N looks like a slight curve up, N^2 and N both are linear.  
T = CN^x. We want to find x  
logT = logC N^X = logC + X log N  
X is the slope of the line that comes from graphing logT v logN  
y= c +xN. X is the slope in this equation, this is the X from N^X  
Table LogN LogT and Slope and calculate.  
Slope 1 = O(N)  
Slope 2 = O(N^2)  
Slope 3 = O(N^3)  
Slope <1 and changes slightly = O(log N)  
Slope >1 and changes slightly = O(N log N)  
This also should be the same as Slope O(N) + O(log N) values  

T = C NlogN  
Log T = LogC + log(NlogN) = LogC +log N + loglogN  
logT = (log C + loglogN) + logN

### Memory and Linked Lists
All data in a program resides in memory at some point  
Memory is a giant block of bytes  
Each byte has its own address, which is just a number  
Byte N is next to Byte N-1 and Byte N+1  
New keyword, instructs system to find a contiguous block big enough to hold whatever you are creating  
New int arr[] = new int[10] finds a block big enough for 10 ints  
Arr[0] is next to arr[1]  
This makes it easy to iterate and find things  
This means arrays are random access, it is instant to access any item  
Address of arr[23] is address of arr[0]+(23*4) 

New allocates things anywhere in memory  

```
Circle c1 = new Circle();  
Circle c2 = new Circle();  
```

There is no guarantee that c1 and c2 are next to each other. The data with in each individually is contiguous  
C1 may be at 2048 and c2 may be at 640  
This means random access isn’t instant or free.   

We can create our own objects to store multiple objects that aren’t necessarily linked otherwise without using an array  
Each individual item have links (references/pointers) to the other things.  
Items can be dynamically added or removed from the structure by creating or destroying links  
This makes it possible to drop multiple objects if you dont update other links. This is an important thing to remember  
You might want that or you might not

Basically a data structure where there is a collection of pointers to other stuff  
These look kind of like recursive

```
class LinkedNode {
  // each node stores some data
  int ID;
  String name;
  LinkedNode next;  // and one of itself!
  ArrayList<LinkedNode> neighbors; // if it needs to have pointers to multiple things
}
```

With an array, to access any given item we use [ ]  
To do this with a linked list  
Keep track of the first node - the “head” - where the linked list “starts”  
To access 4, you have to start at 1 then find 2, then find 3, then find 4  
Bestcase in this is O(N) where an array is O(1)  
When we reach the end of the list, the node is null.  
No one node knows the length of the linked list.  

```
LinkedNode f = new LinkedNode();
f.ID = 5;
f.name = “first node”;

f.next = new LinkedNode();
LinkedNode temp = f.next; 
temp.ID = 12;
temp.name = “Billy”;
```

This differs from arrays, array[index] becomes list.next.next.next.next  
Inserting and deleting is much simpler. Either update a pointer to the new one, and the new one to what the prior one was pointing to. This is “splicing".  
Removing just move a pointer away from it. Garbage collector would clear what was “bypassed"

Size doesn’t make sense for a linked list. Not every node needs to know the size  
The outer class “linked list” keeps track of headnode, size, and defines all methods  
Each node is simple with a data and one or more links to other nodes  
If a node is the end, the next is null. 

```
public int contains(int item) {
  Node temp = head;
  while(temp != null) {
    if(temp.data == item)
return true;
    temp = temp.next;
  }
  return false;
}
```

#### Doubly linked lists
Nodes also add a previous method. This points back to what points at it  
Can be traversed in both directions  
Linked list maintains a tail member variable as well as the head member variable  
Special cases are difficult.   
Empty lists have head and tail are null  
Single item lists have head and tail point to the same node  

Insertion means updating pairs of pointers  

```
newNode = new Node<Character>(); 
newNode.element = 'n’; 
newNode.prev = curr; //this nodes previous node is what one we are currently at to inset
newNode.next = curr.next; //this next is what the current one’s old next was. 
newNode.prev.next = newNode; // set the item before this’s next pointer to this item 
newNode.next.prev = newNode; // set next items previous pointer to this item
```

#### Complexity
Insertion/deletion (assuming position is known) 

* LinkedList: O(1)
* ArrayList: O(N)

Access random item

* LinkedList: O(N) 
* ArrayList: O(1)
