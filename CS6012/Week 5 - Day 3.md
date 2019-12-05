## Week 5 - Day 3
### Compression
Utilizes Binary coding  
Base-ten numbers each digit corresponds to a power of ten  
436 = (4\*10^2) + (3\*10^1) + (6\*10^0)  
Base-two numbers do the same thing  
101 = (1 * 2^2) + (0 * 2^1) + (1*2^0) = 5  
Computers use binary because they are composed of switches that are on or off  
ALL data in a computer is stored in binary (sequence of bits) 8 bits make a byte  
3 decimal digits can represent 0-999  
One byte can represent 2^8 digits - 0-255  

ASCII uses bytes to code characters   
00100000 = 32 = ‘ ‘ (blank space)  
00111011 = 59 = ‘;’  
01000001 = 65 = ‘A’  
01000010 = 66 = ‘B’  

The test “Hello” is stored on a computer as 0100100001100101011011000110110001101111  
Text editors know to treat every byte as an ASCII characters  
1000 character file takes 1000 bytes of storage  
Videos take more  
We have instances of things we just can’t store. Terapixel cameras can send a terabyte of data a second which is just too much to write to disk  
We use compression on files so we don’t store the “raw” file to prevent memory overusage  
Netflix, Youtube, etc decompress files on the fly as they are watched  

‘ddddddddddddabc’ = 15 characters = 15 bytes. 
This can be represented in less than 15 bytes. The repeating ‘d’ can be stored d and the count of how many times it is used in a row. Becomes 5 bytes instead  

You **compress** (lossless or lossy - can return to the “raw” file or not) to shrink it down  
You **decompress** to expand back to the full data  
We can also represent common characters as fewer bits and rarer characters with more bits to also compress further. 
This means we have to be clever in decompressing back to the original state  

Example:  
E can be compressed as 11  
Q can be compressed as 0011  
Z can be compressed as 1100  
What does 110011 decompress to?  
We need to have instructions/indication of what the sequence was to decompress  
To do this, we include the code to decompress the file in the file  
This means the file is a little bigger than just the compressed data itself  

### Binary Trie
Trie is a French word pronounced tree  
Special binary tree where a left branch represents a 0 and a right branch represents a 1  
The path to a node represents its encoding  
Leaf nodes are what contain the data itself. Every other node is just set as branching left and right

Example:  
‘Ddddddddddddabc’ = 15 bytes = 120 bits  
A = left left left = 000  
B = left left right = 001  
C = left right = 01  
D = right = 1  
Using a binary trie this can get encoded as ‘11111111111100000101’ = ~3 bytes = 20 bits  
To decompress - go bit by bit, and whenever you reach a leaf node get the value of the node and then start from the top of the tree with the next bit  

01000001 = 1 byte = 01 000 001 = cab  
01 = c  
000 = a  
001 = b  

To build a good binary trie we want to encode the most common characters with fewer bits (closer to root)  
This most common characters can change file to file, leading to a different compression tree  
When we compress a file we need to see what is common and what isn’t  
Typical English ‘z’ isn’t common but if the file has it a lot, the trie will place it higher up  
The trie has to be stored with the compressed file  

### Huffman Coding
This is specific and systematic manner to generate a binary trie  
*Encoding*  
Construct a Huffman Tree - repeatedly march lowest frequency nodes  
Write a header - info to reconstruct tree  
Headers cannot be compressed.  
Write encoding chars to the file - using the Huffman tree  

*Decoding*   
Read the header - reconstructs the tree  
While read encoding bits - traverse leaf - write the decoded char  
Read the EOF file - original file  

Algorithm is count the frequency of each different character  
Anything with more frequency needs to be near the top of the trie  

Start by having each character be its own “tree” of a single node  
Each node contains the character and frequency count  
Start by merging the least frequent characters into combined trees. 
If characters have the same values, you have to specify a “tie-breaker” for what is more frequent 

When these get merged, the new connecting node just contains a combined “weight” this is what makes a comparison for the next round of merging frequency  
Always merge the lowest values, so the newly created “tree” doesn’t necessarily get merged again right away  
Higher frequencies go higher on the tree. Combined nodes typically have the smaller value on the on the left  
Final tree contains string length at root  

In practice to build this create nodes and add them to priority queue - use a minHeap to remove nodes and combine them as you dequeue

```
// Add all nodes to a PQ
PriorityQueue<Node> pq = new PriorityQueue<Node>();
pq.addAll(…);

// Remove two smallest values
left = pq.dequeue();// left always first!
right = pq.dequeue();

// Merge the two nodes by creating a new node that combines the weights of left and right
newNode = new Node(left, right); 
left.parent = newNode; 
right.parent = newNode;

// Add that new node to the PQ which will automatically place based on “weight”
pq.add(newNode);

// Repeat until queue has size of 1 - this item is the entire tree
```

### Tiebreaking
Trees are constructed twice - once at compression, once at decompression  
Without a tiebreaker they could be constructed differently  
Common tiebreaker for these is ascii value of the character  
Basically combine the two lowest ascii value characters of the same frequency  The lowest value to the left. 

### Parallel Programming 
Computers have multiple cores - pretty much every PC made today does  
We’ve reached the hard physical limit on transistor size and capacity  
Using multiple cores and parallel computing to increase speed despite this hard limit  
We’ve been coding serial programs, but that is wasting the full power and speed of the multiple cores  
Not utilizing the multiple cores is a common issue for PC programming  

Programs sometimes have tasks that depend on others, but also tasks that can be done simultaneously and combined at the end

We can take a serial algorithm and run it in chunks at the same time  
Ex  
Sum every number in an array  

```
int sum = 0;
for(int i=0; i < arr.length; i++)
   sum += arr[i];
```

In parallel programming we split this work on to two CPUs  
Sum1 + array[0] … array[N/2-1]  
Sum2 + array[N/2] … array[N-1]  

After both are done - final sum is sum1+sum2  

Just throwing more cores doesn’t mean the above for loop runs faster. We have to specify the breaks for the split on the multiple cores. We cannot expect a compiler to split and utilize all the PC cores. We have to be explicit  
Using new libraries on languages we can annotate to the compiler on how to split the work between the cores  
Biggest challenge in parallel programing is how to split the work that needs to be done

Another challenge is how to combine the data at the end, these can end at different times, and we need everything that is split to be done before combining them  
These are called dependancies  
Dependancies are serial in nature but can contain parallel methods

Easiest way to do this is just equally split the work between how many cores you want it on. 2 cores? Divide data in half and calculate halves then combine

It can be easy to create parallel programs that don’t actually increase speed.  There is a cost in creating a new thread, etc. Doing it where it actually benefits the performance can be hard.  
Most threading libraries use a “thread pool” to create the threads and choose when to use multi-threading for the performance gain

### Amdahl’s Law
The theoretical upper bound on performance gain aka the best possible outcome  
Let T be the execution time of the task  
Let p be the fraction of the program that’s parallelizable  
Then, we can write ```T = (1-p)T + pT``` T = serializable portion + parallelizable portion  
Let s be the number of threads/cores we can utilize  
```T(s) = (1-p)T + (p/s) T``` parallelizable is the split among how many threads we can create  
Basically more threads only speeds up the parallelizable portion  
The speedup is then ```S = T(s)/T = 1/(1 – p + p/s)``` Speed up is T with threads over the T without threads  
S = 1/(1-p) as s goes to infinity  
1-p is the serial portion of the program   
Speed up is HARD limited to serial portion. No matter how many threads we add, we can only go so fast based on the serial portion

If 90& is parallelizable (which never happens, often more like 20%) p = .9  
S = 1/(1-0.9) = 10  
This means at BEST it is only a 10 times increase in speed  

This is called strong scaling - more threads at a set program  

We can also weak scale where we increase the problem size. This comes in with Gustafoson’s Law.  
Keep time a constant as problem size and threads grow.  

