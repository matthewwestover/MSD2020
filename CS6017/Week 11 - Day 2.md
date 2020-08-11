## Week 11 - Day 2
### D3.js
What is D3  
Core D3 is not really a visualization library, but is used frequently for visualization.  
Core D3 is:

* Nice APIs for DOM manipulation (like JQuery or others)
* A way of defining relationships between data (mostly plain javascript arrays) with DOM elements (either HTML elements or SVG elements)

Around this core are a bunch of other libs for more Vis specific stuff:

* force based graph layouts and other simulation 
* map/globe stuff
* some other geometric stuff

#### Using the D3 DOM/data stuff
d3.select( the parent, often an svg or a div)  
.selectAll( element type we want for each array element)  
.data (the data, usually an array. We can add a "key" too, more later)  
.join() 

* function for array elements that don't yet have corresponding DOM elements
* optionally, function for array elements that do have a corresponding DOM element (update them if we want)
* optionally, function for DOM elements that no longer have corresponding array elements (usually remove the DOM elements)

#### "Enter function"
Argument is the set of new elements that are being created. Usually start with enter.append(elementType)  
Can assign attributes with .attr() method  
Can add/remove classes with classed() method  
Can set up event listeners  
All methods "chain" and return the elements being created

#### Update, Exit functions
Update parameter is nodes that already exist  
Might want to update them by adding classes or changing other attributes  
Exit parameters are nodes that should be deleted  
Usually you'll call .remove() on them to delete the DOM elements

#### Transitions
A lot of stuff can be easily changed over time (position, color, line thickness) In D3, we can use the transition() method: 

* elementsToChange.transition(timeToChange) 
* .attr(toChange, newValue).attr(otherAttrToChange, newValue), etc()

There's a ton of options for how to transition from the current value to the new value (linear, ease in, ease out, "bounce", etc)  
To "wait" for a transition to end before running more code you can do: await someTransition.end()
