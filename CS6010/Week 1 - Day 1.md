## Week 1 - Day 1
### The Terminal
Uses the Command Line Interface to navigate file tree on computer
Create files, run commands, git management and push/pull to/from github
Basic Commands:  
```pwd``` - Present Working Directory, see your current folder and branch from root  
```ls``` - List files/folders in the current directory  
```ls Desktop``` - Pass parameters to change output. In this case see files on desktop  
```ls -l``` - Pass a flag for additional information from command. Who can access. Owner, Last Date Modified, etc.  
```ls -a``` - Includes all hidden files, including the .filename  
```ls -al Desktop``` - Pass Flags then Parameters  
```--help``` - Get additional info on flags and parameters of the command  
```cd``` - Change current directory  
```open``` - "Double Click" command to open files  
Edit has several methods for in terminal edits:  
```nano``` - Most basic CLI editor. Includes commands at bottom of terminal  
Emacs and VIM are the more "scary" options but good to learn.  
```touch <filename>``` - Creates a new empty file  
```mkdir <name>``` - Creates a new folder  
```rm <filename>``` - Deletes permanently, skips trashbin  
```cp <original> <duplicate>``` - Copies file to a new file name  
```mv <original> <new>``` - Move file to a different location. Also used to rename files  
Looking at text files has several methods:  
```less``` - "pager" method to scroll different pages.  
```cat``` - prints the entire file to the terminal.  
```head``` - first ten lines.  
```tail``` - last ten lines.  
```ssh``` - secure shell

### File Structure
View as a tree. An upside-down tree.  
"Root" is the top of the tree. Folders branch out from the root folder.  
Under 'Root': Users, Applications, Library, etc.  
We mostly use stuff: /Users/username  

### Git and GitHub
Git is all local based  
Github is all on the internet  
```git status``` - see what is currently saved out on your local repo.  
```git add``` - add all or specific files to current comments (verison history). 
  
* ```<filename>>```. 
* ```.``` - add all files currently not saved  
* ```-u``` - any files that were edited, but not newly created  
* ```-A``` - all files.  

```git commit``` - adds "save" to local repo in the '.git' folder. 
```git push origin master``` - pushes to github. Have to add "remotes" to local repo to used.  
"origin" - GitHub Remote Name.  
"master" - current working branch.  
