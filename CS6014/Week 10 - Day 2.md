## Week 10 - Day 2
### Tools for Understanding Programs
There are multiple ways to practically do a buffer overflow exploit  
It is tricky in the real world, more so than one would think

We have a stack with an exploitable code below it. This code is a return address pointing to someplace else with the actual code.  Beneath that we have a buffer somewhere in memory. We have to figure out where the buffer starts and how much to overflow to hit that pointer for the code to execute

### Tool 1: Disassembly
Typically we don’t have access to the actual source code. More often we get access to the compiled program. This is machine code, so its not human readable.   It also contains multiple “sections” for static data (like string constants)  
There is a way to compile back up to assembly which still sucks but is easier to read than binary.  
We disassemble the compiled program to do this

On Mac the otool program can do this, on other *nix systems, objdump is avaiable

Commercial (and VERY EXPENSIVE!) tools such as IDA Pro provide this along with a huge suite of tools to help make sense of what you find (these tools are often used for reverse engineering). Ghidra is an open source tool from the NSA (lol) that’s apparently quite good.

Before we start disassembling, it’s useful to know functions are in our executable  
Things like variables and functions names are exposed as “symbols” (basically a name, pointer pair), that are part of the executable file  
These are necessary for the linker to do its job, and to make separately compiled libraries interoperable  
the nm command tells you which symbols are mentioned in an executable

```$nm -g shell```

This returns is a nightmare to look at. Names are mangled. We can “demangle” it

All the extra stuff around the function name is included to make function overloading easier for the linker  
We can demangle by using the c++filt tool:

```
$ c++filt __ZlsRNSt3__113basic_ostreamIcNS_11char_traitsIcEEEERK7Command
BECOMES
operator<<(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, Command const&)
```

operator<<(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, Command const&) = The symbol for operator << for an ostream and a Command struct  
We can take the mangled function name from nm and run it with otool -tV 
This returns the actual assembly. 

```
otool -tV -p __Z12testTokenizeRKNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEE shell
shell:
(__TEXT,__text) section
__Z12testTokenizeRKNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEE:
00000001000049f0 pushq %rbp
00000001000049f1 movq %rsp, %rbp
00000001000049f4 subq $0x150, %rsp
00000001000049fb leaq -0x100(%rbp), %rax
0000000100004a02 movq %rdi, -0xe8(%rbp)
0000000100004a09 movq -0xe8(%rbp), %rsi
0000000100004a10 movq %rax, %rdi
0000000100004a13 callq __Z8tokenizeRKNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEE ## tokenize(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&) 0000000100004a18 leaq -0x78(%rbp), %rax
```

What does it tell us?  
All the instructions in the function (if we wanted to trace through it, usually we don’t)  
Tells us the layout of the stack! We have to be a bit clever, but the function prologue allocates space on the stack (a sub 0xwhatever, %rsp instruction). Most local variables are referenced relative to rsp (or rbp)!  
It also tells us the address of each instruction, which can be useful if we want to get the code to jump somewhere

These tools will give you a deluge of information! Far more than you care about or need!  
In CS we’re almost always tasked with pruning away irrelevant information!  
Having the focus to know what you need to find out and locating it while ignoring everything else is vital!  
It shows you the code, but it’s really hard to make good use of it!  
It’s much easier to understand a program by inspecting it in action (your CPU will automatically “step through” the code for you.... by executing it)  
These tools also assume that the code is all inside the “text” section of the executable, and is read only. You can’t reliably disassemble code that is generated and executed at run time, or self-modifying code

### Tool 2: The Debugger
The debugger let’s you poke and prod a running program You can let it run to skip over parts you don’t care about You can look at disassembly  
You can inspect registers and memory  
You can insert breakpoints and watchpoints to pause the program when at a particular point in the code, or where a particular variable is accessed  
You’ve seen the debugger in XCode, but we haven’t talked about it in much detail We’ll discuss how to use it’s text based interface

#### LLDB 
Xcode runs this, also a command line version  
Breakpoints are a point in the code  
Watchpoint is when a point in memory is accessed

```
$ lldb myProgram
If I’m debugging a crash, I use the following pattern:
run //starts the program running...
// program crashes
backtrace (or bt... there are abbreviations for most things in lldb) This shows me the call stack showing how I got where I am
```

If you’re unlucky, such a backtrace will just have the addresses of the in- flight functions, without names or line numbers  
We can compile our programs to include “debug symbols” which contain this extra information mapping assembly to our original code!  
For clang and gcc, we add the -g option when we compile // gives function names.   
If we have optimizations turned on (-O2 or -O3 are compiler flags for clang), even if we have debug symbols, the mapping isn’t very good. The optimizations cause the generated code to be pretty different from what we wrote! If you plan to debug, compiling your code with -g -O0 is a good plan

#### Breakpoints
In XCode, you can set a breakpoint by clicking the left margin in the editor  
In the LLDB, we can also add breakpoints:  
```b functionName``` will add a breakpoint when we start the named function  
```b 0x141234afff``` will break when we get to the instruction at the given address.  
You can also set breakpoints at a given line of code (check the documentation for how to specify this)

```
`run [commandline args if any]` // Starts your program off. It will keep going until it hits a breakpoint or ends or crashes
```

If you hit a breakpoint then went to keep going, you can use the following commands:

```
continue (or just c) —keep running until another stop
s — single step a line of C code (possibly many instructions) 
thread step-inst — single step a single instruction
```

There’s a family of thread step-xyz functions. Run help thread to see all the options

When you’re stopped, it’s often useful to see what’s in your variables  
You can jump between stack frames with 

```
frame select <frame number>
```

This is useful if you end up in a stack frame for assert() or some other system function. You can navigate to your own code that caused the error with frame select  
frame variable will print the values of all local variables  
Note, that if you do bad things to memory, or have optimizations, this might be misleading!

### Lower level investigation
For exploiting an overflow, we want to get a bit lower level.  
`reg read` will print the value of all registers. Important registers are the RIP (the instruction pointer saying which instruction we’re on) and RSP/ RBP the stack and frame pointers  
the `x` command prints memory  
Example: `x -c 96 $rsp` will print 96 bytes of data starting at *rsp (basically this prints the top 96 bytes of the stack. RSP will be pointing at the lowest address of anything on the stack)
