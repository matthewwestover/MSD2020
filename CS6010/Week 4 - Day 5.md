## Week 4 - Day 5
```
Void x::method(){
     // this is a pointer to x
    To find part of x. (*this).member 
    Updated to this->member
}
```

To keep overloaded functions as method in class: 

```
For(I-0 ..size){
    // This[I] is really *(this + i) which doesn’t work. 
    To call operator [](i);
    This-> operator[](i); 
}
```

If you have to do ```this->``` a lot. Create reference X& self = *this  
This[I] gives stack buffer overflow. Trying to move the pointer in stack to where memory can be everything, not the info on the other end.  

Remaining difference between our DIY Vector and std::vector:  
We currently have to pushback info, std::Vector uses standard initializer list (vector v = {1,2,3})  
Vector of Vectors and Vector of Strings  
Calling constructors and destructors is tricky. Passing T as a type  

### Fast Code - Optimization

Fastest to Slowest operations:  
Arithmetic (so fast sometimes “free”)  
RAM  
SSD  
I/O  
Network  
Store to Disk  

Latency vs throughput  
Receive something vs how much I get when I get something
Things can have high latency and high throughput, that is slow to start but fast once going  

Gap between the speed of these things has gone from being about equal to much wider spread.  
Modernly we can have millions of arithmetic instructs in the time it takes to do one disk read  
We have massive ram stores now, much more complicated vs when ram was in kilobytes several decades ago.  
Bigger == slower  

To solve this problem we “lie” by saying we have small amounts of memory to move faster  
This is done with caching  
Cache is layered between big and slow to small and fast  
By moving info through these layers we get the “illusion” of big fast memory

RAM is big 10^(12-15) bytes (giga-terabytes), vs CPU Register 10^1 (like less then 100 total registers, 16 general public use most are “secret” by the CPU)

In between RAM and CPU Register We have RAM > L3$ 10^7> L2$ > L1$ 10^3> CPU  

First time we access RAM, it is “slow”. We get a chunk of memory that gets passed through each cache. The next time we ask for memory we can pull from L1$ cache  
100s CPU cycles at ram down to 1 CPU cycle at the Register. 
Any time we have to go to RAM we are missing hundreds of possible arithmetic functions  
**Wu-tang clan “Cache Rules Everything Around Me”**

Computer assumes we ask for memory near each other. “Locality” 

Spacial Locality - This is why it grabs data in chunks, We ask for one thing, but are likely to need things near that memory location. Useful for things like going through arrays in order. Out of order jumps around, which has less locality. 

Temporal Locality - Memory I have touched recently, I will likely touch it again soon.

High levels of spacial and temporal locality means your program runs really really fast. 

Run time vs “working set” graph looks like stairs. L1 cache is fast, runout big increase until the next cache is hit, then runtime flatlines again until it needs to access the next cache or RAM. 

2d array vector<vector<float>> aoa;  
Aoa[row][column]  
Aoa[column][row] to access data  
Both are “reasonable” options to access specific item in the 2d array.  
Row-column, jumps me to a row in heap and then what column item on the second heap of the floats

To sum all, use nested for loop  
For(c = 0; col^2)  
    For(r=0; row^2)  
        Sum += aoa[r][c]  
This is a horrible use of cache system. We load an array look at one item, then load a completely different item. Basically every new part of the loop forces a new load into cache. Small 2d array is still okay, but big (10s of thousands) is slow

Better to swap. Do all items in a column per row.  
To be even more fast, keep data in order.  
Vector<double> bv; // this is all the data from all the columns and rows.  
We need to number each item in the “box” of data.  

| R3 C1 | R3 C2 | R3 C3 |
|:-----:|:-----:|:-----:|
| R2 C1 | R2 C2 | R2 C3 |
| R1 C1 | R1 C2 | R1 C3 |

This one big vector is 1d, but we need to access two coords.  
Bv[index] needs to know what’s around it it.  
Index = f(r,c)  

Bv needs to layout data in strips. R1C1,R1C2, R1C3, R2C1, R2C2, etc.  

R+number of columns + c:  
For each (col)  
    For each (row)  
This is fastest for this formula.  

If you switch the two, BV is strips of columns. Flip everything. c*number of rows + r  
For each (row)  
    For each (col)  

Which one you use depends on context.  
Neighborhood is the touching squares (pixels)  
Data on three rows and three columns all at once.  

Only way to know your code is faster is to measure it  
Profiler - runs program and records info about program while it runs  
Sampling time profiler gives view of the timing. It samples every few  milliseconds. It can miss quick functions  
Still a good go to tool. Easy to do.  
