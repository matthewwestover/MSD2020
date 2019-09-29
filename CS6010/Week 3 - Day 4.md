## Week 3 - Day 4
### Assembly and Machine Code 
C++ is a “high level” programing language  
We have source code that turns into an .o file  
.o file is machine code aka “low level” programing  
C++ is kind of abstract. Compiler is complicated and smart to convert to the machine code 

Before High Level Languages people wrote in assembly, which is more human readable than machine code, but still very basic  
Assembly is easily translated to machine code  
Does not have a lot of functions like loops  
Command ‘xxd’ program files shows everything with hex codes which is machine code functionality   

Compilers use to work in two steps. Create assembly code then create the machine code. Compiles to assembly, assembles to machine code

Assembly is **NOT** what the CPU understands. It is a bridge between human and computer readability

Text based is larger with storage while machine code uses less bits    
Text has to compare to list of instructions while machine code has each instruction assigned to a number so it can return faster   

Adding in assembly: add $r1 $r2 // $r1 += $r2 in c++ $r1 and $r2 are called registers  
Registers work really fast


Addi rax, 6 // rax is register and 6 and is immediate. It is loaded in not in memory  
Instruction set architecture aka ISA  
What instructions we can do on this chip  
Common is Intel x86. Like a 2000 page manual for what every instruction is available  
ARM is also common as found in phones, tablets, other small electronics 
*Advance RISC Machine*
*Reduced Instruction Set Computer* < prettier and efficient
*MIPS* > from Berkley for instruction set, not common but sometimes found in network based electronics  

j (Jump) startOfLoop command to go to a section of code LABELED startOfLoop  
Bne $r1 $r2 label: branch not equal. If $r1 and $r2 do not equal jump to this labeled location  
Addi is add immediate  
Load $r1 $r2; $r2 is a pointer in memory that sets the value there at $r1  

Compilers split into front end {intermediate representation} back end  
Intermediate representation is kind of like assembly level. Backend takes that and converts into the cpu instructions. Can change based on chip type  
Writing a new language you really just need to figure out how to turn into intermediate  and just reuse backend  
Example rust uses same backend as c++

Each operands gets an opcode, a number assigned in the cpu to operate the instruction  

Depending on CPU there is a different amount of opcodes  
Can smoosh together things like the opcode and registers  
24 commands needs 5 bits, 8 registers need 3 bits. Can combine into a single byte  
*This is why being able to change bits specifically is useful*