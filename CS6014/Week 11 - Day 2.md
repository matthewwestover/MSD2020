## Week 11 - Day 2
### Return Oriented Programming
**W^X** Write or Execute
We talked about how W^X could protect us somewhat from buffer overflow exploits
If an attacker can overwrite memory with their shell code, those pages must be marked Writeable  
If we have W^X enabled, those writeable pages will NOT be marked executable. If they try to jump to code they’ve written, they’ll get a segfault because the OS will check the page permissions  
ROP subverts this protection because the attacker doesn’t write any shellcode! (they don’t write exec(“/bin/sh”) in a buffer they can overwrite)

This requires **Scavenging for Code**  
There’s a ton of executable code that is already in programs an attacker might want to exploit  
There is a surprising amount of code that an attacker could use to attack with, even though one would not expect it to be malicious.  
x86 is especially generous... because instructions are variable length, you might be able to find an unintended instruction by jumping to the middle of an existing one  
With ROP instead of writing shellcode to a buffer, we hunt through the executable and find the snippets of code we want, then carefully arrange the stack to jump to those snippets in the right order  
Because the “ret” instruction is a single byte (0xC3) we search through all the executable code in the binary for 0xC3, then we try to find all the snippets of assembly code we can produce by starting a few bytes before the 0xC3.  
This will include the actual instructions that were in the functions originally, but also (hopefully) some extras that we get by starting in the middle of the original instructions

Attackers search through all code and look for the RET instruction 0xC3 - Ret is a single byte  
Every function ends with a return. 0xC3 can also be parts of variables and other parts of code that aren’t “RET” in execution. But can become RET in this attack.   
Whenever a RET is found look back 1 byte and see if it is a legal instruction.   If it is, add it to the dictionary of useable code. Then look 2 bytes and see if those two bytes are useable, so on and so forth.  
These snippets are called **gadgets**

We run straight line gadget code by putting their return addresses in order on the stack (first on the top of the stack) then the execute a ret instruction to kick it off  
This is really hard to do by hand... so nobody does. The authors who first came up with this technique wrote a compiler to come up with the desired stack layout to call a chain of gadgets

This is just writing data to the stack, not injecting “new” code

### Mitigations
ASLR: If pointers change every time the program is run, it’s really hard to get the stack set up to execute what the attacker wants.   
Put attacker thwarting instructions before each ret instruction. Search your code for 0xC3 unintended bytes and replace with equivalent instructions.  Basically “Warning” signs that can defuse the gadget.  
“Control Flow Integrity” adds checks to make sure the stack is always the result of function calls that could have come from the source code. This is a heavy duty defense to include! This check would have prevented the buffer overflow exploit lab.   
Static Linking - cut out the extra library info not needed. This provides less RET options for an attacker to use. 

### Real World ROP
ROP is much harder than writing shellcode!  
So... use shellcode as much as possible  
We can still write shellcode to pages marked writeable  
Then, use ROP to call: ```mprotect(addressOfShellcode, executable)```
This changes the permissions of that page to be executable. It’s no longer writeable, but that’s fine, we already wrote what we needed.  
That will be a short-ish ROP chain, then we can write the bulk of the payload as “easy” shellcode and jump there at the end of our ROP chain.  
Attackers are relentless and only need to get lucky “once” 