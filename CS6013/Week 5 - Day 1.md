## Week 5 - Day 1
### Pipes
Pipes can be used both in programs and in the command shell

```
Sort fiforeader.c | uniq
Cat fifowriter.c | head -7 | tail -5 
```
Sort will sort all characters in a file. | uniq prints the unique characters found in that. 

Cat prints stuff. ```Head-7``` gives the first 7 lines, tail 5 gives the last 5 of the 7 lines taken in the head   
```Head -20 | tail -7``` would print the last 7 lines of the first 20 lines in the file. Effectively lines 13-20

#### FIFO - Named Pipes
Not to be confused with a FIFO scheduler  
It is fully duplex - data flows in both directions  
UNIX treats them as a special file type - with a name it can be referred to as an actual file  
Can connect any pair of processes (parent-child, child-child, anything with correct permissions)

```
#include <sys/types.h>
#include <sys/stat.h>
int mkfifo(const char *path, mode_t mode);
```

This create a FIFO “file” named path. Mode sets the FIFO permissions.
Historically can use mknod() to create a FIFO but it is outdated 

##### FIFO Demo
Run 2 programs
One writes to the screen then reads, the other reads then writes
Both keep going until terminated
Classic FIFO demo

FIFO Writer:

```
Int fd;
// FIFO file path
Char * myfifo = “/tmp/myfifo”;

//Creating named file(FIFO)
// 0666 is permissions for read/write
Mkfifo(myfifo, 0666);

Char arr1[80], arr2[80]
While (1)
{
    // open FIFO for write only
    Fd = open (myfifo, O_WRONLY); // write only mode

    // Take an input arr2 from user.
    // 80 is max length
    fgets(arr2, 80, stdin);

    // Write the input arr2 on Fifo then close it
    Write(fd, arr2, strlen(arr2)+1); //this adds the null terminator for the string
    Close(fd)

    // Open FIFO for read only
    Fd = open (myfifo, O_RDONLY);

    // read from FIFO
    read(fd, arr1, sizeof(arr1));

    // Print the read message
    Printf(“User2: %s\n”, arr1);
}
```

FIFO Reader:

```
Int fd1;
Char* myfifo = “/tmp/myFifo”;
 Mkfifo(myfifo, 0666);

Char str1[80], str2[80];
While (1)
{
    Fd1 = open (myfifo, O_RDONLY);
    Read(fd1, str1, 80);
    Printf(“User1: %s\n”, str1)
    Close(fd1);

    Fd1 = open(myfifo, O_WRONLY);
    fgets(str2, 80, stdin);
    Write(fd1, str2, strlen(str2)+1);
    Close(fd1);
}
```

Mkfifo is a unix command as well, can be done in command line/shell

```
Mkfifo /tmp/pipe

// Console 1
ls -1 > /tmp/pipe

// Console 2
cat < /tmp/pipe
```