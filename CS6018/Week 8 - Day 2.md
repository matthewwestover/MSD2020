## Week 8 - Day 2
### Amazon Web Services (AWS)
Big Picture. 
Our app has a Room database.  
We could program security features into the app.  
We could have the app manage user logins and sign-in functionality.  
We could use file backup on the phone.   
Think about “scalability”. 

### Server Backend
The phone is doing too much.  
If there’s shared data to manage across users, you’ll need a location where the logic is handled.  
Why not move a bunch of these features to that remote location?  
This is where a web server comes in.  
Make your Android app lightweight, make the web server do most of the heavy lifting.  
Server backends can have:

* APIs that feed mobile apps with data 
* Databases to manage data across apps 
* Login/security features etc.

### AWS: Backend as a Service
AWS (and other providers) offer an alternative to this approach.  
Instead of writing a web server with all these pieces (login, database etc.), AWS has separate services corresponding to each of these pieces.  
If you want a piece in your mobile app, subscribe to that piece using an API.  
For instance, if you want login functionality, use AWS Cognito. If you want a remote DB, use AWS RDS.  
This is the concept of a Backend as a Service (BaaS).  
Can be more specialized: Mobile BaaS (MbaaS).  

AWS offers global cloud-based products.  
The main ones are:

- Compute 
- Storage 
- Databases 
- Analytics 
- Networking 
- Mobile
- IoT

### AWS: Compute
AWS offers a number of compute services.  
These are cloud based services that allow for computing.  
The primary Compute service in the AWS stable is Amazon EC2.  
EC2 stands for Elastic Compute Cloud.  
It is “elastic” because you can increase or decrease capacity quickly with minimal “friction”.

### AWS: Storage
AWS offers cloud storage services as well.  
The Storage services are fully integrated (accessible from) the Compute services.  
The primary storage services is Amazon S3.  
S3 is viewed as object storage, a service that enables storage and retrieval of arbitrary amounts of data.  
S3 can be accessed from any platform, can be integrated into apps or software.  
Offers security, encryption, automatically distributed storage across several zones.  
You can run “Big Data Analytics” on the data.

### AWS: Database
AWS offers cloud-based database services also.  
AWS offers both relational and non-relational databases.  
Amazon RDS provides support for MySQL, PostgreSQL, Oracle, SQL Server, and MariaDB.  
Amazon DynamoDB is the AWS NoSQL database.

### AWS: Security
AWS offers a host of security services.  
These services give you very straightforward ways to add sign-on, verification, encryption to your app.   
The primary ones are:

- Amazon Cognito (user identity management for apps).
- AWS Single Sign-On (SSO) (cloud based sign on service).