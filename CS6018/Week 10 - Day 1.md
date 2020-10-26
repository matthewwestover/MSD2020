## Week 10 - Day 1
### Design Patterns
A design pattern is typically a recurring code pattern.  
These are often formalized.  
We’ve really only studied two design patterns in class:  
Singleton (Database, maybe Repository, RequestQueue in Volley).  
Repository (architectural pattern that abstracts data inputs to the program). 
There are many others that we’ve used.  
You should learn to both identify and implement them.

There are three types of design patterns:

- Creational design patterns: ways to create classes and objects.
- Structural design patterns: ways to arrange and/or compose classes and objects.
- Behavioral design patterns: ways to communicate between classes and objects.

There are also a couple of relatively new ones:

- Concurrency design patterns: patterns that specifically address multithreaded programming.
- Architectural design patterns: MVP, MVC, MVVM, etc. High-level patterns dictating structure of program.

### Creational Design Patterns
Singleton: class with only one instance, one global point of access.  
Factory Method: define an interface for creating an object, but let subclasses decide which class to instantiate. 
Abstract Factory: provides an interface for creating a family of related objects without explicitly specifying their classes.  
Prototype: specify a prototype for an object, then clone the prototype to produce new objects.  
Builder: separate construction of a complex object from its representation, allowing the same construction 6 / 28 process to create different representations.

### Singleton
How to:  
Define a private constructor.  
Define a getInstance() method as the only means of access to the object. Make sure it uses “synchronized” for thread safety (Java mutex).  
Initialization of instance is also done in getInstance().  
Make the instance static (one instance), volatile (thread visibility).  
Place getters and setters in here as well.

### Builder
We’ve used this in Android whenever we called class.Builder().  
What is it?  
Effectively a more elegant replacement for the constructor.  
Specify/set whatever arguments you want, in arbitrary orders, as opposed to the fixed order described by the constructor.  
The Builder() pattern lets you create different configurations of objects more easily than constructors.  
How is it actually implemented?  
Let’s say we want to build objects of class A.

Replicate class A’s variables inside the Builder. Give the Builder setter methods for those variables.  
To be able to chain methods, the Builder setters need to return a Builder class.  
Have a build() or create() method that returns a class A. Have a private constructor in A that takes a Builder. IDEs can generate Builders from constructors!

```
public class User {
private String firstName;
private User(final Builder builder){ firstName = builder.firstName;} static class Builder{
private String firstName;
public Builder setFirstName(final String firstName){
this.firstName = firstName; return this; }
Public User build() { return new User(this);} };
};
Usage: User newUser = new User.Builder().setFirstName(“Name”).build();
```

### Structural Design Pattern
Adapter: client class expects one interface, adaptee offers a different interface for same features, adapter. 
converts between the two. Not the same as Android adapters!  
Pimpl/Bridge: decouple a class (abstraction) from its implementation. Also called pointer-to-implementation idiom (PIMPL idiom).  
Flyweight: support efficient creation of a large number of objects of certain type. The objects share part of their internal state, and the other part of their internal states can vary.

### Behavioral Design Pattern
Iterator: allows iterating through containers without actually exposing implementation details.  
Observer: event listeners, such as LiveData’s observe method, or onSensorChanged from SensorEventListener.  
Memento: remember the state of an object, provide the ability to restore that state (rollback).

### General Comments
These are amazingly useful in many cases.  
The Gang of Four (GoF) book titled “Design Patterns: Elements of Reusable Object-Oriented Software” is the standard reference.  
Use these wisely, you may increase complexity if you’re not careful.  
Some languages obviate the need for many design patterns-- in other words, design patterns may only be filling in gaps in C++ or Java.  
There are many others that we haven’t discussed, and very few classes cover these.

### Android Programming Review
At the UI level, we have Activities and Fragments. Fragments are reusable UI chunks.  
Pass data between Activities using Intents.  
Pass data from Activity to Fragment using setArguments() and getArguments().  
Pass data from Fragment to Activity by implementing a listener/callback using an interface. This is an example of the Observer design pattern!  
Fragments get their own layout files, typically inflated into a FrameLayout within the Activity layout file.

Activities have lifecycles, and your program needs to be lifecycle aware.  
Override various callbacks to achieve lifecycle awareness.  
Fragment lifecycles are an additional pain on top of this, and they interact with Activity lifecycles.  
An easy way to make Fragments configuration-aware is to call setRetainInstance(true), which will preserve the Fragment instance across Activity recreation.  
Make sure Fragments do not have dependencies on each other. Activity is the traffic controller.

RecyclerView is an efficient way to show lists of objects on the screen.  
Four components to a RecyclerView:

- LayoutManager (linear/grid/staggered grid). 
- ViewHolder.
- Adapter.
- Animator (we never touched this).

ViewHolder is defined inside the Adapter class. The idea is that each ViewHolder is bound to the RecyclerView automatically, as needed.  
We need an XML file for a generic RecyclerView item.

We discussed fetching data from the internet using raw Java code.  
We saw how to do the same task using Request and RequestQueue in the Volley library.  
We also tried downloading files using DownloadManager.  
We explored JobScheduler for downloading files (and in general scheduling tasks) according to some constraints.  
In the context of Volley, we saw the Singleton pattern for RequestQueue (ensuring only one RequestQueue exists for whole program).

To ensure we didn’t hang the UI thread, we downloaded data using AsyncTask.  
This presents challenges in context of lifecycle awareness: zombie threads can be created.  
The old solution to this was to use AsyncTaskLoader, which automatically handled configuration changes.  
Loaders are a deprecated pattern now in Android.  
The replacement to Loaders is a ViewModel.  
AsyncTask is also deprecated as of API 30.  
Replace with Java threads, or Kotlin coroutines (functions that execute asynchronously).  
RxJava is the de-facto standard for asynchronous programming in Java

ViewModel is like a Fragment with setRetainInstance(true). This means it preserves data past configuration changes.  
It’s a great place to store UI data across a single Activity’s destruction and recreation.  
It doesn’t protect against switching to another app and returning, but onSaveInstanceState() works for that.  
ViewModel is therefore an alternative to a Loader.  
ViewModels also help simplify communication between Activities and Fragments.

LiveData is a wrapper around any data type.  
It allows you to attach an observer to that data type, and handle changes to the data.  
If you want an observer inside the ViewModel, you’ll have to use observeForever() rather than observe().  
The ViewModel has a filtered subset of the data in the repository – only what’s necessary for the UI.  
It’s bad design to have to change the ViewModel every time your data source changes.  
Instead, we abstract things away another level: we use a Repository to pass data to the ViewModel.  
The Repository itself is hooked up to a database, cloud, or some form of file storage.

We use the Room persistence library to do database reads and writes in Android.  
This is done using 3 classes:

- Entity: the class to be written to the database.
- Database: the (Singleton) class that manages the database.
- DAO: the data access object, maps Java methods to SQLite queries.

The DAO’s magic is to use getters and setters in the Entity to do the SQLite stuff.

The modern way of scheduling tasks in Android is to use WorkManager.  
It lets you schedule periodic or one-time tasks. It lets you chain tasks.  
It will automatically decide to use JobScheduler, AlarmManager, or Firebase JobDispatcher to do these tasks for you.  
It is fully backwards compatible (unlike JobScheduler).  
WorkManager is multithreaded automatically. No need to mess with AsyncTasks.

Android gives us both physical and synthetic sensors.  
Physical sensors are real ones.  
Synthetic sensors are a combination of physical sensors.  
Sensors either return scalar-valued or vector-valued data.  
Among the vector-valued sensors are the gyroscope, accelerometer, step counter, and step detector.  
You could use either the gyroscope or accelerometer to develop gestures. The former is more accurate, the latter more battery efficient.

Main takeaway: enforce separation of concerns in your code using functions, classes, “modules”, “packages”, or whatever tools are available.  
Don’t leave testing for the end. Modern practice is to test as you go along.  
Understand that most of modern programming involves “Inversion of Control” – callbacks that are triggered when events occur.  
Golden rule applies to programming also.  
Don’t write code that you wouldn’t like if someone else handed it to you.  
You will rarely be the first or last person to work on your code. Be courteous with variable names, engineering, testing, and documentation!