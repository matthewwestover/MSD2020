## Week 2 - Day 2
### Quick Sort and Merge Sort
We can divide an array in half log N times  
After dividing in half log N times - you have array of 1  
Best case run time of mergesort is O(N log N)  
Average run time of mergesort is O(N log N) 
Worst case is also O(N log N)  
No matter what it has to split and compare  
MergeSort when there is a small set of data it can be better to use insertion sort  
If N < value do insertion sort  
Quicksort best case is O(N log N)  
Average case is O(N log N)  
Worst case is O(N^2)  

When would you pick one over the other:  

* Quicksort is done inside its current array
* Mergesort needs a more memory to do the sorting. If you can’t afford that memory space you can’t use it
* Quicksort might run out of access as it is done inline. The entire array has to be in ram for it to work. If some is in disk it has to pull from that, and slows the process down

We can randomize the order of the already existing list.  
Reordering a list is called permuting.  
Useful for testing  
Also useful for quick sort as it makes it better for random selection to find a median for pivoting  

```
for(int i=0; i < arr.length; i++)
  swap(arr, i, rand.nextInt(arr.length());
```

```ArrayList.ensureCapacity``` just allocates the memory, doesn’t actually set memory values  
Don’t set temp array to store and add all items from the arr inside the merge/recursive calls. 

```
void merge(ArrayList<T> arr,
Comparator<? super T> cmp,
int left, int mid, int right)
{

ArrayList<T> temp = new ArrayList<T>();
while(i < mid && j < right) {
  if(cmp.compare(arr.get(i), arr.get(j)) < 0)
    temp.add(arr.get(i++)); 
```

This isn’t optimal
Best option to pass temp into the merge driver method

```
void merge(ArrayList<T> arr,
ArrayList<T> temp, int left, int right, Comparator<? super T> cmp)
{
int i = left, j = midPoint, k = left;

while(i < midPoint && j < right) {
  if(cmp.compare(arr.get(i), arr.get(j)) < 0)
temp.set(k++, arr.get(i++)); else
    temp.set(k++, arr.get(j++));
... finish implementation of merge }
```
So while ensureCapacity is more complicated, it avoids the cost of growing the array associated with it. By presetting the size we remove that and can better test efficiency. 

Data graphs, log both sides, slope becomes complexity

|                |    Best    |   Average  |    Worst   |                   Notes                  |
|:--------------:|:----------:|:----------:|:----------:|:----------------------------------------:|
| Selection Sort |   O(N^2)   |   O(N^2)   |   O(N^2)   |          Never Used in Practice          |
| Insertion Sort |    O(N)    |   O(N^2)   |   O(N^2)   |       Takes Advantage of Sortedness      |
|   Shell Sort   | O(N log N) |  O(N^1.25) |  O(N^1.5)  |          Depends on the Gap Size         |
|   Merge Sort   | O(N log N) | O(N log N) | O(N log N) | 2x space overhead  guaranteed O(N log N) |
|   Quick Sort   | O(N log N) | O(N log N) |   O(N^2)   |             Depends on Pivot             |