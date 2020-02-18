## Week 1 - Day 3
### ISA + Assembly Code

Programs are made of instructions  
CPU executes one instruction every clock cycle  
Modern CPUs do more, but we ignore that.  

Specifying a program and its instructions:

* Lowest Level: machine language
* Intermediate Level: assembly language
* High Level: modern languages (Java, Python, etc)

High level compilers convert the code to assembly language for usage. It can be possible to see what the assembly language is post compile

High-level is easier to understand for humans. A single instruction might be many steps in assembly  

```
C = A + B 
```
Converted to assembly becomes

```
Load A into r1
Load B into r3
R1 + R3 into R2
Store R2 into C
```
Assembly is each individual single action. Load, Store, etc

```
Load r1, A
```
This gets converted to machine binary

#### Instruction Set Architecture (ISA)
Two types of ISA by complexity  
A very complicated one - **CISC**  
CISC = Complex Instruction Set Computer  
Variable instruction sizes in RAM  
Semantically more useful for usage for humans  

In the 80s **RISC** was introduced  
RISC = Reduced Instruction Set Computer  
Less possible instructions  
All instructions are stored on register  
All instructions are the same size  
Semantically more basic but faster/easier for CPUs to utilize. 
You could create more “powerful” instructions by combining several simple ones 

Both CISC and RISC are in use today, often in combination together  
To update/change between the two, CPU manufacturers had two choices:  
Break compatibility  
Decouple the processor architecture from the instruction set  

Basically come up with a standard in the industry, and the hardware can support it no matter how it changes

Modern CPUs (like x86 architectures from Intel and AMD) use a CISC Architecture  
They are complex, and more semantically useful  
RISC is faster however  
These CPUs can quickly convert from CISC to RISC instruction sets. 
Basically they are CISC but RISC under the hood. Truly RISC but allow for CISC instructions

Different CPUs use different instructions:  
Intel and AMD use x86 and x86-64 (desktops, servers)  
ARM (mobile, etc)  
PowerPC (old Macs)  
Many others including very specialized.  

OS doesn’t truest computer programs completely  
Most CPUs have different “modes” that the execute in  
OS’s use 2: 

* User mode (‘ring 3’ for x86
* Kernel mode (‘ring 0’)

If a user mode tries to do something not allowed, it triggers a processor exception and we run some kernel code

#### Typical Assembly Instructions
Load - load a value from RAM into a register  
Load Direct - put a fixed value into a specific register  
Store - copy value from specified register to RAM  
Add - add the contents of two registers and put result into a third register (sometimes can be contained to store in the register of one of the two used values)  
Compare - if the value in a specified register is larger than a second register, put 0 in a special register r0  
Jump - if the value in register r0 is ‘0’ change instruction pointer to the value register  
Branch - if the value in a specified register is larger than another, change the IP to a specific value  

There are two types of x86 syntaxes - **Intel** which is easier to understand, **AT&T** is more complex

##### Basic structure of an ASM Instruction
Mnemonic \<operands>  
Command, Destination, Source  
Example:

```
Add, edi, esi
imul edi, esi 
jmp loop
```
Common Instructions:

* Arithmetic: add, sub, imul, inc, dec
* Logical: and, or, xor, not, shl, shr
* Comparison: cmp - basically does a subtraction, checks the sign and sets values in a special “status” register
* Jumping: jmp <location> (unconditional jump), jne (jump not equal), jeq(jump equal), etc
* Conditional jumps examine the “status” register to see the results of the last comparison/arithmetic operation

##### Registers
Originally 8 general purpose registers in a 16 bit processor:

* Ax, bx, cx, dx (general purpose ones)
* Bp, sp, si, di (specific use ones)
	* Bp = base pointer - start of the stack frame
	* Sp = stack pointer - modified when things get “pushed” or “popped” from the stack
	* Si, di = source index and destination index usually memory accesses

We dont use 16 bit processors very often. We use different prefixes if we are in different bit versions  
64 bit uses r = rax, rbx, rcx, rdx  
32 bit uses e = eax, ebx, ecx, edx. 
8 bit uses l for low bit and h for high bit  

Switch to x86_86 added new registers, r8, r9, r10 … r15  
Xmm1, xmm2 … are for floating point stuff  

##### Memory
Accesses use [ ] many different ways to write them  
Ex: mov eax, DWORD PTR [rcx+8]  
DWORD PTR is called a size directive and indicates how much to read  
DWORD = double word. A word is a 16 bites. So DWORD is loading 32 bits of data from the address in the square brackets  
Above this is copying 32 bites from rcx+8 address to the eax register  

##### Immediates
These are numbers, decimal by default  
Usually hex with a 0x prefix  
Immediate values are part of the instruction sequence. The binary representation of the instruction must store the immediate value  
xor eax, eax = 0 outs eax bits  

##### Jumps and Labels
Label is just a name for a spot in code  
labelName: - put in code  
jmp labelName will jump to that location.  
This can be done in C and C++ even, but don’t it is bad  

C++ Code

```
int sum = 0;
for(int i = 0; i < 10; ++i){
    sum += I;
}
```

Assembly

```
xor eax, eax //int i = 0
xor ebx, ebx //int sum = 0
J1:
cmp eax, 9 // see if value at eax (i) is > 9
jg J2 // go to J2 if i > 9
add ebx, eax //add value at eax (i) to the value at ebx(sum)
Add eax, 1 //add 1 to eax (i)
jmp J1
J2:
```
