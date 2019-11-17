## Week 1 - Day 5
### Recursion

Break a problem into smaller parts of the same type  
A function calls itself on a *subset* of the data  
A problem solving technique in which the solution is defined in terms of a simpler version of the problem  
A method that calls itself it must resolve itself to prevent an infinite loop  
**Base case** is the stopping point for a recursive method  

N! - N factorial  
Factorial(N) is defined as N * N-1 * N-2 * … * 2 * 1  
Recursively it is factorial(N) = N * factorial(N-1)…  
Base case is 1, where it stops

EX:

```
Public static int factorial(int N) {
    If(N==1)
        Return 1;
    Else
        Return N * factorial(N-1) // calls the same method. 
}
```

EX 2 Summing Recursively

```
Public static int sum(int N){
    If(N <= 1)
        Return 1;
    Return N + sum(N-1)
}
```
EX 2.B Summing with a For Loop

```
Public static in sum(int N){
    Int total = 0;
    For(int i = 1; i <= N; i++)
        Total += i
    Return total;
}
```

To figure out recursive method find the pattern needed and write it down as the return line  
Then figure out when it needs to stop, add a base case  

In General recursion is a runtime method. C++ can do it at compile time if the initial value is known. Java cannot  
Every time a method is invoked a unique frame is created. It contains local variables and state.  
When that method returns, executions resumes in the calling frame. Each frame basically pauses until the next one down returns.  
Recursion creates frames from start to base case, once base case it hit, it closed those frames back up to the original call  
Multiple frames of the same method stacked on each other. Each Frame has different arguments   

This means recursion comes with a higher memory usage due to stack usage. Most recursive stuff can be re-written as a loop. Recursion is just easier in a lot of cases  
Converting to a loop might help speed but often not the case for bottlenecks. It can be premature optimization but also can be called for  

```
int divide (int A, int B){ // without divide operator only + and -
    If(A < B)
        Return 0;
    Return 1 + divide((A-B), B);
}
```

Recursion isn’t free, if there is a simple loop, use a simple loop instead.  
There is a lot more overhead in setting up new frames.  
Iteration of a for loop is a lot less expensive.  

```
public static int factorial(int N){ 
    int product = 1;
    for(int i=N; i > 0; i--)
        product *= I;
    return product;
}
```

Writing recursive methods seems like “magic”  
Assume it will work when you write it.  
This allows you to use it as part of your solution  

### Binary Search
1. Check if middle item is what we’re looking for. If so, return true
2. Else, figure out if item is in left half or right half
3. Invoke BinarySearch on that half

```
Int binarySearch(array, item, int start, int end){
    Mid = (start + end)/2
    If (start > end)
        Return -1 // item is not in array
    If(array[mid] == item)
        Return mid;
    Else if(array[mid] < item)
        Return binarySearch(array, item, start, mid-1) //we know mid isn’t the value sub one
    Else
        Return binarySearch(array, item, mid+1, end) // we know mid isn’t the value add one
}
```

Example initial innvocation:
```binarySearch(array, item, 0, array.length()-1);```

Often good to have a checker _**Driver**_ method to validate stuff.  
We want high-level to be binarySearch(array, item)

```
public static boolean binarySearch(array){
    if(array == null) // only check this once 
        return false;
    return binarySearchRecursive(array, 0, item, array.length-1);
}
```