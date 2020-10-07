## Week 5 - Day 2
### Android Architecture Components
They are a part of Android Jetpack, which is a huge and relatively new library separate from the platform APIs.  
Updated more frequently than the Android platform itself.  
These components were designed to save programmers time in writing tons of boilerplate code (for example, loaders).  
There are several components.

**The Data Binding Library**: Helps you avoid writing Java code using annotations in the xml file.  
**Lifecycle and LifecycleOwner**: Help you manage your app lifecycle.  
**LiveData**: An observable data holder, lifecycle-aware, can respond to changes in data.  
**Navigation**: Helps you manage navigation within your app.  
**Paging Library**: Helps you gradually load data into RecyclerView.  
**Room Persistence Library**: Easy read/write to SQLite databases using annotations.  
**ViewModel**: Helps preserve data model through configuration changes.  
**WorkManager**: Abstraction layer over JobScheduler, AlarmManager, and Firebase JobDispatcher that automatically selects between these.

The architecture components are constantly evolving.  
They are now the favored tool for writing nice Android apps. Today, we’ll focus on ViewModel and LiveData.

### LiveData
LiveData is an observable wrapper around other data types.  
It is lifecycle-aware.  
Typical workflow for LiveData objects:

* You wrap your datatype in LiveData.
* Create an Observer object, define the onChanged() method.
* Attach the Observer object to the LiveData object using observe().

It’s impossible to discuss LiveData without discussing ViewModel.

### ViewModel
ViewModel decouples UI data from the UI controller (the Activity/Fragment).  
This allows data in the ViewModel to survive configuration changes.  
ViewModel is lifecycle-aware with regard to configuration changes.  
However, a ViewModel cannot survive the process being stopped! You still need onSaveInstanceState().  
It’s a replacement for the Fragment setRetainInstance(true).  
A ViewModel is a replacement for a Loader.  
It’s also a great way to separate the data from any fragment or activity.  
It provides a fantastic way to share data between Fragments.  
You typically store your LiveData objects inside ViewModels.  
Testing is now easier, since you can make mock data sources to feed your ViewModel.

We used AsyncTaskLoader to get the data on a separate thread and make things lifecycle-aware.  
The UI will need to pass in the location to our ViewModel.  
The ViewModel will hold a MutableLiveData<WeatherData>, which will contained the parsed WeatherData object.  
We’ll use an AsyncTask inside ViewModel to get the data on a separate thread.  
The data lives in a different place from the Activity, and is therefore safe.  
From the software engineering perspective, it’s still not perfect.  
The issue is that the ViewModel is fetching data from the network.  
While we’ve decoupled the data from the UI, it’s still coupled strongly to our ViewModel.  
We’d have to rewrite ViewModel if we wanted to fetch data from a database instead.

### Repository Design Pattern
The repository design pattern exists to solve this problem.  
The ViewModel shouldn’t really know whether the data is coming from the internet or a local database.  
So, the solution is that the ViewModel should get the data from a repository.  
The repository itself gets the data from either the internet or a database (or whatever).

`[Activity/Fragment] -> [ViewModel w/ Live Data] -> [Repository] -> [Room (SQLite) or Remote database]`  
The repository class will obtain the data from the network and parse it.  
The ViewModel will pass data onto the repository to help the latter fetch data.  
The ViewModel will be the only class to hold/create instances of the repository.  
The Activity won’t know anything other than the ViewModel.