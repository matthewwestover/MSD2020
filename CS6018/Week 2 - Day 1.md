## Week 2 - Day 1
### Activity Lifecycle

A good Android app must be **lifecycle-aware**!

An activity cycles through a set of states.  
Each state transition is associated with a method/callback.  
If you want to deal with some part of the lifecycle, you override the appropriate method.  
For instance, if the activity calls an implicit intent, you may want to save some UI data, then reload it when the activity regains focus.  
You can do so by overriding onPause() and onResume(). Many other possible cases.

You open Google Play, download an app to install.  
A screen appears, asking you about app permissions.  
The activity for the install screen is paused, permission screen is visible.  
However, the install screen is also still visible!  
So, the UI still needs to be drawn for the install screen activity. However, the rest of it needs to be paused.  
When you think of the activity lifecycle, you are forced to think of how different parts of your activities need to be handled differently.

So, user interaction, intents, etc. can force your app to move from active->visible->background.  
This is unlike traditional desktop programs.  
Traditional programs run until they’re done, or until you quit.  
You save state and free resources at a predetermined time in the program’s lifecycle.  
On Android, lifecycles are non-deterministic!  
Your app should be ready to handle these possible state transitions.

Every time you change screen orientation (not phone orientation), Android destroys the app, and creates it again.  
This is to facilitate clean rendering in different modes. So, our TextViews got destroyed.  
They’re populated in onClick(), so they won’t be repopulated when onCreate() is called.  
EditText is untouched.  
This is kind of a pain. How do we know what will be untouched?  
We don’t. We have to plan for the worst.

### Data Persistence
We need a way to make data persistent over app lifecycle.  
Save the TextView information somewhere, then retrieve it.  
Clearly, when the Activity is destroyed, we need to store all relevant data.  
When the Activity is recreated, we need to retrieve that data and populate the views with it.  
Which methods do we need to override? How do we save data?

There are a few ways, depending on the size of the data, and your general goals.  
If the data is lightweight (a few strings and ints), Android provides an internal mechanism to save a simple UI state.  
Override onSaveInstanceState() to save an “instance state”.  
Either restore this instance state in onCreate(), or override onRestoreInstanceState().  
onSaveInstanceState() is called right before onStop(). onRestoreInstanceState() is called right after onStart().

Example of onSaveInstanceState():

```
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    // Save the user's current state 
    savedInstanceState.putInt(KEY, VALUE); 
    savedInstanceState.putString(KEY, “value”);
    // Always call the superclass so it can save any view hierarchy
    super.onSaveInstanceState(savedInstanceState); 
}
```

Example of onRestoreInstanceState():

```
@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
    mTv.setText(savedInstanceState.getString(KEY)); 
}
```

Example of restoring in onCreate():

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); //call the superclass first 
    if(savedInstanceState!=null){
        mSomeInt = savedInstanceState.getInt(KEY);
    mSomeString = savedInstanceState.getString(KEY); 
    } else {
    //Initialize stuff as you would’ve for the first time
    } 
}
```

What should you do when you lose focus, onPause()?  
Stop any battery draining tasks, this mode exists mainly to save battery.  
onDestroy() may never get called! Don’t do anything in it. Seriously.  
What should you do in onStop()?  
If you want to do a large data dump of your app to a file or online database, this is the right place to do it (before stopping).

### Data Storage
Android gives you a few ways to store your app data:  
Internal file storage (restricted to your app).  
Internal cache storage (temporary file storage).  
External file storage (any app can access this).  
Shared preferences (primitive types as key-value pairs). Databases (SQLite, storage in private database)

### Internal Storage
Every app is allotted an internal storage directory.  
Android gives you an API to access this special location in the file system.  
You could use Java streams to write data  

```
String filename = "myfile";
String fileContents = "Hello world!”; 
FileOutputStream outputStream;
try {
    outputStream = openFileOutput(filename, Context.MODE_PRIVATE); 
    outputStream.write(fileContents.getBytes());
    outputStream.close();
} catch (Exception e) { e.printStackTrace(); }
```

### Internal Cache
You can also store temporary files in a cache.  
Most apps do this already.  
Small amount of storage allotted, must clean it up ourselves.  

```
try{
File myFile = File.createTempFile(“fileName”, “extension”, context.getCacheDir()); 
}catch(IOException e){ e.printStackTrace(); }
```

### External Storage
This is a lot more involved because these files can potentially be shared with other apps.  
Could be on external storage, but private to your app.  
Could also be public.  
The following steps are required:  
Request storage permissions for your app.  
Verify that storage is actually available.  
Choose between public and private external storage. Store data.  
If the storage is to a private directory on external storage, stuff gets deleted when your app is uninstalled.  
If you want files to stick around after your app is uninstalled, choose a public directory on external storage.  
External storage could be an SD card, a partition of internal device memory.  
Let’s see some code, then see an example.

Write (and, implicitly, read) permissions

```
<manifest ...>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
</manifest>
```


Read-only permissions

```
<manifest ...>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
</manifest>
```

Read and Write Verification

```
public boolean isExternalStorageWritable() {
String state = Environment.getExternalStorageState(); 
if (Environment.MEDIA_MOUNTED.equals(state)) {
return true; }
return false; }
```

At-least Read Verification

```
/* Checks if external storage is available to at least read */ public boolean isExternalStorageReadable() {
String state = Environment.getExternalStorageState(); if (Environment.MEDIA_MOUNTED.equals(state) ||
Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
return true; }
return false; }
```

Public directory

```
File path = Environment.getExternalStoragePublicDirectory(Environment.SOMEPREDEFINED DIRECTORYNAME);
File file = new File(path, “filename.extension”); If(!file.mkdirs()){
//Complain or log.
}
```

Android provides choices for predefined directory names. Eg. DIRECTORY_PICTURES

Private directory

```
File path = Environment.getExternalFilesDir(Environment.SOMEPREDEFINEDDIRECTORYNA ME);
File file = new File(path, “filename.extension”); If(!file.mkdirs()){
//Complain or log.
}
```

