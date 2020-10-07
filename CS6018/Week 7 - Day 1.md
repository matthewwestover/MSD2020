## Week 7 - Day 1
### JobScheduler
JobScheduler is the most common way to schedule background work.  
It can be used to schedule work like downloading data, updating network resources, etc.  
More importantly, this can be done while optimizing for memory, power, connectivity.  
Older alternatives are: Syncadapters. AlarmManager

JobScheduler is considered scalable: good for both small tasks (clearing local cache) or larger tasks (sync your database with a server).  
It is typically used for tasks that are not time critical.  
Rather, you define conditions for when the job has to be done, and those decide when they job is run.

### Setup
Three steps:

* Create a JobService to handle your task.
* Add your JobService to the Android manifest.
* Schedule your task using a JobInfo object.
    * You can define execution conditions when creating this object.

As the names imply, JobService is a kind of Service.  
A little different from DownloadManager.

First, have your class extend JobService.  
Override onStartJob() and onStopJob().  
onStartJob() is called when the job is started.  
onStopJob() is called only when the job is cancelled prematurely.  
You must also call jobFinished() when everything is done to release resources. If you don’t, you could drain battery.  
It isn’t automatically multithreaded, so you’ll have to use AsyncTask.

### WorkManager
Room is awesome for writing SQLite queries, and DB read/writes.  
For your app to be reliable, you need a way to back up the local database on the cloud.  
You also need a reliable way to cache stuff into your DB periodically.  
You also need a way to do all this without draining battery.  
You need a way to keep track of whether you’re connected to the internet or not, and have your app do different things.  
WorkManager is designed to handle all these.

Under the hood, WorkManager may very well be using JobScheduler (or something else).  
WorkManager looks a lot like Volley. You have:

* Worker: the task you want to perform.
* WorkRequest: specify which Worker should run, conditions for the task to be run, whether it is periodic or not.
* WorkManager: queues and manages work requests. It does magical load balancing and battery management, while honoring your constraints.
* WorkStatus: WorkManager provides a LiveData<WorkStatus> for each WorkRequest.

### Typical Workflow
Create a new class extending Worker and implement doWork().  
Create either a OneTimeWorkRequest or a PeriodicWorkRequest using that Worker.  
Enqueue the task using WorkManager.  
Track the WorkStatus using an observer, do something on success, failure.  
Other Features: 

* You can chain jobs with a line or two of code.
* Backwards compatible with old SDKs (unlike JobScheduler).
* Automatic threading.