## Week 5 - Day 2
### DevOps
Pro-grade build tools  
Lots of libraries used, dependancies  
Maven and Gradle are the two common systems to package these together  

**Gradle**  
Has auto dependency fetching. Say you want to use X library it will fetch everything it needs to run  
Has auto tasks for testing, output multiple version of the build  
Newer than Maven  
Default for Android apps  
Uses language Groovy for set up - mix between java and javascript  
Everything you want Gradle to do is called a “task”  
Build info goes into build.gradle file  
Lots of good defaults for everything  
Gradle auto looks for src/main to build  
Java programs are easily compiled to ‘jar’ files  

```
plugins {
    Id ‘java'
}
Sourcesets {
    Main {
        Java {
            srcDirs = [’src’] // not using src/main for the project files
        }
    }
}
Jar {
    Manifest {
        Attributes ‘Main-Class’: ‘MainClass.MainClass’ // where to look for public static main(){}
    }
}
Dependencies {
    // additional libraries program uses
}
```

To bundle run gradle jar and it will create a jar file  
To get Intellij to use this create the file then reopen project and it will ask to import as a gradle

Project is now built and runnable on our computer.  
We need to deploy it somehow  
Just placing the .jar on the server and running doesn’t mean it will work

**Docker**  
Virtual container - called images  
Basically like a virtual new OS (clean install)  
Allows us to test a file like its not running on our own machine  
If it works in an container we know we didn’t miss anything for it to run on the server  
Also commonly used for massive program testing on different platforms/OS/RAM/CPU/ etc.  

For java this starts with a base linux OS and a java package  
We add our jar + html/css/js files in the resources folder  

Once this is created we can run the jar from the container  
We are running it on a Mac, but not really, it's running on a virtual linux OS.  
We can test while running on our computer and then ship the entire docker container/image to a server like Azure  

To make a Docker container/image we create a Dockerfile  
Put Dockerfile in top level of project folder  
These look a bit weird and can get complicated  

```
FROM openjdk:12 //fresh stripped down linux os with java. 12 is the java version
WORKDIR /
ADD build/libs/myChatServer.jar ChatServer.jar //puts jar in container at /ChatServer.jar
ADD resources/ resources/ //add the resources folder
EXPOSE 8080 // all ports are blocked by default, this lets us access this port
CMD java -jar ChatServer.jar //runs the jar file
```

To run this DockerFile:  
```docker build -t containerName .``` the dot can be replaced with file path to where it needs to open the file  
```docker run -p8080:8080 containerName```
Now we can connect to localhost:8080 to access the container running the program

```docker images``` this shows all current docker containers you have created. 

Any updates to project needs to have the docker build done again (and any possible updates to docker file) then push the new image up  

To deploy to azure there is a easy way (not a real company product way you should do it):  
Docker can create a public image anyone can access. 
The url docker gives us we can give to azure and azure will run it  
The docker build -t line has to use username/reponame:projectname  
Then run ```docker push username/reponame:projectname``` will push it to dockers online hosting  
Azure ```—image docker.io/username/reponame:projectname``` run this on the cloud terminal azure website uses
