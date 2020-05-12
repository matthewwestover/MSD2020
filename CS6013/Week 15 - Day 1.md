## Week 15 - Day 1
### Virtualization
VMMs can be structured in many ways  
VMM ”owns” whole machine, just runs VMs

* VMware ESX Server, Joyent SmartOS, Citrix XenServer
* VMM schedules CPU cores, physical memory, storage
* VMM implements hardware device drivers

One kernel acts as VMM for others  
Linux KVM, Windows HyperV

* Host Linux/VMM runs normal processes, but can also run VMs
* Host Linux schedules resources like usual, but can give to VMs
* Host Linux implements hardware device drivers like usual, can use them to implement virtualized devices to VMs

VMM running as a process runs other OS within

#### Benefits
Consolidation and load balancing – many VMs can run on one physical host, VMs can be moved around  
“Infrastructure as a Service”  
Manageability – Small team can manage massive clusters - Templating, scripted/orchestrated scale up/down  
Isolation – VMs are separated from each other  
Apps that don’t trust each other can run on different VMs  
Portability – same VM can run on different physical machines  
Fault tolerance – VMs can be checkpointed, migrated, etc.  
Whole system/application and all of its state/dependencies together

#### Encapsulation
Say I want to suspend/restore a Linux or Windows application  
I decide to write the process mem + PCB to disk  
I reboot my kernel and restart the process  
Will this work? No, application state is spread out in many places. 
Application might involve multiple processes. 
Applications have state in the kernel (lost on reboot) - (e.g. open files, locks, process ids, driver states, etc)  
Virtual machines capture all of this state. 
Can suspend/restore an application on same machine between boots and n different machines. 
Useful in server farms. 
Useful for security. 
Can load an untrusted web page or install an untrusted application in a “throwaway” VM and just delete the whole thing when I’m done

#### Security
Hypervisors buggy too, why trust them more than kernels? Narrower interface to malicious code (no system calls)  
No way for kernel to call into hypervisor  
Smaller, (hopefully) less complex codebase  
Should be fewer bugs  
Anything wrong with this argument?  
Hypervisors are still complex  
May be able to take over hypervisor via non-syscall interfaces  
E.g. what if hypervisor is running IP-accessible services?  
Paravirtualization (in Xen) may compromise this
