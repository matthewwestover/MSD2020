## Week 4 - Day 2
### Access Modifiers
We don’t need to have access to all elements of an object  
Restrict elements from being interactive with things that don’t directly need to  

```
Struct{
public: // open and available everything by default is public in a struct
private: // restrictive to anything not related to the struct. Accessible only be methods directly linked to object
    int* pointer; // good one to have private. 
}
```

Methods are functions declared inside the struct that allow you to edit, read, the datafields in the struct

Get function as a method

```
Struct s{
Public: int get(int index); // don’t pass the object into the struct function/method
}
```

In .cpp file have to show function is related to the struct.

```
int s::get(int index){
    return (*this).ptr[index]; // (*this) is dereferencing what is on the other side of the dot
}
```

Format **Struct name::function name**

The parameter of the specific instance of the struct changes: call as s.get(3) // passes the current object ’s’ with getting the value at index 3. 

Function has a secret first param: s* this
It is not seen in the code, but is how we pass the object with the object name.function

(*this). Requires the ( ). There is a shorthand: ->  
Becomes:  
Return this->ptr[index]

Ptr[index] even more short hand. If undeclared name (ptr) it looks at the struct definition in the header and if it matches, uses that. This is the C++ norm. Does require using the same name of the field in the object as the name used in the function. 

All of this together lets us protect data. By setting data as private, users can’t access the data, but has to use the functions to access or modify (get, pushBack, etc).  

### Classes
Work exactly like a struct, but defaults to private, not public. 

```
Class c {
//private:

//Specify public: 
}
```

Still usually put public first, then private. Easier for readability. Public is the “documentation”  
No practical difference between struct and class  
Structs tend to “hint” toward a dump of data with few methods. Grouping of data
Classes tend to “hint” going to interact with a lot of methods.  
Really is a style choice.  

```
Int sum(const S& s){ // will not compile. Compiler assumes get and size will modify s. we need to reassure compiler we not change the object. 
    Int total = 0;
    For(int I = 0; I < s.size(); I++){
        Total += s.get(i);
    }
    return total;
}
```

Add const to end of method signature ```s::get(int index) const```  
Now the compiler knows we won’t attempt to change the object.  
Full embrace the const. If you try to pass a nonconst method into a const method it doesn’t work.  
Be consistent use anywhere you are NOT changing values. This will catch a lot of bugs  

### Constructors
Used when creating a new object of a struct or class  
This sets up an object to be used, but doesn’t return anything.  
Must have same name as the object in header  
Can have multiple constructors to depend on what parameters they have.  

Constructors should only provide reasonable ways to create an object  

```
Struct S {
S(int _x);
S(string _s);
// Both above work, but S(int _x, string _s) would be a bad idea as they could have different inputs that don’t match for the struct type

Private: // cannot pass in easily. They are inaccessible. Ss{3,”3”}; might work but could be messed up by whoever is making the object. 
    Int x;
    String s;
}

In .cpp file
S::S(int _x){
    x = _x;
    S = std::to_string(x);
}
```

To use

```
// S s; does not work. 
S s(3);
S s = S(3);
```

MyStructName ms;  
MyStructName ms{};  
MyStructName ms(1,2); // works passes in the 1 and 2 ints  
MyStructName ms{1,2}; // works like above, but better as doesn’t get confused  with a function.   
Can have objects create objects. Default constructors can be called simply, but ones with variables need to be done before the {}s.  
Initialization list is the pre bracket list of items to create.  
  
```    
Struct y{
Y(int z)
X x;
}

In .cpp
Y::Y(int z)
:X(z) // constructs x to be used in y. 
{

}

 Possible to have empty body constructors too 
S::S(string _s)
:X(stoi(_s)), S(_s)
{};
```
Can have full constructor or method in the header if simple (return x).