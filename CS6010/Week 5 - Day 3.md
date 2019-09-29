## Week 5 - Day 3
### Sequenced Containers
* contiguous storage
* Cache Friendliness (all in a row)
* Efficient iterators

Common sequenced containers are Vector and String  
Accessed by index [ ]  
All have to be stored by the same type  
Duplicate items are allowed  
Order of items matters  
Order is set by the user  
Hard to input something into the middle  

### Unsequenced Containers
Not all memory is sequenced, we don’t always want data stored in this manner.  

* We don’t always want to allow duplicates  
* Order might not matter  
* Don’t need to access by index
* Use brackets [ ] with something non integer
* Different methods of find and contains

The Phonebook example: <- Map  
Lookup by name to get phone #  
Possible duplicates on names, shared phone #  
Phonebook pb;  
Number = pb[“Matt Westover”] <- not an integer for index search  

Student List example: <- Set  
No duplicates  
“Membership” of class  
No need for bracket index searching  

#### Std::set main operations  
insert(T)  
Remove(T)  
Find(T) // more like contains  
size();  
Iteration;   
Set has no beginning or end  
Not contiguous storage  
Sorted but easily updated  
Insert/Remove and find take about log of size amount of time  

There is also std::unordered_set is “newer” in standard library. Normal set uses sorting to make things fast. Unordered uses const time to make fast. Unordered set in real world functionality is harder but faster

Sets keep track of “membership” - You are **in** or you are **out**

#### Std::Map main operations
Keys to values  
Map(k, v);  
Key is how you look stuff up, value is the return  
In phonebook key is name, value is phone number  
Searching based on keys is fast.  

Std::unsorted_map uses hashing to be fast. Std::map does sorted and range searching.  
Think of map as an array. Instead of a small int for index, you pass in something for key value. Can be a string, etc.  
Iterating and searching is a bit more complicated. We really want to get two things, key and value  

Map Searching - uses a struct to store key and value - it is called pair

```
For(auto pr :myMap){ //use auto since key and value can be different datatypes
    Key = pr.first
    Value = pr.second
}
```

```
Template <typename F, typename S>
Struct Pair{
    F first;
    S second;
}
```

Also can use find. 

```
Auto it = map.find(key) // iterator return  
If value is not present in map, it has a special iterator to indicate it  
It will == map.end();  
Auto it = map.find(key)  
if(it != map.end()){ //as long as it returns not the end() iterator  
    Key = it->first  
    Value = it->second  
}  
```