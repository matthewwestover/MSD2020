## Week 4 - Day 2
### JSON Parsing 
To parse JSON, we need to grab name-value pairs. Treat the name as a key, the value goes into variables.  
The code to parse the previous example in Android is straightforward.  
Assume we read that whole JSON code into a String variable from the internet (or a file).
The parsing code is as follows.

```
{
“firstName”: “John”, 
“lastName”: “Cena”, 
“address”: {
    “city”: “Awesomeland”, 
    “country”: “USA”
    “zip” : 98416
    } 
}

{
JSONObject jsonObj = new JSONObject(json_string); 
String firstName = jsonObj.getString(“firstName”); 
String lastName = jsonObj.getString(“lastName”);
JSONObject adrsObj = jsonObj.getJSONObject(“address”);
String city = adrsObj.getString(“city”);
String country = adrsObj.getString(“country”); 
int zip = adrsObj.getInt(“zip”);
}
```

This isn’t too bad. A bit error prone, but okay.  
Always check if there are other widely-accepted solutions before you roll your own.  
Google kindly wrote Gson, a Java library for converting between Java objects and JSON strings.  
To use this, add a Gradle dependency:  
`implementation 'com.google.code.gson:gson:2.8.5’ `. 

Let’s redo the previous example with Gson.

```
class ListItem{
@SerializedName(“firstName”) private String mFirstName;
@SerializedName(“lastName”) private String mLastName;
private Address address;
public String setFirstName(String firstName){
mFirstName = firstName; }
...
@Override
public String toString(){ return ...} }
class Address{ @SerializedName(“city”) private String mCity;
...
}
//Call Gson method to build ListItem from JSON string.
```

### Internet Connectivity
Let’s learn how to use a fairly standard workflow.  
We’ll get some JSON objects from an internet source. We’ll read it in, parse it, and display it in a RecyclerView. A great source is Openweathermap.  
This API gives us detailed weather data for many cities. You need an account to access their data.  
An API call for London looks like:  
http://api.openweathermap.org/data/2.5/weather?q=London,uk&appid=99ea8382701bd7481e5ea568772f739  
My key here is:99ea8382701bd7481e5ea568772f739a

```
{ "coord":{
"lon":-0.13, "lat":51.51 }, "weather":[ {
"id":803,
"main":"Clouds", "description":"broken clouds", "icon":"04n"}
],"base":"stations","main": {"temp":289.95,"pressure":1018,"humidity":77,"temp_min":289.15,"temp_max":291.15},"visibility" :10000,"wind":{"speed":7.2,"deg":230},"clouds":{"all":64},"dt":1536643200,"sys":
{"type":1,"id":5091,"message":0.0337,"country":"GB","sunrise":1536643751,"sunset":153669023
7},"id":2643743,"name":"London","cod":200 }
```

### Parse the JSON
The main tags and their JSON data types are:

* coord (object) 
* sys (object) 
* weather (array) 
* main (object) 
* wind (object) 
* name (String)

We can parse the JSON for this without using Gson since it is pretty straightforward.

We’re going to write a simple JSON parser for Openweathermap’s API.  
This will be the JSONWeatherUtils class.  
Software design note: JSONWeatherUtils is a helper class. You should never have to instantiate it unless there is some benefit in doing so (has member variables, does other stuff).  
The class must therefore have static member functions.  
We’ll use this parser to populate our own WeatherData class.  
This is not particularly complicated, just tedious.  
In your codes, I recommend you use Gson instead.  

### Getting the Data
Our previous example just sits there and does nothing. We’d like to get data from the Openweathermap servers.  
Ideally, we get to pick the location for which we’re retrieving data.  
We’ll need the URL and the api key associated with my Openweathermap account.  
Let’s look carefully at constructing the URL.

### Building the URL
The URL must be constructed piece by piece.  
First piece is fixed:http://api.openweathermap.org/data/2.5/weather?q=  
Second piece is user-defined:{city},{country}  
Third piece is fixed: &appid=  
Fourth piece is my app id: 99ea8382701bd7481e5ea568772f739a. 

You can use the URL class to do this, or use Uri builder and convert it to URL.
The URL class can be used as follows:

```
private static String piece1 = “http://api.openweathermap.org/data/2.5/weather?q=”; private static String piece3 = “&app_id=”;
private static final String app_id = “99ea8382701bd7481e5ea568772f739a”;
public URL BuildURLFromString(String loc){
    return new URL( piece1 + loc + piece3+app_id);
}
```

The previous approach is not considered very safe or portable.  
Google recommends building URLs from URIs.  
URIs should in turn be built piece by piece using URI.Builder.  
We won’t cover this, but keep it in mind.

### Connecting to the Internet 
Now that we have the URL, we are ready to get our JSON data.  
There are many ways to do this.  
Many programmers use the library Volley.  
We will write Java code so you can use this in your non-Android Java projects.  

Create another helper class called NetworkUtils.  
Two methods: buildURLFromString and getDataFromURL.  
The latter takes a URL, and uses HTTPURLConnection to open a connection.  
Open an InputStream through that connection.  
Use Scanner to parse input stream, return the JSON string.  
NetworkUtils should only have static methods (why?)

There’s a serious problem with what we’ve written so far. If you run it naively, this could crash your app.  
This is because the network request is being done on the main thread.  
Your app will become unresponsive if the data is large, or the network is slow.  
Solution: network request must be on its own thread.

### AsyncTask and AsyncTaskLoader
The network request is being done on the main thread.  
Your app will become unresponsive if the data is large, or the network is slow.  
Solution: network request must be on its own thread.  
When you start an Android app, the “main” thread (aka UI thread) is automatically created.  
Must avoid lengthy ops on UI thread.  
Move non-UI operations (data-related) to background/worker threads.

Imagine you want to load an image from the network when click a button. We could do this:

```
public void onClick(View v) { new Thread(new Runnable() {
public void run() {
Bitmap b = loadImageFromNetwork(); mImageView.setImageBitmap(b);
} }).start();
}
```

Unfortunately, this modifies a UI element on a background thread (the ImageView). Android doesn’t like that.  
Fortunately, Android provides some ways of modifying the UI. 
thread from a background thread.  
Most Common Ways:  

```
Activity.runOnUiThread(Runnable) 
View.post(Runnable) 
View.postDelay(Runnable, long)
public void onClick(View v) { new Thread(new Runnable() {
public void run() {
Bitmap b = loadImageFromNetwork(); mImageView.post(new Runnable(){
public void run(){ mImageView.setImageBitmap(b);
}
} }).start(); 
}
}
```

### Multithreading in Android
That got messy real quick.  
Using low-level Java threads, we have to create threads for both worker tasks and for talking back to the UI.  
Keep this method in mind for large data transfers.  
For small to medium data transfers, Google recommends the use of AsyncTask.  
Let’s see how to use AsyncTask.

### AsyncTask
AsyncTask is an abstract class that needs to be extended.  
Ideal for threads that run for a few seconds and stop.  
An AsyncTask is defined by 3 generic types:  
Params, Progress, and Result. An AsyncTask needs four methods:  

* onPreExecute() 
* doInBackground() 
* onProgressUpdate() 
* onPostExecute()

Params: what you send in. In our case, we want to send a String or URL from which to read the JSON data.  
Progress: some type of progress update. You can leave this as Void in many cases, or return some calculation of how much data has been downloaded.  
Result: the result of the background thread. In our case, it’s the String obtained from the URL, or maybe an instance of WeatherData.

onPreExecute(): invoked on UI thread before the task is executed.  
doInBackground(Params...): invoked on background thread. Takes in data for executing task, returns Result.  
onProgressUpdate(Progress...): invoked on the UI thread, can be used for conveying progress (logs, animations).  
onPostExecute(Result): invoked on the UI thread. Good time to store result or display it.  
Finally, call execute(Params...) in the UI thread.

### Loaders
They run data on separate threads.  
They give you callbacks.  
They can cache/persist results across lifecycle/configuration changes.  
They can monitor changes in the underlying data source. They are also a huge pain to use.  
AsyncTaskLoader is the Loader version of AsyncTask. It works with both Fragments and Activities.  
It knows contexts, and can therefore handle configuration changes.  
Let’s see how to use an AsyncTaskLoader.

### AsyncTaskLoader
Have the MainActivity implement LoaderManager.LoaderCallbacks<String> (that’s the JSON string returned).  
Override onCreateLoader(), onLoadFinished(), and onLoaderReset().  
Have onCreateLoader() return an AsyncTaskLoader<String> as an anonymous inner class with “this” as the constructor’s parameter.  
Get that String within loadInBackground().   
Initialize the loader inside onCreate().   
Update UI in onLoadFinished().  