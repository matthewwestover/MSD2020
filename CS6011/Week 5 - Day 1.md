## Week 5 - Day 1
### Android
UI Updating during runtime

Only have one thread that just updates the view (drawing) and nothing else.  
Main thread  
Updated UI  
Draw UI  
No hard work  

All other threads do the “work” sending a message, receiving a message  
Single core device OS has jump back and forth, multicore can do both at the same time  
Android enforces main thread doing ui and other threads not doing ui.  
It throws exceptions when you try to go against this  
Have to “pawn off” from other threads to main thread for ui to update  
“Schedule” action in main thread  

Uses “Handler”   

```
Handler h = new Handler(this.getMainLooper()); //get me the ui thread’s work queue
h.post( () -> {
// do this code
});
```

Different view classes to allow lists of new messages  
RecyclerView - when something is off the screen use the same view object  
ListView - simpler  
Better to store messages in list, use an adapter to tell list view to update from the message array.  
This lets you only have data stored in one place  
Edit data array- call ```notifyDataChanged(); // this contacts the adapter it will update the UI ArrayList```

Websockets  
Different libraries to use  
[https://github.com/TakahikoKawasaki/nv-websocket-client](https://github.com/TakahikoKawasaki/nv-websocket-client) is a good one to use  
Have to update android manifest to ask for internet permission  
AsyncTask - run somewhere not the main thread  
Used localhost for the webbrowser because local host is the machine it is running on. 
Android runs on a “fake” phone thus cannot use localhost to check.  
Use special IP: **10.0.2.2**