## Week 4 - Day 3
### Nonblocking I/O
```
Class websocket{
    Constructor(handshake)
    String readMessage
    Void sendMessage(string)
}
```

We handshake - open socket  
We need to read join room message  
Keep track of everyone in room  
When anyone posts echo to everyone in the room array list<websocketconnections>  
We are blocking currently. Wait until something happens then do something  

Two approaches:

* Every single connection reads a messages
    * Sync problem. Waiting to read a message and not one coming. Thread is stopped. “Blocked”
    * Means one thread is awake on message send, rest are still sleeping 
    * Inputstream and output stream on sockets are independent
    *  S1 and S2 can be waiting to read. S2 gets a message, thus wakes up. S2 can write the message to itself and then to S1. 
    * This is a thread for every client is connect, when ever anyone is woken up, it will go and write to all sockets in the room. 
    * This needs to be atomic so there isn’t read/write overlap
    *  Make method synchronized so there isn’t interference
    * We are sharing the array list of who is in the room. 
    * Verify this is safe. Anything touching array list has to be synced so it can’t be modified while being accessed
* Or have one thread that just listens for posts from anyone in the room. 
    * Specify things to listen to
    * Wait for any of them
        * Use selectors
    * Selector Class
        * Keeps track of what we want to listen to
        * Has method for what to do when something happens
    * Channel Class
        * Socket/event
    * Register a channel+event with a selector
        * We are responsible for code after wake up
        * Can register sockets, pipe, file stuff
        * Has to be unblocked to register
        * Wait for something to happen,
        * Grab it -> move it back to blocking to do stuff 
        * Move back to unblock mode

Both of these are valid. Individual threads was done by Apache for a long time. Channel + selector is newer and faster. 
