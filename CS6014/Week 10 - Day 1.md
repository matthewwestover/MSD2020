## Week 10 - Day 1
### Buffer Overflows
The most dangerous function in the world

```
void sayHello(){
    char name[10]; gets(name);
    printf(“hello %s”, name);
}
```

Gets() is bad. If the input is too long (in this case over 9 characters)   
The manpage for get() literally says not to use the gets method  
It is limited to 9 characters due to an input null space terminator for input  
Because this isn’t a “new” array it goes on the stack for memory  
Going further extends past the stack. If it writes too far it overrides the return address  
This means it will return to somewhere else in the code which malicious input can mess with

Overrun the buffer and overwrite the return address to be somewhere else  
Possibly another function that was not intended to be called  
Possibly fill the buffer in with malicious code, and return to THAT!  

If the attacker can get execute a “remote code exploit” they commonly start up a shell. The evil payload is called “shellcode”

The input they provide to the vulnerable function is the assembly code version of ```execve(“/bin/sh”, [“/bin/sh”], NULL);```

```0x68732f6e69622f2f = /bin/sh```  
This lets the attacker do whatever they want once the shell is open  
Often users are NOT supposed to be able to run arbitrary commands, a shell lets you do that  
Some programs use “setuid” meaning that they run as a more privileged user (eg sudo). If you exec a shell from one of these, it retains the elevated permissions! known as “privilege elevation vulnerabilities”

### Programmer Prevention
Use safer APIs if possible: cin >> name is NOT vulnerable.  
Most string functions in C have variants that take max lengths that when used correctly are not vulnerable to these issues. It’s VERY easy to make mistakes.  
During development (and maybe even deployment!) turn on Asan and UBsan and use a thorough test suite

### OS/Compiler mitigations
Programmers are stupid and forget stuff, OS and compiler has built in safeguards to help

* Stack Canaries
    * Adds special values near the return address on the stack that are checked before returning
* Make stacks grow up (for, I guess? historical/inertia reasons, no one does this!). Reduces the number of possible chances to overwrite (though does not eliminate them). This isn’t done because it is just TOO much to change the current set up
* Address Space Randomization (ASLR) give everything new virtual addresses each time you run the program. It’s much easier for an attacker if they know addresses of things in advance! Stack grows contagiously but the starting point is different every time the process is run
* Separate the code stack and the data stack (also not too common AFAIK). One stack stores return addresses and is far away from the data stack in memory
* W^X (write xor execute). Any memory page that is writable is not executable (stacks are writable). This means that the hacker can’t place their shellcode in the stack buffer they’re overflowing
    * This causes a segfault if an attacker writes an exploit to memory as it cannot be executed as well
* BSD “pledge” — Special system call programs can make to remove privileges for themselves. Before a webserver starts serving, it can remove any permissions it doesn’t need to reduce the severity of any bugs!