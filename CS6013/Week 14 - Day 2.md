## Week 14 - Day 2
### Disks
Disk has a sector-addressable address space  
Appears as an array of sectors  
Sectors are typically 512 bytes or 4,096 bytes.  
Main operations: reads + writes to sectors  
Sector is unit of atomicity  
Mechanical (slow) nature makes management “interesting”

#### Typical Disk Org
Coated with magnetic material that encodes bits  
Capacity increases come from improvements in bit density  
Logically divided into:

* Track: ring on a platter
* Sector: unit of r/w, portion of a track
* Cylinders: stacks of tracks

Read/write data (overview): 

* Position disk head over track -Seek time
* Wait for sector to rotate under head - Rotational delay
* Read/write data from/to sector - Transfer time
* Total Delay = Seek + Rotation + Transfer

Disk physics:

* Modern disks spin at 5400, 7200, 10000, and 15000 rpm
* Outside edge of 3.5” disk spins at over 150 mph
* Disk head “floats” on very thin cushion of air above platter
    * Bernoulli effect used to “fly” as close as possible
    * Magnets keep heads steady
    * "Head crash” → disk head contacts the surface

Disks organized as stacks of platters:

* Disk heads mounted on “combs” → often heads on both sides
* Disk arms/heads have a single actuator; they must move together

Disk controller

* Managing arm/head movements
* Contains RAM to cache disk contents from/to disk
* Accepts commands from CPU → responds using DMA/interrupts

#### Seek
Seek cost: function of cylinder distance  
Not purely linear cost  
Must accelerate, coast, decelerate, settle  
Settling alone can take 0.5 - 2 ms  
Entire seeks often takes several milliseconds - 4-10ms  
Approximate average seek distance = 1/3 max seek distance  

#### Rotation
Depends on rotations per minute (RPM).  
7200 RPM is common, 15000 RPM is high end.  
With 7200 RPM, how long to rotate around?  
1 minute / 7200 rotations =  
1 second / 120 rotations =  
8.3 ms / rotation  
Average rotation?  
8.3 ms / 2 = 4.15 ms

#### Transfer
Pretty fast — depends on RPM and sector density  
100+ MB/s is typical for maximum transfer rate  
How long to transfer 512-bytes?  
512 bytes / (100 MB/s) = 5 μs

#### Fastest Workloads
Sequential: access sectors in order (transfer dominated)  
Random: access sectors arbitrarily (seek+rotation dominated)

#### Buffering
Disks contain internal memory (2MB-16MB) used as cache  
Read-ahead: “Track buffer”  
Read contents of entire track into memory during rotational delay. 
Write caching with volatile memory  
Immediate reporting: Claim written to disk when not  
Data could be lost on power failure  
Tagged command queueing  
Have multiple outstanding requests to the disk  
Disk can reorder (schedule) requests for better performance

#### Avoid Slow Seeking
Design file system carefully  
Use RAM as a cache for disk  
Once a block is read, cache it as long as possible  
When a block is written to, delay the actual write  
Replace hard disk with SSD  
Smarter disk scheduling   
Permute I/O operation order to reduce/eliminate seeks  
First-Come First-Served  
Shortest Seek Time First -This time we need not be omniscient to know job length  
Hybrid hard drives? (Look up the Seagate Momentus XT).  