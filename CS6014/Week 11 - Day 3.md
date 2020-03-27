## Week 11 - Day 3
### Side Channel Attacks
A side channel attack is based on the fact that algorithms are not abstract ideas: they are implemented in the physical world.  
Even with a perfect algorithm is that they run on CPUs with machine code.  
An attacker can measure physical properties such as elapsed time, power usage, or more exotic things such as sound or electromagnetic waves!  
Based on these measurements, an attacker can figure out secret information, such as passwords or keys!

### Timing Attacks
**Simple Example**

```
bool checkPassword(string realPassword, string userGuess){ // this is bad don’t store real password
if realPassword.length
    != userGuess.length: return false; 
for i = 0 to realPassword.length:
    if realPassword[i] != userGuess[i]: 
        return false; 
return true;
}
```

Algorithmically, this function doesn’t tell us anything except whether our password guess was right of wrong: it just returns a boolean  
By putting in different passwords and measuring physical responses they can work towards putting together the actual password

We attempt to log in. If our password is the wrong length, the function returns immediately. If it’s the right length, it will take slightly longer to return  
By sending in passwords of different lengths and measuring the time the function takes to run, we can determine the length of the correct password!  
Once we’ve discovered the length of the password, how can we determine the correct characters?  
A timing attack relies on the fact that the physical implementation of the algorithm leaks information (in this case the time it takes to run the function)

One way to help mitigate it:

``` bool checkPassword(string realPassword, string userGuess){ 
bool result = true;
if realPassword.length != userGuess.length: 
    result = false; 
for i = 0 to userGuess.length:
    if realPassword[i] != userGuess[i]: 
        result = false; 
return result;
}
```

Compilers are smart and might optimize this. Timing can still be different.   
Hashing the password is a good thing. Hash return length is always the same, so we don’t leak the password length, it also doesn’t tell anything about how many characters were correct. 

**Diffie-Hellman RSA Example**

```
BigInt fastExponentiationModN(x, k, n) //compute x^k % n 
accumulator = x;
for most-significant-one-bit down to 0:
accumulator *= accumulator
if(current bit is 1): 
    accumulator *= x 
return accumulator
```

All 1 bits slow this down so they can learn exactly how many bits there are being tweaked

### Power Analysis
Not a big deal for stuff over the internet, attackers can’t see local power usage.  
Not only do different operations take different amounts of time, they consume different amounts of power  
By monitoring the power usage of a processor while running, an attacker can infer what instructions are executing, which can leak secret data  
This generally requires physical access to the machine under attack (you need to hook a power meter or oscilloscope up to the machine, basically)  
Some instructions can have data dependent power usage, so even if you avoid branches, you might still leak information!  
We can try to be cognizant of these vulnerabilities during our software design, but the best solution is physical security! Note: if you’re making a DRM chip for a Blu-Ray player, or something similar that you’re going to distribute, you’re in for a tough time!

### Electromagnetic
Rather than measure power usage, we note that these devices generate electromagnetic waves when operating!  
We can measure these, and perform analysis quite similar to power analysis. 
Usually require the attacker to be close to the target, but less invasive than power analysis. 
Particularly scary for things like credit card chips and smartphones!

Closely related are **Acoustic Attacks**  
Same idea, different physical property  
Other circuit components on your motherboard emit ultrasonic acoustic signals (sounds at higher frequencies than humans can detect)  
Proof of concept extracted RSA keys by placing a cell phone next to a laptop!

There are software based vulnerabilities like this as well 

### Data Remanence
```
doEncryptionStuff:
    copy encryption keys to a buffer on the stack do some stuff
return

login:
    call doEncryptionStuff 
do more stuff
```

Do more stuff’s stack frame can access stuff from the stack. Normally we don’t just 0 out all data when done with it on the stack. 

Deallocating memory doesn’t erase or modify its contents!  
We’ve seen this for the assembly code for function prologues/epilogues: deallocating stack memory consists of ```add rsp 0xwhatever```  
We haven’t looked in detail, but heap deallocation is similar... the allocator just marks the space as reusable  
If our memory contains secrets, these secrets remain in RAM after deallocation  
If another program (or code in the same process) accesses that RAM, it can access secrets  
Other attacks are possible with physical access (“cold boot attacks” - dip ram in liquid nitrogen, bits remain on the stick to read)

Even “safe” delete isn’t really “safe” compiler will automatically skip commands that write to memory that are never read, or written twice in a row will skip to the second one. This is a “dead store” issue and very difficult to get around 

### Paging
When we covered paging, we noted that the OS gives us zeroed out pages when we ask for them to prevent this exact problem... Does that help? Sort of
malloc does not make this guarantee. If the malloc library gets a page from the OS, a user malloc’s it, then frees it, malloc might give that same chunk of memory to a future call to malloc!  
It shouldn’t be possible to read another process’s memory this way, but it is possible to exploit this within a process!  
We also need to be extra careful about sensitive data on pages that are swapped out to disk!

### Wrap-up
All of these attacks exploit a particular implementation of an algorithm, not the algorithm itself  
Defending against these attacks requires a bit more creativity to consider all the possible ways information might leak out of your system. 
These take advantage of some fundamental aspects of modern CPUs, so they appear to be very difficult to defend against and will likely haunt us for some time