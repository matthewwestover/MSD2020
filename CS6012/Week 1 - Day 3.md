## Week 1 - Day 3
### Algorithm Analysis
Correctness is only half the battle  
Programs should finish in a reasonable amount of time  
The run time is strongly correlated to the choice of algorithm  

*Find a Word in the Dictionary*

* Option 1:
    * Start on first page, first entry
    * If word not found move to next
    * If end of dictionary reached, word not reached
    * Worst case it takes N total time in not finding the word
    * The larger N the longer this takes
    * This is a correct algorithm to find a word in a dictionary
    * But it can be much better
* Option 2:
    * Guess which page the entry is on
    * If too far/not far enough narrow in
    * Continue until narrowed in to find it
    * This assumes words are sorted in order to be efficient
        * If not no different than option 1
        * If it is sorted, it is much faster as we eliminate chunks of the dictionary we don’t need to look at
* Option 1 is a linear search
    * Runtime correlated to size of dictionary
    * Assuming 180k words, and .25s to check each word takes 12 hours to finish in worse case
    * If we double the size, it takes twice as long
* Option 2 is a binary search
    * More like what humans do
    * 4 seconds to complete
    * If we double the size, it is the current time + .25s because every .25 seconds we are taking half of the total

###Logarithms  
An exponent indicating the power to which a base is raised.  
logbN = Log N to the base N  
If LogbN = X  
**B^x = N**  
Default is base 2 in computer science  
Any digit to represent in binary is log2N  

Logs grow slowly  
9 < log1,000 < 10  
19 < log1,000,000 < 20  
29 < log1,000,000,000 < 30  

The best algorithms we have can do *N log(N)*  
Binary sort assumes data is sorted  
We can take time to sort first to save time later  

Linear search is a straight line growth  
N = T, double size 2N = 2N

Binary search are log growth rate.  
N = T  
double size  
2N = T + 1

###Complexity Analysis
Finding max value in array

```
int max = array[0];
for (int i=0; i<n; i++) {
    if (array[i] > max ) max = array[i];
}
```
We want to count the number of instructions

* Break code into simple instructions
* Things that the CPU can directly execute (roughly)

Assume the following are one instruction each:

* Assigning a value to a variable
* Looking up the value of a particular array element
* Comparing two values
* Incrementing a value
* Basic arithmetic ops such as addition and multiplication

Assume branching is instantaneous  
Above: array[0], max = array, I = 0, I<n happens N times  
I++ happens n times plus an assignment  
Array[I] happens n time and array[i] > max happens n times  
In worst case max = array[I] is 2n times  
This totals to 6N + 4  
As N gets large +4 matters less and less  

In reality this is different as CPUs, Compilers, Programming languages do things different.  
Computer Scientists ignore these when reasoning about complexity.  

We ignore a lot of this. +4 doesn’t matter. The constant in front of N also matters less  
This function is O(N)  
The running time of a loop is at most its body times the number of iterations.  The inside doesn’t matter. We just see a loop going from 0 to N, it takes N  
Nested loop, in the same setup we just multiply the N together. (i 0 > N, j 0 > N)  

We look for *asymptotic behavior*.  
What happens as N items reaches infinity.  
This is the “complexity” of the algorithm.

```
f(n) = 5n + 12 -> This is N
f(n) = 109 -> This is 1
f(n) = n^2 + 3n + 112 -> This is n^2
f(n) = n^3 + 1999n -> this is n^3
f(n) = n + n^(1/2) -> this is n1/2 which is similar to logN
```

To verify complexity on graph data. Plot as log Time on y and log N on x.  Whatever the slope is that is the power value of the above. If slope = 1, it is just N. If slope = 2, it is N^2  
Between 1 and 2 is N logN  

Never go off what the graph shows, back up calculations of the slope to say it is N^2, etc.  
T < cN^P  
Log T < log(cN^P)  
Log T < logC + PlogN  
This just shifts the curve up. And P still remains the the value we care about. 

**Log growth**  
Repeated doubling principle  
x=1 how many times of x*2 before x>=N  
X can only double log N times before it is >= N
Repeated halving principle  
x=n, how many times of x/2 before x<=1  
X can only half log N times before it is <= 1

### Big-O notation
Assume N is huge  
We know there are possible other constants and variables at play  
N^2 in big O is O(N^2)  
O(N^2) and O(N^3) is bad and impractical  
Clever programming tricks dont speed up highly complex algorithms  
Better to pick the faster one from the get go  

**Worst Case**: Has to run on all inputs, it will never be more than this  
**Average Case**: The common case, most useful and what algorithms are in practice  
**Best Case**: Absolute fastest an algorithm can finish

**Gauss’s Trick** 

``` 
I = 0; I < N; I++  
    J = I, j < N; J++
```
Becomes (N-1)(N-2)/2 = O(N^2)
