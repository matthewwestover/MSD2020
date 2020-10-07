## Week 4 - Day 1
### Master Detail Flow
Problem: how to pass a List<String> to a Fragment?  
Turns out this is complicated.  
Change the data type to an ArrayList<String>, which can in fact be passed.  
Create a Bundle, call putStringArrayList, and pass data.  

Don’t lose sight of the bigger picture.  
We’re not just learning how to develop UI.  
We’re learning to break an “app” into modular chunks.  
We’re learning to think about where the data lives.  
You should ask what types of data one can pass around.  
One might want to pass more complicated objects around. We actually haven’t seen how to do this.

### Passing Complex Data
There are 3 major ways to pass complex data (say custom objects).  
First way: break it down into simpler types that we can pass, pass those types.  
Second way (Java style): make your data class implement Serializable.  
Third way (recommended): make your data class implement Parcelable.

### Parcelable
Parcelable is an interface.  
This means you have to do all the work :)  
Imagine a parcel of data is given to your fragment.  
The data class needs to implement the following:

* A writeToParcel method that packs its data into a parcel.
* Private constructor that reads from a parcel into class members.
* A CREATOR method that creates objects of your data class from a parcel, implementing the Parcelable.Creator interface. This uses the above private constructor.

Let’s replace the ArrayList with a custom data object, CustomListData.  
We’ll send this object from the Activity to the MasterListFragment.  
This will require the CustomListData class to implement Parcelable.  
We’ll then put the CustomListData into a bundle as a parcelable, and send the bundle along.  
We’ll receive and unpack the bundle in the Fragment.  
We’re one step deeper into OOP now :)  
The data sits in its own class. You can now get the data from the internet, or wherever.  
The Activity does not know about the data, just passes it around.  
The Fragment doesn’t know about the data either.  
The Adapter only knows about the part of the data it needs for its functionality: the List<String>

The RecyclerView lives in the Fragment (as it should, IMO). Currently, if we click on an item, the item is removed.  
We want to create the corresponding detail view when it is clicked.  
This should be a new Fragment containing details.  
Two questions:

* Who creates the new Fragment?
* Where should the Detail data come from?

### Fragments: Detail View
Only the host Activity should create the new Fragment! This ensures the Master list Fragment is self-contained.  
Master list Fragment is only allowed to send clicked item number to Activity.  
Activity must process, create a Detail fragment, and populate it with Detail data.  
Where does the Detail data come from? From the CustomListData class, of course!

The CustomListData class must contain details.  
Let’s first add this to the class.  
For details, we’ll identify each list member with a unique random number.  
We’ll also need a way to select a particular list member, and show only its details.  
This requires a getter method.


On item click, we’ll need to send position data from the Fragment to the host Activity.  
This requires the interface pattern again, but in the RecyclerView (itself inside the Fragment)!  
Steps are:

* Define interface in RecyclerView. 
* Implement it in the host Activity.
* Cast the ViewHolder’s parent’s context to the interface, store in interface object.
* Call the data passing function when item is clicked.

The RecyclerView has its own way of passing data around.  
This works regardless of which Fragment it is bound to.  
This is more OOP: we wrote more modular code.
In the MainActivity:

* Implement passData(int position).
* This must grab the item details for the given position. 
* Pass that information along to the ItemDetailFragment.
* Replace the second FrameLayout with the ItemDetailFragment.

