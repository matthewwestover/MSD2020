## Week 13 - Day 2
### Intrusion Detection

#### Types of Intruders
Criminals  
Hactivists  
State sponsored attackers  
??? 

#### Intruder Skills
“Script-Kiddies” — unsophisticated. Use existing tools with (maybe) a little ability to slightly modify them. Exploit existing unpatched vulnerabilities  
Midrange hackers — Can modify existing tools, or apply exploits to new software etc. More dangerous than script kiddies  
Professional Hackers — Highly technically skilled. These are the sorts of folks employed by government agencies (though many do not work for governments). Can find/develop new exploits

#### Pre-Intrusion
Identify targets  
Gather info on: 

* network organization
* operating system/software packages/versions 
* info on users/administrators

#### Intruding
Hacker exploits a vulnerability to gain initial access to a system on the network  
Uses any of the techniques we’ve discussed: buffer overflow, code injection, side channel attack to learn a password, etc.  
Or a social engineering attack (a phishing email to trick a user into divulging their login/password), etc  
After this step, they have a foothold in the system

#### Privilege Escalation
Attacker tries to get more powerful access within the network  
They could use another exploit on the machine they’ve compromised  
They might install some sort of surveillance to try to sniff an administrator password, etc

#### Exploitation
Once the attacker has the desired privileges, they perform their exploit:

* Data exfiltration
* Vandalism
* Surveillance

#### Maintaining Access
The attacker may want to keep the compromised system under their control by:

* Installing a backdoor, or some other form of remote access
* Stealing user credentials so they can remotely log in “legitimately” 
* Disable antivirus or other security software/software updates

#### Covering Tracks
Attacker cleans up after their attack to avoid detection/traceability  
Deleting any files containing exploit tools  
Editing logs to remove suspicious actions

#### Detection
The sooner an intruder is detected, the less damage they can cause  
We can measure many facets of system activity using various “sensors” that can be placed a various locations in a network  
“Analyzers” interpret the sensor data and try to detect intrusion events An undetected intruder is a “false negative”. 
A legitimate activity “detected” as an intrusion is a “false positive”
Assuming intrusions are rare, it’s difficult to have few false positives while catching actual intrusions!

#### Sensors
What can we measure about a system in operation?  
CPU instructions/memory accesses — authoritative, but impractical System calls (maybe just a subset?)  
Network traffic  
Modifications to system files  
Need to balance the impact/invasiveness of recording all activities, the expense of storing/analyzing sensor data, and the likelihood of capturing useful information about an intrusion

#### Anomaly Detection
An intrusion is a rare/unusual event (hopefully)  
One way to analyze sensor data is to try to detect anomalous events  
This requires us to understand what “normal” behavior looks like, and to detect deviations from it   
These might be based on: statistical model, expert knowledge, machine learning based models  
Again, difficult to balance creating too many false positives, with missing false negatives  
Generally difficult to train ML/statistical models because there aren’t many examples of “intrusive” behavior

#### Signature Based
Learn or specify “signatures” of likely behaviors of attackers  
Example behavior: port-scanning (trying to connect to many ports on many machines in a network)  
This is effective at detecting “script-kiddie” type attacks, but is less effective against new attacks mounted by better hackers

#### Distributed Intrusion Detection
By monitoring sensors from a variety of locations in the network, we have a “higher level” view of the network operation  
These systems have better chance of detecting events such as virus/ worm propagation since they involve network traffic/intrusions across many machines in a network

#### Honeypot
A honeypot is a system that appears to be a vulnerable and valuable target for an attacker  
A “low interaction” honeypot has “server facades” that appear to be exploitable server software, but don’t actually  
a “high interaction” honeypot may have legitimate software, but reduced functionality and intense logging  
An attacker who finds one of these and tries to exploit it will be detected (hopefully) before they attempt to breach the rest of the network  
It’s easy to detect intrusion attempts because any connections are probably attempted intrusions!

#### Post Detection
A human expert can examine the sensor data/analysis to decide whether it is a false positive, or a legitimate intrusion event  
Then, the security team works to patch whatever vulnerabilities led to the intrusion and to respond to the intrusion (uninstalling malicious software, auditing what information was leaked, reprimanding a user that gave away their password, etc)