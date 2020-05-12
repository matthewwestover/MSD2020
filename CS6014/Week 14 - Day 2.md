## Week 14 - Day 2
### Course Review - Final Question Examples
Networking Stack - Deploy a new protocol somewhere in the stack. Where is easiest where is hardest?

* Easiest is Application, it is done all the time. It is all user mode code and only two nodes are involved the end points.
* Transport Layer is hard to change, very well established. In theory it should be easy, but the long standing protocols involve middle nodes looking at this stuff, which means it is hard. 
* Network Layer is hard as it is all nodes in the path
* Link Layer is easier as it is just two nodes, the next hop
* Physical Layer - cant change physics so its very hard

Wifi - Link Layer sends ACK frames back and forth. Why does the router send an ACK back? 

* If you don’t get an ACK you can assume a packet was lost
* As to the wifi router is the first hop, even not being responsible for the TCP guarantee, it is the first place to see if a packet it was lost. 
* Just a quicker way to check instead of the round trip time 

Website with big file to download - includes the SHA256 version of the file to verify against. With no encryption on the site is this protecting anyone? 

* Helps verify against corruption of the file. 
* However an attacker who changes the file, can also update the SHA256 to match the changed file
* To make this beneficial - can sign. Or you can just encrypt the connection - HTTPS makes needing the hashed version of the file meaningless. 

Run a webform. Userlist is public. Solution during attack at guessing passwords. Simple solution is lock account for an hour after 5 failed attempts. Does this make things “better”?

* Good at preventing password guessing. 
* Easy way for a DOS attack
* All public members can be attacked and it is easy to lock them out 
* Don’t make list public, be better at throttling (lock IPs for example)

UNIX Servers commonly use a separate user account for each service. User WWW for web stuff User SQL for database stuff. Why is this good?

* If one account is compromised the entire server isn’t fucked. 
* Each has their own unique secret stuff, they each have different access allowances

What does write xor execute (W^R) mean?

* If something is writable (content can be changed) it cant be executed. 
* If something can be executed it cant be written. 
* These are pages in memory. Each page is marked as W or R

What could a group with a compromised trusted secure key do?

* Can easily do man in the middle attacks.
* Can easily spoof certs that say “yes I’m google, yes I’m Wells Fargo"
* Can get passwords from people
* Can maliciously get software installed (look this is really Microsoft ;))
