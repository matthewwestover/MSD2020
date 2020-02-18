## Week 1 - Day 1
### Operating System
The Operating System (OS) is just another program  
It is hard to define as it is a process between user and hardware  
The OS arose from a set of historical computing problems

#### Prehistory - (Before 1945)
Charles Babbage (1792-1871) & Ada, Countess of Lovelace (1815 - 1852)  
Babbage: 1st computer architect  
Ada: 1st computer programmer - Her father was Lord Byron  
First “digital” computer  
Gears and crankshafts - analytic engine  
Babbage never got his Babbage analytical engine to work, it has been created and functional recently however  
No operating system, programmer programmed to raw hardware  

#### Phase I - (Early 1950s)
Hardware is very **expensive**, humans are **cheap**  
One user at a console using punchcards  
One function can be done at a time. No overlap between computation and IO  
User has to debug at the console  

First OSes: common library routines (don’t need punchcards)  
Example: IBM Model 701 - The defense calculator  
Uses vacuum tubes (72 of them)  
18 bit codes  
Very specific low level coding  

Batch processing: load, run, print, dump, repeat

* Put up glass walls around computer
* Users give program (cards or tape) to human who schedules jobs (a dude in a booth gets a tape of instruction to feed to the computer)
* OS loads, runs, and dumps user jobs
* Non-interactive batch processing - program either works or it doesn’t 
* Efficient use of HW
* Debugging hard, core dumps
* Short jobs starve behind large ones (can’t prioritize. Got a tape/card. Run tape/card)

Once the metal drum was added for memory storage, General Motors (GM) wrote the first real operating system

Data channels and interrupts

* Buffering and interrupt handling in OS (“batch monitor”)
* Spooling (SPOOL: Simultaneous Peripheral Operation OnLine)
* No protection – one job running at a time!
* Improves performance by running computation and IO in parallel

This splits input, computation, output. Can read a file while letting something read. 
There is still just one program running at a time

Early 1960s - IBM 7094 - 50,000 transistors instead of vacuum tubes (today we have billions of transistors)  
Still the size of rooms. Expensive, used by NASA and the Military. Not available for consumers.  

Multiprogramming:

* More and more memory available – can load several jobs at once (RAM doesn’t exist, kB amounts of memory)
* OS (monitor) always resident to coordinate activities
* OS manages interactions between concurrent jobs:
    * Runs programs until they block due to I/O
    * Decides which blocked jobs to resume when CPU freed
    * Protects each job’s memory from other jobs
* Example: IBM S/360 (designed by Amdahl) combined jobs of IBM 1401’s (Tape/Card Reader) and IBM 7094 (What ran the program) – First machine to use ICs (Integrated Circuit) instead of individual transistors

#### Phase II (1965-1980s)
Hardware is **cheap**, humans are **expensive**  

Timesharing: 

* First time-share system: CTSS from MIT (1962)
* Timer interrupts: enable OS to take control (pre-emptive multitasking)
* MIT/Bell Labs/GE collaboration led to MULTICS (Multiplexed Information and Computing Service)
    * Envisioned one huge machine for all of Boston (!!!)
    * Started in 1963, “done” in 1969, dead shortly thereafter
    * Bell Labs bailed on project, GE bailed on computers!
    * While it was dropped, a lot of UNIX was based on what was developed here
* DEC PDP (Programmer Data Processor) minicomputers: start of bottom feeding frenzy
    * PDP-1 in 1961 (4K 18-bit words, $120,000)
    * Kernighan dubbed OS “UNICS” to poke fun at Ken Thompson
    * “C” language developed for Unix (ancestor was “B”)
    * Guiding principle of UNI(X): Keep it simple so it can be built
* Terminals are cheap
    * Instead of 5 rooms with one person/computer you have 1 room with 5 computers and 5 people
    * Let all users interact with the system at once
    * Debugging gets a lot easier
    * Process switching occurs much more frequently
* New OS services:
    * Shell to accept interactive commands
    * File system to store data persistently
    * Virtual memory to allow multiple programs to be resident
* New problems: response time and thrashing
    * Need to limit number of simultaneous processes or you can fall off performance cliff (“login”)

#### Phase III (1980s-2000s)

* Personal computing: every “terminal” has computer
    * One user per machine, since machines are no longer scarce
    * Initial PC OSes similar to old batch systems (w/ TSR hacks)
    * Advanced OS features crept back in!

#### Phase IV - (Present)
* Lots and lots of computers per person
* Embedded systems
    * Cars commonly have 50+ processors
    * Airplanes, factories, power plants run a huge amount of software
    * Internet of (Insecure, Compromised) Things
    * “Why Software is Eating the World”
    * Some embedded systems run a “real OS,” others run an RTOS, still others run a program on the bare metal
* Mobile computing
    * Smart phones and tablets
    * PCs and laptops are mostly power tools for content producers
* Cloud computing
    * Buying and administering hardware sucks, maybe most of us should avoid doing that
    * Virtualized compute resources are flexibly allocated on demand Computing as a service rather than a product