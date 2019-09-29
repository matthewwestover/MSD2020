## Week 4 - Day 3
### Operator Overloading 
Matter of convenience. 
For fractions assignment when we added fractions we did ```f1.plus(f2)```  
We’d prefer to write ```f1 + f2```  
Overloading is a programming term for allowing multiple uses for the same terminology. 
.to_string is overloaded. We can convert ints, floats, char*, which all require different actions to convert to a string.  

Easy to overload, just write a special weird function to allow it. We can overload mostly any operator. 

```
Fraction operator +(Fraction lhs, Fraction rhs){
    Return lhs.plus(rhs); 
}
```

Can’t just copy and paste code from plus function into this. Fields are private in the class.  
They keep order of operation. Just add functionality. 

To allow access to private data types: turn into a method, or add as a friend function

As a method:

```
Class Fraction{
Public:
    Fraction operator +(Fraction rhs); // lhs becomes “this” thus changes the signature
}
```

As a “friend function”:

```
Class Fraction{
friend Fraction operator +(Fraction lhs, Fraction rhs); // keep separate function code as above. 
}
```

Don’t use both, do either or. Method is generally more common, but friend is valid. Sometimes we can’t make something a method

Ex cout:
Method - doesn’t work. Needs to become cout.operator <<(myFrac)
<< is part of cout and can’t be called this way.

As a function however it can work

```
Operator <<(ostream outs, Fraction f){
    Outs << f.toString();
    Return outs; 
}
```

If we do cout << myFrac << “/n”  
The cout << myFrac needs to return cout so the << “/n” works  
Correct function signature is ostream& operator<<(ostream&, Fraction f)  
This is called chaining. We avoid copying output and return it. Allows you to repeat actions and string them together.  

Overloading should return “this” that is the left hand side of the operator.  Return a fraction, return an ostream, etc. 

With the addition example sum+= fraction doesn’t work, only sum = sum + fraction.  
+= is kept separate to be overloaded. 

```
Fraction& Fraction::operator +=(Fraction rhs){ // lhs is *this have to return the reference to what was changed
    Fraction result = plus(rhs); //this is implied. Can be result = this->plus(rhs);
    *this = result; // just this is a pointer to the fraction, need to dereference with *
    return *this;
}
```

Commonly the += is implemented in this manner and + is different using the += from the public functions of of the function: 

```
Fraction operator +(Fraction lhs, Fraction rhs){
    lhs += rhs    
    Return lhs;
}
```

There are additional operators that can be overloaded. 

```
Class string{
   char Operator[](int index) const{
        Return data[index]
    }

    Char *data;
}
```

s[0] = toUpper(s[0]); // this makes a char on the left, we can’t “change” chars. It needs to be something we can update
s[0] needs to be a char&

Write identical function to the [] function but no const

```
Char& operator [](int index){
     Return data[index];
}
```

Operator overloading helps readability a lot. Super convenient 

### Rule of Three

Destructor functions are “special” that clear out created classes and structs
Put clean up code in destructor to prevent memory leak
```~ClassName();```

In .cpp

```
MyString::~MyString(){
    delete [] data;
}
```

If you delete a thing of data and have another thing equal it, it will point to the same thing, and will break system with the originals pointer gone

Use a copy constructor, that allows you to make a “new” copy of the data
MyString(const MyString& other);

In .cpp

```
MyString::MyString(const MyString& other)
:MyString(other.data)
{}
```

When not creating a new variable, but assigning it equal to another one (s3 = s2) create an = overload

```MyString& operator=(MyString rhs);```

In .cpp

```
MyString& MyString::operator=(const MyString& rhs){
    std::swap(data. rhs.data);
    std::swap(size, rhs.size);
    
    return *this;
}
```

If you use one of these, create ALL of these.  
**This is the rule of three**. 

Operator = can be optimized

```
MyString& MyString::operator=(const MyString& rhs){
    if(rhs.size > size){
        char* newptr = new char[rhs.size + 1];
        delete [] data;
        data = newptr;
    }
    size = rhs.size;
    for(int i = 0; i < size; i++){
        data[i] = rhs.data[i];
    }
    data[size] = '\0';
    
    return *this;
}
```

With all 3 we no longer have to worry about the memory wherever we use it. Always makes real copies, and always clears out data once done. 
