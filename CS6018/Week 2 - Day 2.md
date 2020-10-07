## Week 2 - Day 2
### Intro into Fragments
A Fragment is a portion of a user interface in an Activity. It’s a nice way to combine repeated UI functionality into a single object.  
You can now think of an Activity as comprising of one or more Fragments.  
Each Fragment can be thought of as a “pane” in an activity, with each activity now having multiple panes. This seems like it’s unnecessary complexity.  
There are some cases where Fragments help...

You’ve probably already seen Fragments in action.  
The classic example is the Master/Detail flow. What’s that?  
Example: you have a list of items/icons that you display the user.  
When they click an item in the list, you show more details about that item.  
Why would you use fragments for this?  

The Master/Detail flow needs to be implemented differently on different devices.  
On a phone, you typically show the Master as an Activity. Clicking a list item there opens the appropriate Detail Activity as a child activity.  
On a tablet, you could show the Master list on one portion of the screen (typically the left).  
Selecting an item from the list simply shows the details on the remaining portion of the screen (typically the right).

No need to write separate code for the tablet and the phone!  
You put the Master in Fragment A, the Details in Fragment B.  
If you’re on a phone, Fragment A ends up in Activity A, Fragment B ends up in Activity B.  
If you’re on a tablet, a single Activity A would now contain both Fragment A and Fragment B, positioned neatly on the screen.

### Activity vs Fragment
What should you put in an Activity vs a Fragment?  
In general, put “content” in Fragments, and “traffic control” in Activities.  
Layouts, views, event handling, error handling, data storage and retrieval, all go into a Fragment.  
Activities handle hiding/showing Fragments, navigation to other Activities via intents, passing data between activities and fragments.  
Basically, a Fragment is a partial, reusable, activity. How does one setup a Fragment?

### Defining a Fragment
Very similar to an Activity.  
A pair of files: an XML layout file, and a Java file that implements the logic.  
The XML file only contains whatever goes into one particular Fragment.  
Imagine we have two Fragments: Fragment1, and Fragment2.  
Each would have their own pair of XML and Java files.  
You can then embed the Fragment style files into an Activity.

Fragment1.xml

```
<?xml version=”1.0” encoding = “utf-8”?> 
    <LinearLayout ...>
    <TextView .. /> 
    <EditText.../> 
    <Button>
</LinearLayout>
```

Fragment1.java

```
public class FragmentA extends Fragment{
//Called after onCreate() of Fragment, inflate view here
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup parent, Bundle saved
InstanceState){
//Inflate the fragment xml
return inflater.inflate(R.layout.fragment_1, parent, false); }
//Called after onCreateView
@Override
public void onViewCreated(View view, Bundle savedInstancestate){
//Event handling for view objects can go here }
}
```

How do you embed a Fragment into an Activity?  
Two ways: statically (via XML), or dynamically (via code). Static embedding is straightforward:  
Activity1.xml

```
<?xml...?> 
<RelativeLayout ...>
<fragment
android:name= “com.varunshankar.Fragment1” 
android:id= “@+id/Fragment1” 
android:layout_width= “match_parent” 
android:layout_height=”match_parent”
/> 
</RelativeLayout>
```

### Dynamic Embedding
You can also embed Fragments dynamically.  
Imagine wanting to choose between which Fragment to embed at runtime, based on some user selection/preferences.  
You need to write both XML and Java code here. 
First, in the xml file, put a placeholder FrameLayout where you know you’ll insert the Fragment.  
Then, in the Java file, user FragmentManager to create a FragmentTransaction.  
FragmentTransaction allows us to add fragments to the FrameLayout at runtime.

Activity1.xml

```
<?xml...?> 
<RelativeLayout ...>
<FrameLayout 
android:id=”@+id/fl_fragment_placeholder” 
android:layout_width=”wrap_content” 
android:layout_width=”wrap_content”
/> </RelativeLayout>
```


Activty1.java

```
FragmentTransaction ftrans = getSupportFragmentManager().beginTransaction(); ftrans.replace(R.id.fl_fragment_placeholder, new Fragment1()); ftrans.addToBackStack(null); //enables back button functionality by placing on stack ftrans.commit();
```

### Finding by ID
Just like we use findViewById() to get Views and work with them, we may want to get a Fragment.  
If a Fragment is statically embedded, you can use findFragmentById.

```
Fragment1 mFragmentAObject = (Fragment1) getSupportFragmentManager(). findFragmentbyId(R.id.Fragment1);
```

Of course, this won’t work if you added the fragment via code.  
If you anticipate having to lookup dynamically created Fragments, you should associate a tag with each one, then use tag to find the Fragment.

### Finding by Tag
Activity1.java:

```
...
FragmentTransaction ftrans = getSupportFragmentManager().beginTransaction(); ftrans.replace(R.id.fl_fragment_placeholder, new Fragment1(), “SOME_TAG”); ftrans.addToBackStack(null); //enables back button functionality by placing on stack ftrans.commit();
...
Fragment1 fragment1Obj = (Fragment1.getSupportFragmentManager().findFragmentByTag(“SOME_TAG”);
```

### Passing data from a Fragment to an Activity
This is not as straightforward as sending a bundle.  
The right way to pass data is somewhat complicated. 

* First, you define a listener Interface with a callback method in your Fragment.
* Then, have the host Activity implement that interface and the callback method.
* Override the onAttach() method within the Fragment class, and use this method to capture the host Activity interface implementation.
* This is the Java callback pattern, allowing you to use a method defined in one class within another class.