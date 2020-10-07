## Week 3 - Day 1
### More Fragments
Recall an Activity shifts between states, exposing callbacks to you as it does.  
We know that a Fragment is associated with its host Activity. Of course, a Fragment has its own lifecycle also.  
Added level of complexity: the Fragment and Activity lifecycles interact.  
To manage data and instance states, you need to understand the Fragment lifecycle by itself and in relation to an Activity’s lifecycle.

| Activity State | Fragment Callbacks                                      | Fragment Lifecycle                 |
|----------------|---------------------------------------------------------|------------------------------------|
| Created        | onAttach() onCreate() onCreateView() onActivityCreate() | Added to Activity                  |
| Started        | onStart()                                               | Active and visible.                |
| Resumed        | onResume()                                              | Active, ready for interaction      |
| Paused         | onPause()                                               | Paused because Activity is paused  |
| Stopped        | onStop()                                                | Stopped, not visible               |
| Destroyed      | onDestroyView() onDestroy() onDetach()                  | Destroyed                          |

### Important Callbacks
* onCreate(): called when Fragment is created. Anything initialized here is preserved if Fragment is paused and resumed.
* onCreateView(): we know this already (inflate views).
* onPause(): save data from the Fragment! This can be called if:
    * User navigates backward.
    * Fragment is replaced/removed/modified. 
    * Host Activity is paused.
* onResume(): called by the host Activity to resume a Fragment.
* onAttach(): called when Fragment is attached to host Activity. Good time to check if Activity has implemented callback interface to receive data from Fragment.
* onActivityCreated(): called when Activity’s onCreate() has returned. Typically used to do final initialization (retrieving views, restoring a saved instance state).

### Data Persistence
Call onSaveInstanceState() just like for an Activity. This will be called after onPause().  
Retrieve Fragment state data in onViewStateRestored(Bundle savedInstanceState).  
This is called after onActivityCreated().  
You can also restore/retrieve data in onCreate(), onCreateView(), and onActivityCreated().  
Due to similarity with Activities, no example here.

### Master Detail Flows
This is also called a “two-panel selector”. In panel 1, you have a list of items.  
In panel 2, you have expanded details corresponding to each item in panel 1.  
If you’re on a phone, panel 1 and panel 2 appear on different “pages” of your app (aka one-window drilldown).  
If you’re on a tablet, panel 1 and panel 2 appear on same page of your app.  
Let’s develop this step by step.

### Lists: The Modern Way
You will often be loading and displaying lists of items on- screen.  
The old way was to use either ListView or GridView.  
If the lists are long, you will run into difficulties displaying the list, doing findViewById() on list items, or implementing custom animations.  
To really implement efficient ListViews, you needed to implement the view holder pattern.
RecyclerView solves all this, at the cost of added programming.

### RecyclerView
RecyclerView decouples the list layout from the items in the layout.  
You can implement lists as a linear layout, grid layout, or staggered grid layout (offset or asymmetric grids).  
You are forced to ensure that only on-screen list items are fetched and rendered.  
You can implement custom animations.  
RecyclerView uses a few different components in conjunction.  
The different components are:

* The layout manager.
* The view holder. 
* The adapter.
* The animator.

### Layout Manager
The layout manager controls how the list is shown on screen. 
Android gives you a few default types:

* LinearLayoutManager: allows for horizontal/vertical linear arrangements of list items.
* GridLayoutManager: allows for a grid of list items.
* StaggeredGridLayoutManager: pinterest style staggered/ offset grids of list items.

You could also implement your own layout manager.

### View Holder
These are instances of a class defined by extending RecyclerView.ViewHolder.  
Each view holder displays a single item in the list with a View or ViewGroup.  
The RecyclerView automatically creates only as many view holders as needed (everything on screen, plus a few extra in either direction).  
As you scroll through the list, RecyclerView takes off-screen views and rebinds them to data visible on the screen.  
All done under the hood automatically!

### Adaptor
View holder objects are managed by an adapter.  
Each adapter is created by extending RecyclerView.Adapter.  
Adapter creates view holders as needed, and binds view holders to data.  
Done by assigning the view holder to a position, then calling onBindViewHolder() (defined inside the adapter).

### Animator
An animator changes an item’s appearance on the list.  
Animators extend the RecyclerView.ItemAnimator class.  
The default animator is DefaultItemAnimator.  
We don’t need to mess with this, RecyclerView gives us automatic animations through DefaultItemAnimator!