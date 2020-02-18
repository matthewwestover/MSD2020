## Week 1 - Day 2
### CPU Architecture
Microprocessor - circuit of transistors and other electrical components on a chip that can process programs, remember information, or perform calculations  
This is an **integrated circuit**  
We currently have transistors of size ~10 nanometers in size which needs electron microscope to see. These are numbered in the billions on a single chip.  
This is the heart of every single CPU  
It requires programmed input  
It has advantages over a customized digital circuit: cheaper to make, scalable, single design-multiple use  
Customized Digital circuit would only handle video processing, or sound, etc.  
Currently the computer market is moving back toward customized digital circuits.   
See: GPUs - graphics specific. These have special chips specific for drawing triangles for gaming

Design objectives for a CPU:

* Maximize performance (minimize latency - time taken to complete a single instruction, and maximum throughput - number of instructions in a fixed amount of time. Usually instructions per second [IPS])
* Productivity - “easy” to program for the CPU
* Power usage - performance per watt (especially with mobile now)

A CPU Clock cycle (ex 3 gHz) how fast transistors can turn on an off to a reference oscillator 

Design constraints:

* Power consumed - todays processors peak at ~100W 
* Actual size of the CPU on the motherboard
* Cost
* Backward compatibility - things on a Intel P3 should run on a Intel P4
* Fast design and production time
* Security, Scalability, Reliability (SSR)

Different Markets: desktops, servers, embedded (like in appliances, cards, mobile devices) 

Most computers follow a Von Neumann Architecture.  
A CPU to handle processing, RAM for memory, and input/output devices to work with them

Modified Von Neumann Architecture includes: 

* A bus
* CPU
* RAM
* Keyboard
* Harddrive
* Display
* CD-Drive

The Bus is a way for many devices to communicate with each other, and the CPU and RAM.  
A “highway” for information  

CPU has a bunch of “registers” in a file or bank  
These are temporary memory  
Usually not a ton of registers, there is more cache  
CPU stores stuff from RAM on register, works on it, and puts it back  
All CPU operations HAVE to do be done on the register  

Basic operations are done by Arithmetic/Logic Unit (ALU). It only looks at the register  
There is a specific “instruction” register that it reads from to know what to do on the data in the other memory registers.  
The instruction register holds things like add, or multiply.  
Instruction pointer holds address of what the instruction  

A control unit coordinates communication between the instruction register, instruction pointer, ALU, and memory register.  

Control Unit Structure:  
1. Fetch: Tell RAM for the instruction stored in the instruction pointer  
2. Execute: There is only a finite number of possible instructions. Tell the ALU to do this specific thing.  
3. Repeat: Add 1 to the address in the instruction pointer (next memory point) and go back to step 1  
It will cycle between these three states until the program is done executing