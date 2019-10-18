## Week 2 - Day 4
### Synthesizer Discussion

```volumeWidget.getComponent().connect(0, sinWidget.getComponent();```

Make a line object to visualize connection need start and end coords  
Coords need to be correct ref frame. Layout is to parent, scene is window, window is full screen.  
Convert between them as scenetolocal, localtoscene  
Find location of circles, make a line, set endpoints(conversion of coords)  
Find location - add getter methods in widget() to return the location coords  
Add getter methods for circle locations too.  
Cable connect methods - sets the components relative to each other.  

How to drag around screen.  
Who is responsible for the movement and what are the triggers  
Widget() can manage dragging  
Widget should store the double x,y passed in for starting widget position  
Need to listen for mouse pressed(down), mouse released(up), and mouse drag  

Member var of old position  
**Mouse down**  
Set old mouse position here  

**Mouse dragged**  
Find position change - listen to mouse event to get current mouse position  
Move ```widget.relocate(x, y)``` or ```setLayoutX and setLayoutY // change+= x/y```

**Mouse up**  
Start with first two. If there are issues this might have to be added 

**Cabling**  
More tricky. The handler for managing connection can’t be handled by the Widget(). Each individual widget can’t see what other widgets are for mouse click handling. 

The mouse listeners have to go into the main pane  
**Mouse down**  
See if it is on a circle. There is intersect method. It uses local coords but not scene where the mouse lives. Have to convert local to scene scene to local.   
Make a line(start input)

**Mouse dragged**  
Update one endpoint of line  

**Mouse up**  
See if it is in an appropriate circle  
Set end point of line  
Cable class calls connect to the components of the two widgets.  

If not in an appropriate circle, delete line.  
