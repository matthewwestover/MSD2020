## Week 3 - Day 3
### Bit Operators
We can only do actions on bytes  
To do something to a specific bit, we have to change handful of the bits while leaving others alone in the byte  
Things work on the whole byte, (all 32 bits)  
Actions for changing bits we used unsigned ints, as signed have the first digit as important for negative or positive. It does weird things for these storage types  
    **Use uint\*_t for variable types**  
Operate on bits with boolean operations   
    ```And, or, not, xor```  
Operate on each bit at the same time (up to 64) so all in parallel 


| "Logical" | Operator | "Bitwise" |
|:---------:|:--------:|:---------:|
|     &&    |    And   |     &     |
|     \|\|    |    Or    |     \|     |
|     !     |    Not   |     ~     |
|     !=    |    Xor   |     ^     |

**Uint4_T is not real, hypothetical examples of operators**:  
*uint4_t  x  = 15 1111*  
*uint4_t  y = 6 0110*  
*uint4_t z = 1 0001*  

*~x == 0 0000* flip all values  
*~y == 9 1001*  
*x&y == 6 0110* (1111/0110) return 1 if both are 1 or both are 0  
*y|z == 7 0111* (0110/0001) if 1 only return 1, if 0 return 0  
*x^y == 9 1001* (1111/0110) if they are different  

```
bool isBit3On(uint8_t x){ // 0000 0000 <- bit 3 in italics
    // x|0xF7 sets all other numbers to 1, if that bit is on, it will =0xFF
    return((x|0xF7) == 0xFF)
}
```

x|1 will set the bit to 1 no matter what  
x|0 will set the bit x no matter what (setting 0 in the spot will preserve what is in that spot)  
x&1 will set to x no matter what  
x&0 will set the bit to 0 no matter what  

Design a “mask” to change what you don’t care about and keep what you do to ensure you know what the outcome would be.  

### Shift Operators
We can “move” bits by shifting with << and >> 

```
0111 << 2 == 1100 // Fill in with 00s behind what Is shifted  
0000 1111 << 2 == 0011 1100 // 15 > 60 is multiplied by 4  
0000 1111 << 1 == 0001 1110 // 15 > 30 multiplied by 2  
```

Lets you shift the power by 2. // 2,4,16,32, etc.

```
bool isBit3On(uint8_t x){ // 0000 0000 <- bit 3 in italics
    Return((x << 4) >> 7) // bit to the far left then to the far right, makes it 1 or 0. If it is 1 return is true, 0 is false
    Return((x >> 3) & 1) // move bit to the far right, clears out everything else to 0. 
    return((x|0xF7) == 0xFF)
}
```

### Endianness
Larger numbers (34/64 bites) are stored as separate 4 or 8 bytes  
01/02/03 > jan 2, 2003, or feb 1, 2003  
How we interpret the data we get different results.  
This is true of multi byte data  
Int x = [d][c][b][a] <- stored in ram  

This is known as Endianness   
Things are either big or little endian  
Little endian = a comes first in memory  
Big endian = d comes first in memory. 

Most of the time we don’t care about what this order is.  
We care when care what individual bits are vs what the whole variable is  
We care when we decode something is sent by someone else. We want to save and load the file with the same order.  
If you try to construct binary order in the wrong order, you get the wrong result.  
There is a network standard order 
