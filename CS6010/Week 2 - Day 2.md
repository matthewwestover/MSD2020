## Week 2 - Day 2
### Structures
Structures (structs) allow you to create new variable datatypes in C++  
This gets around the current limits of the built in types (int, bool, string, float, etc.)  
Struct definitions go in the header (.hpp) file  

```struct name{type1 name1; type2 name2, ...};```

Only C++ instance of requiring a semicolon after the closing } bracket  
*name* is the new datatype with a list of data variables included in the brackets  
These can be mixed data types:

```struct student{string name; int age; unassigned long UNID; vector<courseInfo> credits};```

To call this as a variable ```student s;``` this is an empty struct  
```student s2{"Matt", 123456, "email@utah.edu"}``` this is a second struct with info in it.  
can also assign as sub parts of the struct name ```s2.name = "Matt";```  
Objects can be called by name like this as well  
This takes multiple variables and groups them together, easier to pass into functions, call all data at once, keep things as a single variable instead multiple you need to remember to include  
structs can be nested just like vectors can  

### Review
* variables - used for storing data, "remembering" information
* if statements - used for making decisions
* loops - used for repeating bits of code quickly
	* continue - skips to start of loop again
	* break - stops the loop
* functions - reusable code, library code
	* should be returned as soon as possible
	* keep short and simple
* methods - complex library code for strings and vectors (.method())
* strings - storing and manipulating text
* vectors - storing and manipulating lists of data
* structs - grouping data together
* Arithmetic - ! Is not; == is equal comparison (!= not equals to)
	* = is assignment not comparison

These are the basic fundamentals of all programs

Boolean pattern - think of as small range ints

```
bool isSpace(char c){
    If(c==‘ ‘){
    Return true;
    } else {
    Return false;
    }
}
```
Re-write into

```
bool isSpace(char c){
    Return c==‘ ‘;
}
```

```
bool isConsonant(char c){
    If(isVowel(c) || isSpace(c) || isPunctuation(c)){
        Return false;
    } else {
    Return true;
    }
}
```

Re-write into

```
Return !(isVowel(c) || isSpace(c) || isPunctuation(c));
```

```If(isConsonant(c) == true){}``` is meaningless. Really just ```if(isConsonant(c)){}```
```If(isConsonant(c) == false){}``` is really just ```if(!isConsonant(c)){}```
