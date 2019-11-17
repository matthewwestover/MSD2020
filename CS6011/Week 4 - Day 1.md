## Week 4 - Day 1
### Threads
Programs need to do multiple things concurrently  
This isn’t really parallel, but we have many CPU cores to utilize  

A thread is a running function  
”Running” - has a call stack, and has access to the CPU to do stuff. 

In Java  
To make one thread it is the running program from main()  
To make additional ones use the thread class  
Threads take a runnable constructor param. 
Runnable is a interface containing ```void run()```

```Thread t = new Thread(Runnable)```

Runnable is frequently just a lambda/anon function call

Thread Class has its own methods

```t.start();``` This execute the code from the runnable object  
```t.join();``` wait for a thread to finish so it can return to the class that called the thread.  
This is a blocking call. The running main() stops until the thread finishes and returns to join back in  
Main waits for t. 

```t.getId();``` get the identification for the running thread. These are Ints  
```Thread.currentThread();``` get a thread object that is the thread this method Is called in.  
```Thread main = Thread.currentThread();``` - we never created a main thread in starting program, but now we can get its id. 

You can have arrays of threads.  

Seems easy and short, is one of the most complicated things to do, and do correctly  
Threads might be running at the same time on the same core, but maybe not  
They can get switched out based on the scheduler. It is unpredictable and we have no control over it.  
Once running, we have no control over their order to find which goes first.  
If there is any shared resources, we have to be careful. The CPU doesn’t ensure they are handled safely.  
Bugs can be impossible to figure out as issues can occur based purely on the random order of the threads running and their overlap  

If all threads are only reading from the same source, that is okay. You aren’t updating the object.  
If there are threads that are reading, and some that are writing. This becomes an issue. You don’t know what’s been updated. (Find before updated, search says its there but its not. Update can put what is searched for in an area already searched. There is a tearing where you can see half an old value and half of a new value, finding in the middle of the update). 

In java mark sections of code as synchronized  

```
Class C {
    
    Public synchronized void method(){
        Code.
    }
}
```

Only one thread in Class C can call synch method at a time. Multiple threads calling it have to take turns. 