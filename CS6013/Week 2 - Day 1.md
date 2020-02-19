## Week 2 - Day 1
### ASM Function Calls

Assembly code utilizes the **C stack**  
This is memory that "grows down" from a base address. As more more memory is needed, the addresses get lower in value  
the %rsp register is the stack pointer

|   Bottom  |
|:---------:|
| Address 5 |
| Address 4 |
| Address 3 |
| Address 2 |
|    %rsp   |

There has to be a “rule” book - calling convention - **applications binary interface** (ABI)  
This dictates how parameters are added, where return values go, and what registers you can you  
Example ABI for x86_64  
First 6 int or pointer parameters go in rdi, rsi, rdx, rcx, r8, r9  
Floating point parameters go in xmm0 - xmm7  
Int return values go in rax  
RBX, RBP, R12-R15 are callee-saved “registers” if the function uses them, it must restore them before it returns  
Basically there is a limited fixed amount of registers, we need to know which can have values “saved” or not  
Other registers may be clobbered by function  
Other parameters are passed on the stack  
Some registers are temporaries - caller saved - register value may be changed to save the value it is on the thing using that register  
Some registers are preserved - callee saved - register value has to be the same after being used by the function  

Call compilers parses automatically into these registers

OS-Specific ABI defines:

* How arguments are passed to functions
* How results are returned from the function
* Which registers are preserved (or not)
* Other constraints - such as as stack alignment
	* (x86-64 Linux: stack aligned on call 8 mod 16 - starting address has to be a multiple of 8 or multiple of 16)
* Optional debugging protocols

#### Managing the Stack
Function must set up stack before running its body  
If it calls another function it has to call the stack space needed to run the other function  
Must clean up before it returns  
Set up is called “prologue”  

* Save the bp(base pointer) to the stack, then overwrite it with sp (stack pointer). BP is preserved, SP moves down
* Make room on stack for other function calls by subtracting from the sp address (grows downward)
* Copy the arguments into locations relative to the bp

Tear down is called “epilogue”

* Store return into rax
* Restore the base pointer so that when we return, the caller’s stack pointer is in the right place
* Pop rbp

Recursive functions slow down because it has to set up / tear down on each call   

#### Special Instructions
**call functionLabel ;** save the program counter (the special/secret IP register) to the stack, then jump to functionLabel  
**leave ;** restore rbp from the stack  
**ret ;** jump to the return address saved by call instruction and pop it off the stack  

```
int main(int argc, char** argv)
{return 0;}
//BECOMES
push rbp
mov rbp, rsp
mov DWORD PTR [rbp-4], edi //argv
mov QWORD PTR [rbp-16], rsi //argc
mov eax, 0 // This is line return 0;
pop rbp
ret
```

```
int a(int b) {
    return b+3;
}
int main(int argc, char** argv) {
    int g = a(2);
    return g;
}

a(int): // has its own local bp that gets restored back to main when done
push rbp // create the a(int stack frame)
mov rbp, rsp
mov DWORD PTR [rbp-4], edi // look at the value at edi (2) to its own stack area
mov eax, DWORD PTR [rbp-4] // 2 is moved to the eax register
add eax, 3 //add 3 to value in the eax register
pop rbp
ret

main:
push rbp // create main stack frame
mov rbp, rsp
sub rsp, 32 // making room on stack for a(2)
mov DWORD PTR [rbp-20], edi // argh
mov QWORD PTR [rbp-32], rsi // arg
mov edi, 2 // store the 2 from a(2) in the di register
call a(int) // use the lines from a(int)
mov DWORD PTR [rbp-4], eax // get the value at the eax register
mov eax, DWORD PTR [rbp-4] 
leave
ret
```