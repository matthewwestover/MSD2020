## Week 1 - Day 4
### Network Programming
Java I/O is multi level  
Low level - Streams  
Input streams and output streams  
```Int read(byte[])``` and ```int write(byte[])``` - read and write in byte chunks at a time  
No formatting or higher level info. Just how many bytes do you want/need at a given time   
Scanner and Printwriter are higher level classes that let us use the input/output streams of bytes  
There are also object input/output stream allow you to send whole object out and in different places  
Buffered reader and buffered output stream are older versions of scanner and print writer, not as good.  
Doesn’t put things in the stream right away for efficiency purposes.  
Buffering - not sending right away, big chunk of data is built up then sent
.flush() forces the building buffer to send  
Throwing involves kicking exceptions, when errors occur. Mostly commonly with I/O involvement. Lots of errors possible in input/output usage  

***Networking***  
Two major classes we care about  
**Socket()** 

* TCP connection open between a client and server
* Once socket is open, not major distinction between client and server, both read/write
* It is important when creating the connection. 
* Getinputstream() and getoutputstream() are the two major methods we can use with it
* Two constructors client side and server side
* Client side
    * Input/output of server
        * Asks to connect to a server

**ServerSocket()**

* Wait for clients to connect
    * “Listens”
    * Uses accept() to take the request and create the connection socket
        * Blocking call - code stops until the connection is requested
        * 
* ServerSocket is created, hangs out, once request is received, a normal socket is opened
* .close() will close sockets and serversockets
* Server is a big while true loop
* To connect to multiple clients on a server, use multithreading

**Wireshark**
Can’t use Wifi to see localhost requests. Use Loopback to see those requests from localhost servers in testing

