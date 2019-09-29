## Week 5 - Day 1
### Library
Use a “library” to add additional functions and methods and classes, etc.  
These prebuilt collections allow for things like user interface, graphics output, etc.  
SFML - simple fast multimedia library  
Simple api for opening windows, mouse response, drawing, etc  
JSON - format for saving “objects”. Can use a library to load and save info with this  

**Types of Libraries**   
Headers - always included - what the library machine code.  

* These are “templates”  
* Easiest to work with,  
* Just #include file to use  


“Source code” - includes everything  
Library developers don’t want people to see the written C++ code as people can just steal and use it  
They distribute the machine code (.o files)  
Usually small library (.hpp and .cpp files)  
Compiled library - headers and necessary additional code specific for what you need to compile.  
All header files with the precompiled formatted code.  

You need to get the actual files on your computer and then compile correctly to use library.  

.lib .so .a .dy_lib are common formats for libraries  
Compiler needs to know where the library files are.  
```clang++ -I/path/to/headers``` (upper case i)  

Once compiled you have to link files together  
```clang++ -L/path/to/library -llibraryname``` (lower case l)  
-l adds the lib to library name when searching. -lsfml is looking for libsfml.libraryfiletype

Writing all this stuff out to compile code is a PITA.  
Build Systems keep track of a lot of these, makes it easier to compile  
Xcode has Build Xcode   
Can specify what libraries and where to find, when it compiles it will do the lines automatically.  
This is still really complicated  

Command line is commonly used by developers. There is a command line builder
Make - build too  

CMake - specifically for C++  
Will automatically run steps needed, but only the ones needed (say you have 100 cpp files, edit one, only one needs to be recompiled and linked)  
Tell CMake what files are needed in a makefile  
Dont build from scratch, find one and edit as needed.  
Common to write what we want, and have a program write a makefile to create.  
CMakeLists.txt  
Run through CMake to get the makefile or an Xcode project or VS project or Ninja, etc.

Use package managers to manage dependancies and updates to new versions
Homebrew is a good package manager

cmake file into main folder. Have src folder for code. Build folder to run make. Xcode folder is cmake -G Xcode .. (run in Xcode folder .. is where the cmakelists.txt file is)

 