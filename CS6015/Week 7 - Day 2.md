## Week 7 - Day 2
### Git
Understanding how git works makes it easier to use

**Git =/= GitHub**

Utilizes commands:

* Diff - compare two files and extract the difference (commonly diff -u)
* Patch - apply a diff that may have changes already (patch < p.patch
* Hash with Sha-1 - reliably summarize the content with a short identifier (openssl sha1 file)

Save a diff as a patch file, apply the patch to the other file 

Git init - creates a .git/ folder inside a directory  
Tree .git/ -> displays the tree of stuff inside the .git folder  
Important bits are the HEAD, Objects, refs, 

.git/objects - a key value store  
Values are things like file content  
The keys are sha-1  
Hello, World! \n in a greetings.txt  
Sha1 greetings.txt gives a different number than git’s  
Git hash-object greetings.txt actually sha1s ‘blob 14\0Hello, World!\n’ | openssl sha1

It is not based on a file name, but on the content of the file that is sha1’d

To use git to see inside a file:

```
Git cat-file -p hash#
Git show hash#
Git show shorter_at_least_4_digit_hash_prefix# //still unique
```

Never really do this in practice (git hash-object -w)  - shouldn’t manipulate the git folder directly.  
Use ```git add file```  
This will create the hash in the git object folder for you  
This also creates an index folder. This is a “staging area” for non-committed files  
Git has a garbage collector that will remove files from the object folder

```Git commit -m``` 
Creates two new folders in objects. One is different based on current time, username, etc. (commit #)  the other will be consistent 

```Git cat-file -p``` the commit # will show the author, tree, committer.
```Git cat-file -p``` new tree refers to the initial blob file

Objects in the store looks like commit # -> tree # -> blob (file content)

After first commit the commit# object adds a parent reference to the previous commit on file

For git to know what the most recent commit is, defaults to “master” branch when not working on a new branch.  
This master is in the .git/refs/heads/master which contains the hash # for the most recent commit. 
If you checkout to a new branch you will get a new folder in the heads folder that will hold pointers to the newest commit on that branch

Git reset hash# will just change the head folder pointer to the hash # you pointed it at

Git reset —hard will full change the working directory of your files to what is in within the reset commit hash object

Git show - see the difference between the current commit and the previous commit
This calls a diff on the two trees inside the two trees.  
Any file that is the same is unshown, but all the changes are

Branches are important! Create a new line of code that can get merged into one later  
Great for multiple people working on the same file at once

```Git branch branch-name```. 
.git/refs/heads/ now has branch-name and master within it.  
Does not create new objects at this point.  
Basically just creates a tiny 40 byte file that points to the current commit.   Now there are two pointers to the same commit.  
But now as new files are added/changes the master and branch-name heads will point to different things

Just creating a branch doesn’t move to it  
Git checkout branch-name will change to the branch-name  
.git/HEAD is how you know what branch you are on. The head points to what branch you are on  
Check out also changes the working directory to match what’s in the most recent commit on that branch

To keep all these new trees from being “expensive” they share pointers to different object files even while in different trees. 

With conflicts between new branches we need to merge them together into a new single branch - often onto the master branch  
Do this by git checkout master -> move back to the master branch  
```Git merge branch-name```  
This merge has two parents to show it was two different branches being merging. 
This creates a totally new tree object with the final merged file

This doesn’t make it linear however, it is cyclic. 

To make a more linear history merge with rebase  
Merges into the working branch with changes, and pulls in the updated master branch.  
It takes what’s in the working branch and puts that on top of what is rebased in with master  
Once rebasing with master so the commit chain Is linear, the master head is now orphaned.  
Is possible to do git pull —rebase to pull from remote and keep it linear. 

Rebasing is more complicated to visualize. Git provides less help with rebase as well

Branches are just references to different points in the object store. 

Git commit —amend will commit to the current commit hash and let you update the comment  
Only useful to to change history on yourself. Don’t amend things that are already pushed to other people