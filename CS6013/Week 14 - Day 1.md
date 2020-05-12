## Week 14 - Day 1
### I/O Devices
Filesystem Layers  
User’s viewpoint:

* Objects: Files, directories, bytes
* Operations: Create, read, write, delete, rename, move, seek, set attributes

Physical viewpoint:

* Objects: Sectors, tracks, disks
* Operations: Seek, read block, write block

User ←→ OS layer

* User library hides many details
* OS can directly read/write user data

OS ←→ Hardware layer

* I/O registers
* Interrupts 
* DMA

What good is a computer without any I/O devices?  
keyboard, display, disks. 
We want:  
H/W that will let us plug in different devices  
OS that can interact with different combinations

### Protocol Variants
Status checks: polling vs. interrupts  
Data: PIO vs. DMA. 
Control: special instructions vs. memory-mapped I/O. 

#### PIO
```
while (STATUS == BUSY)
; // spin and wait until available
Write data to DATA register
Write command to COMMAND register (starts device) while (STATUS == BUSY)
; // spin and wait until done with request
```

Interrupts

```
while (STATUS == BUSY) // 1
{ add to dev waitq; state=BLOCKED; schedule; }
Write data to DATA register // 2
Write command to COMMAND register // 3 while (STATUS == BUSY) // 4
{ add to dev waitq; state=BLOCKED; schedule; }
```

On interrupt, kernel disk device driver loops makes processes on its wait queue RUNNABLE again  
Using Interrupts to avoid busy-waiting/polling!

#### Interrupts vs Polling
Fast device: Better to spin than take interrupt overhead

* Device time unknown?
* Hybrid approach (spin then use interrupts)

Flood of interrupts arrive

* Can lead to livelock (always handling interrupts)

Other improvements

* Interrupt coalescing (batch together interrupts)

#### DMA
DMA (Direct Memory Access):

* In practice, you use a DMA controller
* OS programs the controller, gives it information on where data lives, which device to send to etc.
* OS proceeds with other work (CPU is free).
* When DMA is complete, DMA controller raises an interrupt.
* Transfer is complete.

Interrupts

```
while (STATUS == BUSY) // 1
{ add to dev waitq; state=BLOCKED; schedule; }
Write command to COMMAND register // 3 while (STATUS == BUSY) // 4
{ add to dev waitq; state=BLOCKED; schedule;
```

#### PIO vs DMA
PIO (Programmed I/O):

* CPU directly tells device what the data is
* Explicitly, word by word by word...

DMA (Direct Memory Access):

* CPU leaves data in memory
* Device reads data directly from memory
* CPU immediately goes back to work


### Special Instructions vs Mem-Mapped I/O
Special instructions

* each device has a “port” (similar to a physical address)
* in/out instructions (x86) communicate with device
* instructions are privileged (OS only)

Memory-Mapped I/O

* H/W maps devices’ registers into physical address space
* loads/stores sent to device by OS via memory controllers


No great difference between the two
