## Week 1 - Day 4
### Basic Sorting
Sorting is a fundamental application in computing  
It is one of the most studied operations  
Most data is useless unless it is sorted somehow  

Shows us how to move data and the cost of moving it  
For any given problem, a specific goal isn’t necessarily sorting, but we need to sort efficiently to solve problems   

Easy to understand sorting algorithms run in quadratic time  
More complicated ones go to O(N log N)  
We can do better  

**Selection Sort** - The Simplest One  
Array has two parts, unsorted and sorted  
Go through unsorted find smallest and move it to the sorted (swaps and shrinks total unsorted size)  
Go find the next smallest in unsorted and move it to the end of the sorted
Complexity: O(N^2) for worst,average,best cases. It still has to loop through N^2 times even when its in order for the sub loop   
Doesn’t know it doesn’t have to do less work if array is already sorted  
There is no practical reason to ever use Selection Sort in a real program

**Insertion Sort** - Good for Small N  
Take first item in unsorted array  
Find where it needs to be in the sorted side of the array   
This shifts the unsorted and sorted arrays whenever the first time in the unsorted is moved  
This is kind of smarter as it can tell if array is already sorted   

```
void insertionSort(int[] arr) {
    for(int i=1; i < arr.length; i++) {
        index = arr[i]; // item to be inserted
        j = i;
        while(j>0 && arr[j-1]>index) {
            arr[j] = arr[j-1]; 
            j--;
        }
        arr[j] = index;
    } 
}
```

**Measuring Unsortedness**  
Determining average and worst cases  
An inversion is a part of array items that are out of order. They do not have to be consecutive.  
43, -3, 9, 75 < 45 is inversion with -3 and 9  
In worst case is all items are inverted, this is a O(N^2) issue for insertion sort  
Average case - About half of items are inverted. Unique pairs are N(N-1)/2. This is still O(N^2) 
Best case is array is already in order, O(N)  

Insertion Sort can be better than some O(N log N) algorithms for small N values.   
Any good quick sort checks N size, and does insertion sort when it is small. 

**Quadratic Sorting**  
An inversion is a pair of items out of order  
Sorted arrays have 0 inversions  
Average array has N^2 inversions  
Insertion removes 1 inversion per step  
To do better we need to do better than 1 inversion per step  

**Shellsort**  
It is like insertion sort but will move items back several spaces at a time  
Start with gap size = N/2  
Compare arr[0] with arr[N/2]  
Then arr[1] with arr[N/2+1]  
Swap them if they are out of order  

In one sweep more things are removed from being inverted  
Keep going with smaller gap sizes  
This starts looking like O(N log N)  
In reality it is more O(N^1.5)  
The gap size and shrinking is still being studied   
Originally was N/2, N/4, N/8, 2, 1  
Newer ones find it can be O(N^1.25)  
For moderate N (N < 100K) this is better than a O(N log N)  
Worst case is O(N^2) with Shell’s gaps, with other it is about O(N^3/2)  
Average is O(N^3/2) with shell’s gaps and O(N^5/4) for others  
These are incredibly difficult to verify.  
O(N^5/4) was only found from simulation  
Shellsort with gap of 1 is the same as insertion sort

**BubbleSort**
Very well known, but not super efficient  
Compare neighbors, if they aren’t in order, swap  
Repeat until sorted  
O(N^2) in all cases 
This is just a fancier but also not efficient version of Selection Sort

### Complexity Practice

```
For(i = a; i <= b; i++) {
    Print stuff
}
A < B
```
**O(b-a)** 

```
For(i = a; i <= b; i *= c) {
    Print stuff
}
A < B, C > 1
```

A c^k = b  
A = start, c = multiple times, k = unknown times, b = end  
c^k = B/A  
Logc(c^k) =logc(b/a) -> logcB - logcA  
K = logc(b/a)  
**O(logc(b/a)**

```
For(i = b, i >= a i /= c)
b>a
C > 1
```

B/C^K = A  
B = A*C^K  
B/A = C^K  
Logc(c^K) = logc(B/A)  
K = logC(B/A)  
**O(logc(b/a)**

```
For (i=1; I <=M; I *=2)
    For( j = 1; j <= n; j*=3)
```

N = 3^k  
M = 2^k  
O(Log3N * Log2M) -> O(LogN * LogM)  
If N ~ M -> (logN)^2  
If M >>> N -> LogM  

With a table

| I valuesPossible |   j valuesNumber  | J values for each I value |
|:----------------:|:-----------------:|:-------------------------:|
|         1        | 1, 3, 9, … N/3, N |           log3N           |
|         2        | 1, 3, 9, … N/3, N |           log3N           |
|         4        | 1, 3, 9, … N/3, N |           log3N           |
|         8        | 1, 3, 9, … N/3, N |           log3N           |
|        M/2       | 1, 3, 9, … N/3, N |           log3N           |
|         M        | 1, 3, 9, … N/3, N |           log3N           |

How many  in column 3. Count from I from 1-M  - log2M  
**O(Log2M*Log3N)**

```
For(i = 1l i <= N; i *= 2)
    For(j = 0; j < N; j++)
```
J loop = N  
I loop = Log2N  
O(log2N*N) = **O(N log N)**

| I values | J valuesNumber | J values for each I value |
|:--------:|:--------------:|:-------------------------:|
|     1    |  1, 2, 3, … N  |             N             |
|     2    |  1, 2, 3, … N  |             N             |
|     4    |  1, 2, 3, … N  |             N             |
|    N/2   |  1, 2, 3, … N  |             N             |
|     N    |  1, 2, 3, … N  |             N             |


```
for(i = 0; i < n; I++)
    for( j = N; j > I; j—)
```

| I values |    J valuesNumber   | J values for each I value |
|:--------:|:-------------------:|:-------------------------:|
|     0    | N, N-1, N-2, … 2, 1 |             N             |
|     1    | N, N-1, N-2, … 3, 2 |            N-1            |
|     2    | N, N-1, N-2, … 4, 3 |            N-2            |
|    N-2   |        N, N-1       |             2             |
|    N-1   |          N          |             1             |

Gauss’s trick N + n-1 + n-2 + … + 2 + 1 = N(N+1)/2  
(N^2+N)/2 = **O(N^2)**