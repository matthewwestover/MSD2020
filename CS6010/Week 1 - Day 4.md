## Week 1 - Day 4
### Strings
Strings are a complicated datatype  
```std::string```  with ```#include <string>``` in the header  
"String of Characters" : is a literal string - programmer lingo for text data  
```std::string s = "String of Characters"``` : is a variable string. Can be called as just ```s```  

You can do "string arithmetic" to add chars and strings together into new strings  

You can compare strings. Are they ```==``` or ```<``` ```>``` by dictionary order.  

Accessing characters in a string is done by index. 

```
string h = "hello";  
h[0] = 'h';  
h[4] = 'o';  
```

You can modify strings by changing the found chars index. 

```
h[0] = 'j';
h == "jello";
```

You can loop through a string.

```
For(int i=0, i < s.size(), i++){ 
	cout << s[i] << endl
}
```

Strings have multiple methods that can be used. 

* s.size() = length of the string (# of characters in the string)
* s.front() = first character in the string
* s.back() = last character in the string

s.back() is the same as s[s.size() - 1] due to the fencepost problem. Index start at 0.  

Methods can have additional parameters passed in the (). 

.substr() method - can return a handful of characters, starting at an index # to the end, or for a specified number of characters.  

```
std::string s = "hello";
s.substr(1) = "ello";
s.substr(1,3) = "ell";
```
There are standard library functions that change data types. 

* ```std::stoi(variable name)``` Turns to integer
* ```std::stod(variable name)``` Turns to double
* ```std::to_string(variable)``` Turns to a string