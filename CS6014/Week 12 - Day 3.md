## Week 12 - Day 3
### Denial of Service (DOS)
Most of the attacks we’ve seen involve exploiting a vulnerability to get a program/system to do something it’s not supposed to: e.g. run user code, leak secret information, etc  
These attacks require an attacker to find a flaw an exploit it.  
This is pretty hard to do! It also limits attackers to exploiting systems using such vulnerable software, and the attacks stop working once it gets patched  
Denial of Service instead tries to get a system to NOT do what it IS supposed to do

DOS attacks prevent systems from servicing legitimate user requests They typically don’t require exploiting any particular software vulnerability  
Instead the rely on the fact that it’s easier for users to submit requests than it is for a server to process and respond to them  
Attackers send as many bogus requests as possible in order to overload some aspect of the system, preventing it from serving legitimate requests

Fork Bomb:

```
:(){:|:};: //in bash the equiv of while(true) fork();
```

The above code is a “fork bomb” that basically calls fork() a bunch of time in a loop (that’s the shell version of a forkbomb)  
The “exploit” is that users are allowed to create processes... not really a vulnerability  
The forkbomb overloads the system’s resources: in this case, the data structure containing all the PCBs, the scheduler, etc  
legitimate users programs are “denied service” due to the forkbomb executing

#### Distributed DOS (Botnets)
Your laptop connected via WIFI to the campus network probably doesn’t have the bandwith/computing power to put a dent in google’s mass of servers  
To make attacks damaging, attackers recruits lots and lots of machines to mount an effective attack  
Quite often these are machines infected with viruses/worms. Collectively they are called a “botnet”  
The “owners/controllers” of these botnets harness them for attacks, or rent them out for attacks (DDOSAAS)

##### Case Study: Mirai
Mirai is a botnet, first detected in 2016 (and still active). 
It is made up predominantly of unsecured Internet of Things (IoT) devices (webcams, fridges, washing machines, etc)  
Many of these devices are hooked up to the internet with default passwords  
Mirai reaches out to random IP addresses and tries to hack any vulnerable devices it finds. One researcher found that it took about 1 minute for a new device to be hacked after plugging it in!  
Mirai has been used to mount DOS attacks  
It’s open source now... many imitators and modifications are based on Mirai

#### Network Attacks
These attacks are usually carried out over the internet  
Generally, attackers are trying to overwhelm their target’s network resources (this might be bandwidth, or something related to their OS’s network stack). 
The simplest approach is to simply send as many ping packets at a host as possible  
Assuming we have a single attacking machine, what can go wrong?

##### Problems with Ping
The Ping packet will generate a Pong response  
For each attacking packet you send, you get one back. 
If your resources are >> than your target, this is fine, but not ideal Solution: use ping, but lie about your IP address!

#### IP Spoofing
IP packets contain a source and destination field  
What’s stopping you from “spoofing” the source field? Basically nothing  
We’ll talk about how to (partially) prevent this later  
If you spoof random source fields in your ping packets, pongs get sprayed all over the internet  
This saves the attacker bandwidth, and helps cover their tracks! IP spoofing makes basically all attacks worse/more dangerous

#### Moving the Stack
Above IP-based attacks are TCP attacks  
These attacks rely on higher level details of specific protocols to make them more effective  
Eg: TCP SYN spoofing:  
Recall the TCP handshake: SYN-> SYN-ACK -> ACK -> connected  
By sending a SYN, and ignoring the SYN-ACK, the server spends resources trying to establish the connection.  
Servers usually don't usually allocate lots of resources for establishing TCP connections since it’s assumed to be fast. Once these are all used up, legitimate connections are dropped!

#### Slowloris
Slowloris is an attack at the Application protocol level, attacking HTTP  
Recall that your server probably has something like this:  

```
while (line = socket.readLine() is not empty) { process header line; }
```
Your server probably also has something like 1 thread per connection  
Slowloris slowly sends 1 header line periodically to keep the server thinking the connection is being slowly established  
Systems usually support fewer web server threads than TCP connections, so these attacks require fewer attacker resources to mount!  
The traffic also looks legitimate, so they are hard to detect!

#### Reflection/Amplification Attacks
A reflection attack is when an attacker send a small request to a server (not necessarily the target) with a spoofed source address. The server sends the response to the spoofed address, which is the target  
For lots of cases, the response will be >> the request, amplifying the attacker’s power  
Simple example, DNS: DNS request ~ 60 bytes. DNS response: > 4kB potentially!
The ratio between request and response sizes is the “amplification factor”

#### Controlling Botnets
Don’t really want all your bots to have your IP/domain hardcoded in them...  
Botnets are often controlled via IRC (internet relay chat), a somewhat decentralized chat protocol. (IRC is sort of like slack, and has been around for a long time and is still used)  
Bots join an IRC chatroom and the controller that the controller sends commands to. 
4chan/anonymous users voluntarily joined botnets by running an open source DDOS client called “Low Orbit Ion Cannon” that they could set up to listen in an IRC channel for commands (don’t do this)

#### Stopping DOS
This is tricky!  
Have more computing/network resources than your attacker (expensive, but effective!). 
If it’s possible to classify traffic as benign vs malicious, block the malicious stuff. This is often hard!  
Use timeouts to prevent things like SYN flooding or Slowloris Use rate-limiting: make a client wait to make lots of requests Use traffic filtering/firewalls (more about this next)  
For ISPs: stop your customers from spoofing!

#### Spoofing Prevention
The farther you get from the sender, the harder it is to detect IP spoofing. 
The router closest to the sender knows which addresses exist behind it, and can filter out any packets with an invalid source address  
More complicated/expensive schemes exist to detect/prevent spoofing as well