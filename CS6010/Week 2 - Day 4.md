## Week 2 - Day 4
### Sorting
Sorting is a fundamental component of programming. We end up with large amounts of data or variables, and we need to be able to sort through it to either find what we need, or to make it useable. 

Sorting is a difficult enough process to be interesting, but easy enough that all the components can be understood. There are multiple approaches to sorting and sorting optimization. Speed of sorting data is a big component of program speed. It allows for faster searching of data, or ability to find duplicates in the data for removal. It is very well studied. 

The goal is to turn 

| 4 | 3 | 2 | 5 | 1 |
|---|---|---|---|---|

into 

| 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|

Program sorting can be difficult as we can only compare two values at a time. There are multiple methods to do this:  
1) Swap adjacent pairs in a loop

* This is not efficent
* A common "first go" at sorting for programmers
* typically we shouldnt do this one
* Takes many loops (as many as there items to sort through) to ensure all items are in the correct location.

2) Put the smallest in front by swapping. Then shrink the number of items you are looking at. so first pass of 4,3,2,5,1 would become 1,3,2,5,4 and the next pass you look at just the 3,2,5,4 portion and do it again. 

This can be sped up by breaking the data into smaller sections to sort then sort comparing the two halfs. 

In either case the ability to swap to variables is key: 

```
void swap(int x, int y){
    int swap = x;
    x = y;
    y = swap;
}
```

Comparison Sorting Algorithms  
**Bubble Sorting**  
Compare adjacent pairs then swap as needed. no variable jumps location but slowly "bubbles" one direction or the other as needed. Best case senario is 51234 -> 12345, only takes one loop to complete. Worst case is having the smallest value at the far right, as it has to loop through as many times are there are positions to move (n-1). 52341 would take 4 loops to complete. 

```
for(int iter = 0; iter <v.size()-1; iter++){
    for(int I = 0; I < v.size()-1; I++){
        if (v[I] > v[I+1])
            swap(v[I], v[I+1]
    }
}
```

There is a marginally better version of the bubble sort called the cocktail sort method. You sort front to back, back to front, and front to back. Cuts down as you move variables one direction or the other. 

Total average times for the loop is ~n^2

**Selection Sort**  
This method is slightly better, only needing to loop the .size() amount of times to be complete. It uses two different functions to work. It finds the smallest value, then swaps it with the front end (v[0]) location. It then shrinks the amount of the dataset it is lookin at by 1 and repeats. This keeps the smallest in its place, then finds the next smallest, and so on. 

The minIndex function takes two parameters. a const reference of the list of data (vector of ints) and a int start position. The start position is passed from the sorting function to say only look from this position onwards. The minIndex then finds the smallest variable in the remaining bits of the datalist. 

```
int minIndex(const vector<int>& v, int start){
    int minloc = start;
    for(I = start + 1; I < v.size(); I++){
        ff(v[I] < v[minLoc])
            minLoc = i
    }
    return minLoc; 
}
```
The second function is the actual sorting/swap function. It takes a referenced list of data (so it can update the list). It loops through the list once, starting at the 0 position. Once the minIndex function has found the smallest value and returned that index location, it swaps the currently i value with whatever the smallest index location was. 

```
void selectionSort(vector<int>& v){
    for(I=0; I< v.size(); I++){
        int minIndex = minIndex(v, i);
        swap(v[i], v[minIndex]);
    }
}
```

### Persistent Storage
Just storing data in RAM causes us to lose it whenever we stop running the function. Using persistent storage allows us to input data and save data. 

This works much like the ```cout``` and ```cin``` functions. The simplest way to do this is with **file streams**. 

```#include <fstream>```

With fstream functions available. we then have to "open" the files we want to load data from. This uses ```std:ifstream```. 

```
std::ifstream myStream(“myFile.txt”); // Opens the file allowing data to be read
int readIntFromFile(ifstream& myStream){
    Int x;
    myStream >> x;
    return x; 
}
```

Alternatively: 

```
std::ifstream myStream(“myFile.txt”);
vector<string> data;
string words;
while(myStream >> words){
	data.push_back(words);
}
```

This data input will repeat as long as there is new words/ints in the file to pass in as a string. This means we do not need to know the amount of data in a file before hand and write a for loop specific to that amount. It reads in until there is nothing else to read in. 

To save data we need to use ```std::ofstream``` and specify the file name we want to save the data to. 

**Important**: Opening the output file stream deletes what ever is currently *in* that file. 

```
std::ofstream myOutputStream(“myFile.txt”);
myOutputStream << “this is text” << std::endl;
```

Alternatively:

```
std::ofstream outs(fileName);
    for(string word : encoded){
        outs << word << " ";
    }
```