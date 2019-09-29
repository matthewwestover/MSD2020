## Week 5 - Day 2
### C++ Standard Library
We’ve been using it all along (std::)

It contains containers and algorithms

**Containers**

* Std::vector
* Std::array
* There are many more containers

**Algorithms**

* Std::sort
* Std::findMin
* Std::transform
* Lots of possible

Vector<int> v;  
Std::sort (v.begin(), v.end())  
Std::sort(v.begin() + v.size()/2, v.end() ) <- sort second half of the vector
Iterators work in half open ranges - can never dereference v.end();  
v.end = v[size] = this is out of range. End is +1 past last item in container  
Seen as (b, e] <- end not included. Ex for loops [int I, size).  
If b == e, it is an empty range

Find smallest element  
It = std::min_element(v.begin(), v.end())  
It is a pointer, needs to be dereferenced to return value cout<< *it << endl; 

A lot of algorithms take a callable  
Basically like passing a function as a parameter  
Can pass overloaded operator () on objects  

```
Bool byId(const S& a, const S& b)
{
    Return a.id < b.id;
}
```

Vector\<s> students
Std::min_element(students.begin(), students.end(), byId)->id - returns value of student.id that is the smallest amount. 

Callable examples: 
comparator: Bool(x, y) ~ x < y
Predicate: Bool(x) “is/has”
Unary: y f(x) returns a different output

Predicate example:

```
Vector<int> v{0,1,2,3,4,5}; / put evens in front odd in back
Bool isEven(int x){
    Return !(x &1) //true if even
}
```

Std::partition(v.begin(), v.end(), isEven) // not calling the function just naming partition decides when and where to call it It is just what partition will base order on. 
v = {0,2,4,1,3,5};  
Returns a iterator to where the change is in the vector. In this case at 1, as that is the first odd value  
Some of these algorithms do modify what is input, others require specifying where to put the new data.

```
Bool divisibleBy(int x, int d){ // tells us if a int is divisible by another another int.   
Return x%d == 0; 
}
```

This predicate can’t be passed into algorithms as it has two elements to be input. To update how it is passed to use in partition, we have to do more to it

Using a struct we can get around this. 

```
Struct DivisibleBy{
    divisibleBy(int d : divisor(d)
    Bool operator()(int x) const; {
        Return X % divisor == 0
    }
    Private:
        Int divisor;
}
```

Newer versions of c++ allow us to pass anonymous functions.
    That is a function without a name.
```
Std::partition(v.begin(), v.end(), [ ](int x){ return !(x&1);}); ```

This is anonymous or lambda function.  
Does the same thing as the bool isEven. 

Restyle this into:

```
Std::partition(v.begin(), v.end(), [ ](int x)
{ 
return !(x&1);
}); 
```

The divisor method. This breaks:  
Int divisor = 3;  
Std::partition(v.begin(), v.end(), [ ](int x){ return x%divisor == 0;}); 

Int divisor is not accessible inside the the lambda function. Inside the square brackets we can list new variables to use in the lambda function:  
Int divisor = 3;  
```Std::partition(v.begin(), v.end(), [divisor](int x){ return x%divisor == 0;});``` <- massive “one” line update to readability

```
Std::partition(v.begin(), v.end(), [divisor](int x){
return x%divisor == 0;}
);
```

Take a vector of ints, get a new vector of strings. 

```
Vector<int> I;  
Vector<string> s;  
s.resize(i.size()); // empty vector has no begin, it has to be initialized prior to putting the to_string value in. 
Transform(input start, input end, output start, unary operator)
Transform(i.begin(), i.end(), s.begin(), to_string);
```

Or:

```
Transform(i.begin(), i.end(), std::back_inserter(s), to_string); // do this without setting vector<string> s to a size. Needs to be empty. 
```

If using iterator operations use the auto function to get the correct datatype  
If you are storing a lambda function it HAS to be auto:  
Auto divisible by = ```[divisor](int x){return x%divisor == 0};```

### Git and Colloraboration
Repository

* contains commit history (snapshot of code)
    * Not human readable
    * Stored in .git
* Working directory
    * The files and code we actually change when programming
To record a “snapshot” of the working directory we commit added files we want save. 

Git pull to start merging updated  
Git fetch will hold the incoming repo for review on how to merge
