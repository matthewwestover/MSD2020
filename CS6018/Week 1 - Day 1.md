## Week 1 - Day 1
### Application System Design
**App Development**: learn to develop an app with a responsive, efficient, and aesthetic UI.  
**Sensor programming**: learn to tap into the Android sensor framework, and read and process data.  
**Cloud Integration**: learn to integrate a cloud backend to your app. We‘ll use Amazon AWS.

### App Development
Learn to develop a single app for many form factors and devices.  
App development requires a good understanding of event- driven programming, the lifecycle of Android apps, about OS resources, efficiency and battery usage, etc.  
App development also forces you to think about aesthetics, the right use of colors and styles, and many design principles.  
This is a great way to actually learn general programming and design principles.

### Sensor Programming
Android lets you tap into a variety of sensors.  
Accelerometers, magnetometers, gyroscopes, light sensors, even temperature and pressure.  
Android also lets you create custom sensor types that are based on existing hardware sensors.  
You will learn to read this raw data, process it to get some meaningful information.

### Cloud Backend
We‘ll learn how to integrate an AWS backend into our Android app.  
You‘ll learn the standard pattern of having a local database backed up on the cloud.  
Time permitting, we‘ll also do some standalone hands-on work on writing web servers in a couple of different frameworks.

### Intro to Activities
Android apps are typically made up of upto 4 connected components:  
1) Activities: a single screen with a user interface (UI), the entry point for user interaction. Activities have to do a lot. Manage the lifecycle of the app, make sure things important to the user are kept around and not killed/stopped by the OS. Help manage user flows between apps, possibly via intents, and within a given app.  
2) Services: no UI. Typically runs in the background for long-running tasks, performs work for remote processes. Typically started by an activity. Eg. Keep playing that Spotify song even if you‘ve switched to some other app.  
3) Broadcast receivers: components that allow the app to receive events from the OS that are not a part of the regular function of the app.  
4) Content providers: manage shared app data to be stored either locally on the phone, on a remote database, or some persistent storage location. Eg. Contact information is managed by a content provider, can be used by other apps if they have permission.

Each screen/page in your app is an Activity.  
Each Activity involves at least a pair of files: a .java file, and a corresponding .xml file.  
The .java file for an activity contains a subclass of the Activity class.  
For instance, MainActivity almost always extends AppCompatActivity, which in turn extends FragmentActivity.  
You override specific functions of the base class that you‘re extending to get different kinds of functionality.  
For instance, if you override onCreate(), you get to do stuff right when the app is created. If you override onDestroy(), you handle cleanup when the app is destroyed.

The xml is in the activity_main.xml file.  
The Java code is in the MainActivity.Java file. We override onCreate() from AppCompatActivity.  
Using setContentView, we inflate the xml file, then render onto the screen.  
Inflation is done by traversing the xml tree.  

Android Studio also has a GUI (design view) for manipulating xml.  
Let‘s see how to develop a simple UI for an app.  
Simple process:  
Sketch out the app, either by hand or in some software.  
Identify elements of your sketch, and decide what Android things they correspond to.  
Create the xml for your sketch.  
Inflate the xml in your Java code.

Attribute android:padding adds space inside a view or ViewGroup.  
Attribute android:layout_margin adds space outside a view or ViewGroup.  
LinearLayout can be either horizontal or vertical.  
For LinearLayout, the order of items in the xml file matters!  
When you add multiple Views to a FrameLayout, they stack.  
The last added view is seen on top.  
This is called Z-ordering. Z here refers to the z-axis that is pointing into the screen.  
Using FrameLayouts, we can decide how deep some views are relative to others.  
This is very useful for overlaying text onto other views, such as images.  

You could nest horizontal and vertical linear layouts to create a grid But this is wasteful. 
You can create a grid of views on the screen.  
Each view can span one or more grid cells.  
It‘s more efficient than a whole bunch of nested LinearLayouts.  

LinearLayouts allow for row/column arrangements of Views.  
FrameLayouts stack their views, allow for z-ordering.  
GridLayouts allow for grid arrangements of views.  
RelativeLayouts allow you to do all the above, and more.  
Clearly, RelativeLayouts are the most flexible and powerful.  
The Android default is ConstraintLayout.