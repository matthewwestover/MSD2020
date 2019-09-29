## Week 3 - Day 2
### Representing Numbers
Ints are stored on a computer as a binary bytes, but are to be thought of as the mathematical ideal.  
This is a difference that is important

Computers store information in bytes.
Ex: int x = 15

The memory stack of 32 bits has 28 0s with 1111 at the end  
This is how positive numbers are stored, up to around 4 million.  
If we are curious what a number looks like in memory, just convert to binary and that’s what it looks like.  
But we still think of it as the mathematical idea of how many items there are

Super large numbers can’t be represented, as well as negative numbers
Large numbers are broken up into different sections.

### Negative Numbers
1's Compliment - first bit 1/0 is denoted as a +/-  
This is more complicated  
There is +0 and -0  
Rarely used in modern computing hardware  

Most commonly used is **2’s Compliment**  
Works out the numbers as everything is positive, then at the end evaluates negativity  
Binary mathematics doesn’t care, can math together numbers then do this at the end  
Positive numbers have a 0 in the "most significant bit" (left most)  
Negative numbers have a 1 in the MSB  

There is an overflow in math, that is truncated which is how this works. (Limited in number of bits)  
Think of MSB location as 2^power of that location but with a negative
```1110 > -8 + 4 + 2 + 0 = -2```
    
Trailing powers always have a smaller value regardless of the size of the number. All 1s would result in -1.  

```1111 = -8 + 4 + 2 + 1 = -1```

```10000011 = -128 + 2 + 1 = -125```

*10010011*  
*01011001*+  
**11101100**

How do we tell if a number is supposed to be negative or not
Is 1111 = 15 or -1  
Unsigned vs signed numbers. 
Unsigned number = assume first digit is positive - unsigned int x. Cannot be positive  
Signed bit = assume first digit is negative - This is default int x - can be positive or negative  

This is signed representation range for 4 bits  

* 0111 = 7  
* 1000 = -8. 
Cannot represent positive 8 in this example  

In unsigned range for 4 bits  

* 0000 = 0
* 1111 = 15

### Ascii
Letters are stored as numbers  
Anything not “English” characters Is not supported 256 characters only  
Unicode is how ascii was expanded to include pretty much all characters  
We have to use a different datatype than we do with ascii to access the full table  
With a 32 bit int we can access any character in the unicode table (might change later as unicode grows)  
Can be wasteful if most of what you use is the latin characters  
Codes are stored UTF-8  
“Strings of code” that return unicode characters  
“Hello 😀”  
‘H’ ‘e’ ‘l’ ’l’ ‘o’ ‘ ‘ marker 'smiley face number’  
Unicode assumes we mostly want ascii values  