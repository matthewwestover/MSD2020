## Week 4 - Day 4
### Code Smells
Common things to look at and try to fix  
Repeated code and repeated data  
Large methods (does it do one thing or a bunch of things?) (does it have a ton of code)  
Confusing variable names  
Public data members  
Getters where more specific methods are better (returning large amount of data)  
Try catch nests,  
Is the code hard to trace (jumps between many classes as you go along)  

### Selector and Channels
Selectors lets us listen to a bunch of stuff  
Channels are like a socket  
Blocking mode and read and write from input/output stream. If there is nothing there you cannot progress  
Non-blocking mode cannot. Can read and continue to move on.  
To get a SocketChannel we have to use a ServerSocketChannel  

ServerSocketChannel on accept returns a SocketChannel  

In main ServerSocket becomes ServerSocketChannel  
Socket becomes ServerSocket, to get the actual socket use ```serverSocket.socket();```


In room  
Make selector - in constructor  
Create a new thread that runs this stuff. (Actor model)  

```
Void serveRoom(){
    //while tier hasNext();
    Listen for messages
    Sel.select() //stop, what for a message
    Figure out what socket is ready
    socket has to switch from non-blocking to blocking mode to read
    Read message from socket, 
    Add message to message queue (add to end)
    Channel socket back to non-blocking mode
    Reregister it to selector
    Post message to all clients
    Add any clients from join list // send all backlogged messages
}

Post messages(){
    Call ws.send()
    Requires taking each socket in blocking mode
    Send 
    Switch back to non-blocking
    Reregister each 
}
```

joinRoom is tricky  
Add WS to list  
Channel needs to be registered to room's selector  

Really need to add WS to join list (array list)  
sel.wakeup() forces serveRoom select(); to run code to allow client to join room.   

In serverclass. 

```
Public static getRoom(){
If room doesn’t exist 
    Make it
    Add to map
else
Return room from map
}
```

Room thread vs Client Thread. 
RT waits (sel.select()) | sc.register(sel)  
Selector has to be woken up to register the channel  
RT is the only one who can touch the selector, cant do it from the client thread  
JDBC - java way to connect to SQLite database 