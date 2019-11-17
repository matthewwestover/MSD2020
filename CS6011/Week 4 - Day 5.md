## Week 4 - Day 5
### Android
One of the most popular OS in the world  
Historically written in Java, also uses Kotlin which is like Java and is the current preferred coding language for the platform  
It can also use C++  

Android Projects are Split into three parts  
Java - actual code needed to run, set up and callback code  
XML Layouts - UI stuff. Very similar to HTML. Component Layout, there is a GUI that has click and drag to layout on computer screen and will write the code for it when you are done  
Gradle Scripts - sort of like CMakeLists, used for compiling and adding libraries 

Adding a new screen added one of each of these files.  
Java and XML Layouts tend to go to the /res/ folder  

Major “unit” in android is an Activity  
These are just different screens with different utilities  
Has an oncreate methods for activity start up  

Things that go inside an activity are Views  
These are things like buttons, input fields, etc  

Intent is how we do something new: moving from activity to activity, open a photo, share a file, etc.

Old Activity  
Create Intent  
store whatever data you still need to use  
Start activity - New Activity  
getIntent to extract data needed in new activity  
Do whatever is on that new activity  

“R” variable  
Kind of like the DOM from javascript. 
R stands for resources  
It is a static class with static constants  
It lists ids within it of what is created in the ui.  

```EditText editText = (EditText)findById(R.id.editText);```
 
