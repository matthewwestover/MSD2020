## Week 1 - Day 1
### Data Structures and Algorithms 
This isn't a *programming* course but an actual *Computer Science* course:  

* Learn to think about programs in abstract ways  
* What do we expect in runtime, memory usage, etc? 
* How to analyze your algorithms (recipes to get things done)
* Improve efficiency
* There are optimizations but hard limits on improvement based on what algorithms you use. Some are just better at things.
* Make good coding choices 
* Recursion - see recursion
* Divide and conquer algorithms
* Basic sorting algorithms is one of the most studied operations in CS
* Elementary data structure - basic building blocks of all software systems

This stuff comes up in interviews ALL the time  

EX. real world problems that use algorithms:

* Traveling in NYC what is the fastest combination of public transportation to get between two points?
* Find a word in a dictionary fast
* Given a start page and and end page, can you reach it just by embedded links (wikipedia chase)
* Given a sentence as a string, reverse the characters in each word, but not the sentence over all.
* In job interviews often get Solve then, then Optimize it

There are many different ways to solve problems  
One method may take 1 micro second longer per item  
Computers typical operate on huge number of items: millions/billions+  
1 micro-second isn’t much of an improvement of an improvement in some cases. 100 items isn’t a big increase  
1 billion items however is. that is 1000 seconds of additional runtime  

### N 
Unspecified integer quantities as ’N’  
N is the problem size  
Sort an array of N numbers  
Searching for a item in a set of N items  
Insert an item into a set of N items  
The amount of work done depends on N  
Work required is a function of N  

Algorithms dont always take N steps for N items  
They can be linear, quadratic, logarithmic  
This is the **time complexity** of an algorithm  
N^2 is much larger than N  
For algorithms we only care about large N  
“Is this scaleable” is it complex and fast for large N  
If you know something will be small, or the gain is minimal just simply pick the easier/quicker one to code out. 

### Big O
Algorithms are in Best/Average/Worst case scenarios.  
*O(N log N)* or *O(N^2)*
This only captures the trend, not the magnitude.  
The actual value could be like 100(N log N) or 10(N log N).  
Both show up as O(N log N), but 10(N log N) is faster  

1ms and 30ms may not matter if it happens once, but in computing it does.  
Gaming for example has to be done real time, the faster things happen the better.  
Google has 1 billion searches every single day. It has to be fast.  

EX matrix multiplication is commonly seen as N^3 times to do it. 
It is currently most optimized to N^2.373 which was as recent as 2011-2014 by Virgina Williams  
Current default for sorting in several languages (python, java) is *Timsort* which started in 2002

The best algorithm may not be the best coding to do in practice. Time to code and actual usage matters

### Phases of Software Development
* Requirements Gathering
    * Read and understand assignment specs, ask questions
* Planning Design and Analysis
    * Outline how to solve the problem, determine algorithms, write pseudocode
* Construction   
    * Write code, debug syntactic errors
* Testing
    * Test to find semantic errors, boundary cases
* Maintenance
