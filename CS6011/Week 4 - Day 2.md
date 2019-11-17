## Week 4 - Day 2
### Server WebSockets
To implement we know with sockets it is built on TCP  
Once websocket is open it needs to stay open.  
Because TCP lets you send anything, there is a protocol for constant open communication

Handshake  
    HTTP Type Protocol  
    Look through head to see if a web socket key is present.  
    To respond from server we have to do something more complicated  
    Concatenate the incoming sec-websocket-key + magic   
    SHA-1 is a standard library function on that value. ^  
    Base64 that value. ^  
    Base64(SHA-1(Sec-websocket-key+magic phrase);   
    Can’t use browser to test as websocket-key is randomly chosen  
    Fake create a client in js with a set key. If onopen(); works then it opened.  
    Design code so that there is a method to convert to and from.  
        Take a string and return a string is testable  
        Take a socket and find a string to get a value cannot be tested  
        InputStreams can test, be passed from socket or byte[] input stream  
        Output streams can be okay  
        Static methods are easier to test as they don't touch member variables  
        
Dataframe  
    Encoded to Hex, send/receive is unmasked/masked  
    Datainputstream wraps inputstream. Can fill in array of bytes, and read bytes as an int  
    Same for dataoutputstream