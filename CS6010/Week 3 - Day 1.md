## Week 3 - Day 1
### Number Value
Numbers have multiple ways to be represented for interpretation by humans

* Four
* 4
* Quatro
* 100 (binary)
* \****

These are just representations of the number. \**** is the closest to what a pure number is, ie the actual amount.  
The variable type *int* is this mathematical ideal of what the number is.   

458 - We think of this as a string. it is an ascii representation of a 4, a 5, and an 8.  
It needs to be translated into the actual amount of items we want.  
This is really 4 x 100, 5 x 10, 8x1  
This is how base ten works. its a number * 10^position

### Binary
In Binary everything is a 1 or a 0.  
This creates long sequences for numbers very quickly.  
In base 10, 4 is just 4, but in binary it is 100.  

Binary is very useful in computer programming as it is a representation of something being on or off. Computers are really just large arrays of bytes, electrical switches that are on or off.  

To figure out what a binary number is, 0s are empty and 1s we multiply 2^postion. Postion starts at 0  
1010 = 1x2^3 + 0x2^2 + 1x2^1 = 8 + 2 = 10  
All odd numbers in binary end in a 1, all even numbers end in a 0

### Hexadecimal
This has a larger base than base 10, so numbers can be even shorter  
It is very closely related to binary however. Binary can be grouped into segments of 4 bytes. These can turn into a hex value very easily.  
Hexadecimal uses some letters to represent numbers  

```0-9, A = 10, B = 11, C = 12, D = 13, E = 14, F = 15```  

Binary 0000 = Hex 0  
Binary 1111 = Hex F  

Hex 121 = 1x16^0 + 2x16^1 + 1x16^3 = 1 + 32 + 256 = Dec 289  
Hex 12V = 11x16^0 + 2x16^1 + 1x16^2 = 11+32+256 = Dec 299  

**2^8 and 16^2 = 256**

Hex is really common for storing color values. 0-255 for R,G,B  
In decimal a color has more data, Hex works in 3 bits for a color  
In hexadecimal a color becomes FF FF FF  
In decimal to increase a value, you have to increase every value
(Color 123456789 = 234567890)  
Hex you can increase in one area and leave the rest alone (Color AA FF AA = AA FF AB)  

Because hex can be seen as 4 bits of data, converting between binary and hex is easy
1111 = F  
1100 = C  
Half of a byte is a nibble (4 bits)  

The computer assumes base 10 unless told otherwise. 

To specify a hex number **0x** prefix  
To specify a binary number **0b** prefix  
Compiler has a legacy number conversion. Leading 0 becomes base 8. 010 = 8 
This is old and rarely used for anything, but happens if you place leading '0'  
 
To convert a dec to binary or hex, it is best to start binary then move from binary to hex.  

247  
Break into 256 128 64 32 16 8 4 2 1 to represent the bit locations  
Subtract from total and mark as 1 as needed  
247 = 0b011110111  
Take binary in to 4 bit chunks and convert to hex  
0b011110111 = 0xF7  

Int is the literal “this many” so the representation of the data doesn’t matter  
Binary, Hex, Dec matter in converting from string to int (stoi or to_string)