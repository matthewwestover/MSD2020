## Week 2 - Day 2
### ASM Recursion
```
// Recursive Method C++
int a(int b) {
    if (b <= 1){
        return b;
    }
    return b * a(b-1);
}

int main(int argc, char** argv) {
    int g = a(4);
    return g;
}

//ASM for Recursive Method
a(int):
    push rbp
    mov rbp, rsp
    sub rsp, 16 // Allots memory for one more call of a(int b)
    mov DWORD PTR [rbp-4], edi // store b here
    cmp DWORD PTR [rbp-4], 1 // compare b to 1
    jg .L2 // If cmp is false go to .L2
    mov eax, DWORD PTR [rbp-4]
    jmp .L3 // if cmp is true go to .L3

.L2:
    mov eax, DWORD PTR [rbp-4]
    sub eax, 1
    mov edi, eax
    call a(int)
    imul eax, DWORD PTR [rbp-4] // eax is the return value. This returns eax * b. 

.L3:
    leave
    ret // End recursive call to trigger going back up

main:
    push rbp
    mov rbp, rsp
    sub rsp, 32
    mov DWORD PTR [rbp-20], edi
    mov QWORD PTR [rbp-32], rsi
    mov edi, 4
    call a(int)
    mov DWORD PTR [rbp-4], eax
    mov eax, DWORD PTR [rbp-4]
    leave
    ret
```
### ASM System Calls
Users are stupid and can break shit  
Hackers/viruses are also users and can break shit  
User side programs need to “dangerous” things like I/o, create new processes, checking the time, etc.  
OS’s don’t trust user programs to have free access to the resources  
e.g. if a user incorrectly writes to a dictionary this can break the whole computer  

This led to the development of ASM System Calls  
This is a set of functions that user programs can use to request the kernel to do the dangerous thing in a safe way  
The API looks like a normal function, but the ABI/implementation is different  
User programs are run on the CPU in “user” mode which doesn’t allow things.  
Anything that is “dangerous” the CPU runs in kernel mode  
When a syscall is made, CPU switches to kernel mode, performs the task, then switches back to user mode when syscall is done.  

#### Syscall Implementation
Similar to normal function calls. ABI describes where parameters go, where the return value goes (registers, memory, etc)  
Cannot be the same because we MUST have CPU in kernel mode to run these  
We use: traps, exceptions, and interrupts.  
These are wakeup calls for the CPU  
Some are hardware based - like a key pressed (an interrupt) cause this to go off  
Some are error reports - accessing out of bounds memory, divide by 0  
Some are caused by user mode programs calling privileged instructions - halt (shut down) I/O type stuff, or a specific syscall instruction

#### Interrupt Vector
Every type of Interrupt has a number associated with it  
Some are hardware based, some OS based  
When we boot up the OS, we fill in an array (vector). Each entry stores a pointer to the code to run when that interrupt occurs. Valid interrupts are checked every time the OS starts up in this manner  
When an interrupt occurs, the CPU is switched to kernel mode, and jumps to the appropriate interrupt handler.  
The interrupt handler has a special return like instruction for setting the CPU back to user mode  
To make a syscall we set up the parameters we want, then trigger the interrupt that the OS knows for syscalls.  
X86_64 is uses the ‘syscall’ instruction. 

#### How to make a syscall
Put the parameters in the appropriate registers  
Put the sys call number in the appropriate registers  
Execute the syscall ASM instruction  
This switches CPU to kernel and execute the interrupt handler.  
Kernel mode runs the command, and returns to the next line  

Many methods in languages are wrappers for doing syscalls  
C++ has printf(“String”) which is a wrapper for a write call “write(1, “String\n”,  7)  
Write is the lower level c method that is a wrapper for a syscall
It becomes syscall(SYS_write,1, “String\n”, 7)  
SYS_write 1 is write out for Linux. SYS_write 4 is write out for MacOS