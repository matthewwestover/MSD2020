## Week 2 - Day 1
### Recursive Sorting
**Merge Sort**  
How to merge arrays  
We have two sorted arrays. We want to combine them into one bigger sorted list  
Compare the smallest out of both arrays and push it into the new array  
You don’t need to necessarily have to remove the item from the original array it came from, just move what index you are looking at.  
You move to the next item in the array that you took the item out.  
Merge Pseudocode:

```
void Merge(int[] arr, start, mid, end) {
  // create temp array for holding merged arr
  int[] temp = new int[end – start + 1];
  int i1 = 0, i2 = mid;
  while(i1 < mid && i2 < end){
put smaller of arr[i1], arr[i2] into temp }
copy anything left over from larger half to temp
  copy temp over to arr
}
```
Mid is more end of the first array, and end is the end of the second array  
This is O(N) -> more like O(N+M) where they are the sizes of the two arrays. You only loop over each array once  

Mergesort  
Cut the array in half. Sort the left and right arrays, then merge.  
You can split the left and right the same way, recursion!  
It was created by John von Neumann in 1945  
You keep splitting until the sorting is trivial - One item array is already sorted.  

```
void MergeSort(int[] arr, int start, int end) {
    // arrays of size 1 are already sorted 
    if(start >= end) { return; } // base case
    int mid = (start + end) /2; 
    MergeSort(arr, start, mid); // 
    left half MergeSort(arr, mid+1, end); // right half
    Merge(arr, start, mid, end); //merge halves 
}
```

This requires a temp extra array to store the merged arrays  
Mergesort needs 2x memory which can be bad if memory is restrictive or data is huge  
Size divides by 2 each time. This means there are log N levels of recursion  
The amount of work being done on the way “up” where the sorting occurs. That is merge (bottom up sort)  
There are log N levels to go through  
There are N comparisons per level  
This is a O(N log N) complexity  
This is best, average, worst case complexity. Always split, always merge  
If the array is already sorted, mergesort still needs to check, so it still takes O(N log N) to run  
This favors the left side branches. It goes down all the way then jumps up one level and back down  
On a single thread you will end up with the left half the array sorted before the right half does anything.  

**Divide and Conquer**  
*Divide* problem into smaller chunks to solve  
Solutions to the smaller chunks solve the full problem  
This is a key factor of recursive calls  
Typically a method calling more than one recursive method is a divide and conquer type   
The smaller chunks typically are disjointed and dont overlap with each other  

Mergesort on small arrays can be less than useful.  
N = 10, on meirgesort is 10 * log10 = 33  
Insertion sort is 10^2/4 = 25  
This means small arrays insertion sorts O(n^2) is faster  
To utilize this, once meirgesort reaches a certain split size, call insertion sort on those smaller datasets  
Do this instead of going down to an array of 1 each. If the data set is split  into two half of 10 items, call insertion sort on everything below that, and merge sort for the stuff above it.  
Timsort does this. When the dataset gets small enough it calls insertion sort.  Timsort is the default sort() in java and python  

The real threshold depends on several things:

* Hardware/OS/Compiler
* Input characteristics
* Original input size (may be fraction of input size),
* not constant

**Quick Sort**  
While mergesort splits down then merges up in order to sort, quick sort, sorts on the way down so by the time you are done splitting everything, items are all sorted  
This uses a *pivot*  
Pivot changes on each level. Everything less than pivot goes into one side, larger the other side.  

Move pivot to the end of the array, then compare everything before it in the array.  
When L and R, swap L and the pivot location  
This puts this pivot where it needs to be,  
This is partitioning.  
Repeating it on smaller chunks is how quick sorting works  
Partitioning swap pseudocode: 

```
find pivot, swap with right_bound; 
L = left_bound, R = right_bound - 1; 
while(L <= R) {
    if(arr[L] <= pivot)
        {L++; continue;} // find next item > pivot
    if(arr[R] >= pivot)
    {R--; continue;} // find its “swapping partner”
    swap(arr, L, R); // partners found, swap them
    L++; R--; }
// point where L met R is the pivot location swap(arr, L, right_bound); // put pivot back
```

Partitioning is O(N). You have to go through all the the array and compare one half to the other half.  
With a good pivot point, partitioning will cut the array in half. Recursively this becomes an average of O(log n)  
Quicksort average is O(N log N)  
Picking a bad pivot point (always picking the first or last time. One partion will contain 0 items, and the other will have all the remained.   
Worst case is O(N^2) as you only subtract one total from the size of the array.   
Better pivots can be just always pick the middle index in the array.  
You can still hit the worst cast in this scenari by bad luck. On average though it is slightly better  
Pick median, guarantees even partitions but finding the median costs O(N)  
Median of 3, sample first, mid, last values and get the median value. Can still lead to O(N^2)  
Random sample index values (3, 4, 5 values) and do the same to get the median value. Slightly more reliable to get closer to the “true” median”  
Random pivot point can fluctuate in speed from very fast to very slow.  

Any non-random pivot can hit a O(N^2)  
Quick sort pseudocode

```
void quicksort(int[] arr, int left, int right) { // arrays of size 1 are already sorted 
    if(left >= right)
        return;
    int pivot_index = partition(arr, left, right); 
    quicksort(arr, left, pivot_index-1); 
    quicksort(arr, pivot_index+1, right);
}
```

Efficiency of quick sort depends on the pivot point selection.  
Best case scenario: O(N log N)  
Worst case: O(N^2)  
Average is O(N log N)  
Proof for the average is complicated   

Mergesort and Quicksort are both average and best at O(N long N)  
But mergesort is slower and uses more memory.   
Quicksort doesn’t need more memory and uses inline swiping of values.  
BUT Quicksort’s worse cast is O(N^2)  
Pros/Cons of each  

Both of these are divide and conquer  
Mergesorts sorts on the way up after splitting  
Quicksort sorts on the way down before splitting  
Quicksort is generally more popular but not always best.  