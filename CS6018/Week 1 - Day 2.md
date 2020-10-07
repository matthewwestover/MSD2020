## Week 1 - Day 2
###More Activities and User Interaction
ConstraintLayout  
This is RelativeLayout on steroids.  
What was the primary issue with RelativeLayout? It still potentially could suffer from nested ViewGroups and complex view hierarchies.  
The more complicated the view hierarchy tree, the more resources it will take to traverse that tree.  
ConstraintLayout was created (3 years ago) to overcome this issue.

Much like RelativeLayout, you specify the locations of Views with respect to each other, and wrt to parent layout.  
However, ConstraintLayout does this by imposing constraints on the different views.
The views are constrained to each other, and to the parent.  
Views can be constrained to groups of views, without explicitly using a ViewGroup to group them!  
The ViewGroup tree is flat since there are no real children of children.  
ConstraintLayout is harder to program, but sometimes very useful.

With RelativeLayout, you don’t really need to use the Design view in Android Studio.  
With ConstraintLayout, it is usually pretty hard to do things without Design view.  
I’ve found that my workflow for ConstraintLayout is:  
Design UI using design mode.  
Polish up measurements in text mode.  
Repeat.

There are many possible constraints that would lead to the same arrangement of Views on the screen.  
You could also design a UI in RelativeLayout, and have Android Studio convert it to a ConstraintLayout for you.  
Able to do it mostly in design mode, but might have to switch back to xml a couple of times.  
It will flatten the view hierarchy, remove any nested layouts, but achieve the same effect.  
Don’t rely entirely on an automated process for design.  
Design hinges on key elements of human perception, psychology, and our sense of aesthetics and spacing.  
These are often hard (but not impossible) to capture using algorithms.

### User Interactions
We’ve been creating static, lifeless user interfaces (UIs).  
The UI is the part of the app that faces the user.  
It needs to be able to accept and operate on input:  
Handle EditText and its text input.  
Handle button clicks.  
Show messages to users alerting them about stuff. Open a new screen based on some input.  
Go back to the original screen.  
Open some other app (maps, mail, camera, whatever). Many more types of interaction.

### Button Click
What we need is a callback: a function that is called when an event occurs.  
Fortunately, Android has a bunch of callbacks already in place for several standard types of user interaction.  
There are many ways to handle button clicks built into Android.  
All of them involve implementing the View.OnClickListener class in some way.

```
Button mButtonSubmit = (Button) findViewById(R.id.button_submit); mButtonSubmit.setOnClickListener( new View.OnClickListener() {
@Override
public void onClick(View v) {
***Do what you want with the click here*** }
});
```

Must do the above for each button!

That’s wasteful and error-prone code, not to mention messy. There’s a related but better way to do this.  
Have your Activity class implement View.OnClickListener. This is an interface, essentially.  
If you implement this, you must override the onClick() method in this interface.  
All the click functionality is handled in the overridden onClick() method.

In the onClick() method, we need to:  
Grab the string from the EditText.  
Parse it to figure out first name and last name.  
Set first name to a TextView, and last name to another TextView.  

Show a message to the user for each edge case. How do we show pop-up messages that vanish?  
These are called Toasts in Android.

Here’s how you create a Toast that displays a string message:  
`Toast clickToast = Toast.makeText(context, ”some string”, Toast.LENGTH_SHORT); clickToast.show();`  
Or, if you’ll never need the toast object again:  
`Toast.makeText(context, ”some string”, Toast.LENGTH_SHORT).show();`

### UI: Intents
All Android activities are started or activated with an Intent. You also use Intents to communicate between Activities.  
Activity A can pass data along with the Intent, and Activity B can receive it and do something with it.  
There are really 2 types of intents: explicit intents, and implicit intents.  
Explicit intents explicitly start a specific Activity in your app.  
Implicit intents simply allow the OS to decide which Activity should be started.  
For instance, you may want to pass a URL to Maps using an implicit intent, or start the camera app to take a picture.

An optional component name (the one you want to start). This decides whether intent is explicit or implicit.  
An action: for instance, whether you want to view stuff, or share stuff.  
Data: This has to be a URI object that references the data to be acted on, and/or the type of data.  
Other stuff (strings, data, etc.).

###Explicit Intents
Let’s say we’re starting a new Activity called ActivityB from the MainActivity.  
Let’s also say we want to send some data over from MainActivity to ActivityB.  
The most general way is to send a Bundle object.

```
Intent messageIntent = new Intent(this, ActivityB.class); Bundle messageBundle = new Bundle(); messageBundle.putString(KEY,”value”); messageBundle.putInt(KEY,value); messageIntent.putExtras(messageBundle); startActivity(messageIntent);
```

### nURI
A URI is a string of characters.  
Meant to unambiguously identify resources.  
`URI = scheme:[//authority]path[?query][#fragment]`

### Implicit Intents
Think of implicit intents as avoiding reinventing the wheel.  
If there’s app for it, and it’s good, use it.  
Another good example along these lines is the camera.  
If you want to take a picture, just use the camera intent.  