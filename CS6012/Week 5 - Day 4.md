## Week 5 - Day 4
### Interview Questions
#### Question 1
Print numbers from 1-100  
For multiples of 3 - print "Fizz"  
For multiples of 5 - print "Buzz"  
For multiples of 3 and 5 - print "FizzBuzz"  

```
for(int i = 1; i <= 100; i++) {
    if(i % 3 == 0 && i % 5 == 0) {
        System.out.println("FizzBuzz");
    } else if (i % 3 == 0) {
        System.out.println("Fizz");
    } else if (i % 5 == 0) {
        System.out.println("Buzz");
    } else {
        System.out.println(i);
    }
}
```

One liner:

```
System.out.println((i % 15 == 0) ? "FizzBuzz" : (i % 3 == 0) ? "Fizz" : (i % 5 == 0) ? "Buzz" : i);
```

#### Question 2
Using a recursive method generate the Nth Fibonacci number

```
public static int fib(int nthNumber) {
	if(nthNumber == 1 || nthNumber == 0) {
		return nthNumber;
	}
	return fib(nthNumber-1) + fib(nthNumber-2);
}
```
Complexity = 2^n  
Every time the function is called - two more are called  
(really more like 2^(n-1) but in big O it is O(2^N))

Can utilize Memoization to cut down complexity


#### Question 3
Fibonacci Sequence Memozation

```
public static long fibDriver(int nthNumber) {
	if(nthNumber == 0) {
		return 0;
	} else if (nthNumber == 1) {
		return 1;
	}
	long[] values = new long[nthNumber];
	Arrays.fill(values, -1);
	values[0] = 0;
	values[1] = 1;
	return fib(nthNumber, values);	
}
	
public static long fib(int nthNumber, long[] values) {
	if(values[nthNumber-1] == -1) {
		values[nthNumber-1] = fib(nthNumber - 1, values) + fib(nthNumber - 2, values);
	}
	return values[nthNumber-1];
}
```
Memorize values of certain calls
Store values as computed in an array  
If the Array is null call recursive function and add it to the array once computed
If array has the value, use that value. don't need to call recursive function again
Becomes O(N)

#### Question 4
Remove duplicate values from a linked list. 
Do this without a temporary buffer.

```
LinkedList<Integer> list = new LinkedList<Integer> ();
list.add(1);
list.add(3);
list.add(5);
list.add(2);
list.add(4);
list.add(2);
list.add(9);
list.add(7);
list.add(8);
list.add(9);
list.add(6);
list.add(10);
// Display current list - contains duplicate 2s and 9s
System.out.println(list);
for(int i = 0; i < list.size(); i++) {
	int current = list.get(i);
	for(int j = i+1; j < list.size(); j++) {
		if(current == list.get(j)) {
			list.removeLastOccurrence(current);
		}
	}
}
// Verify updated list is missing duplicate 2 and 9
System.out.println(list);
```

Space complexity with buffer: 2x  
Space complexity without buffer: 1x  
Time complexity with buffer: N  
Time complexity without buffer: O(N^2)  

#### Question 5
Design a stack that in addition to push() and pop()
also returns the minimum element via function min()  
push(), pop(), min() should be O(1) in time and O(N) space

```
public static class minStack {
	LinkedList<Integer> data = new LinkedList<Integer> ();
	LinkedList<Integer> minStack = new LinkedList<Integer> ();	Integer min = null;
	public void push(Integer n) {
		data.addFirst(n);
		if(min == null) {
			min = n;
		} else if(n < min) {
			min = n;
		}
		minStack.addLast(min);
	}
	public int pop() {
		int value = data.getFirst();
		data.removeFirstOccurrence(value);
		minStack.removeFirst();
		return value;
	}
	public int min() {
		return this.min;
	}
}
```

#### Question 6
Given a 20 GB file with one string per line, how do you sort it?  
How big is ram? Probably less than 20 GB - cannot read into an array list  

Read in what you can to memory, sort it, write to disk  
Do this for the whole file  
Then use merge on all the seperate sorted arrays  
Doing that efficiently is hard  

This *external merge sort* - doesn't just happen in memory  
Read 1GB of data into main memory, sort it in place using quicksort or Timsort, write sorted to disk  
Do this until we have 20 sorted chunks on disk  
Read first 1/21 GB of each sorted chunk into main memory (you need the extra space for output - holding result)  
Do a 20-way merge on the seperate 1/21 chunks from each sorted array chunk.  
 Store this result in output buffer.   
Once that, output the buffer into the final sorted file  
Do this 21 total times to get each section of all the chunks.  

