## Week 9 - Day 1
### Program Tuning

Notes 

```
1 ns = 10^-9 seconds 
1 us = 10^-6 seconds = 1,000 ns 
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns
```

| L1 cache reference                 |         0.5 ns |            |        |
|------------------------------------|---------------:|-----------:|-------:|
| Branch mispredict                  |           5 ns |            |        |
| L2 cache reference                 |           7 ns |            |        |
| Mutex lock/unlock                  |          25 ns |            |        |
| Main memory reference              |         100 ns |            |        |
| Compress 1K bytes with Zippy       |       3,000 ns |       3 us |        |
| Send 1K bytes over 1 Gbps network  |      10,000 ns |      10 us |        |
| Read 4K randomly from SSD          |     150,000 ns |     150 us |        |
| Read 1 MB sequentially from memory |     250,000 ns |     250 us |        |
| Round trip within same datacenter  |     500,000 ns |     500 us |        |
| Read 1 MB sequentially from SSD    |   1,000,000 ns |   1,000 us |   1 ms |
| Disk seek                          |  10,000,000 ns |  10,000 us |  10 ms |
| Read 1 MB sequentially from disk   |  20,000,000 ns |  20,000 us |  20 ms |
| Send packet CA->Netherlands->CA    | 150,000,000 ns | 150,000 us | 150 ms |


Numbers change specifically, what is important is the orders of magnitude between them. 

Tuning is hard  
There are profilers to find time sinks  
20% of the code accounts for 80% of the delay.  
Taking time to tune you need make sure you are working in the correct place  
Sometimes changes don’t do anything.  
HOWEVER, that change can make some other change faster.  
So changes might look minor at the moment but can have impact later. So keep them  
Algorithms optimization > loops  
When tuning make sure to check production mode not debug mode. Compiler can optimize things for you as it hits production’s higher compile level

**Make it. Make it work. Make it right. Make it fast.**

Xcode and CLion can run profilers. You cannot type expressions in. You need to change it accept a command line

```
_let fib = _fun (fib)
            _fun (x)
                _if x == 0
                _then 1
                _else _if x == 2 + -1
                _then 1
                _else fib(fib)(x + -1)
                    + fib(fib)(x + -2)
_in fib(fib)(28)
```