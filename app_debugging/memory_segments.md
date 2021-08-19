# Memory Segments

The OS allocates memory for each process for data and code.

This memory consists of different segments
* stack 
  * for local variables (incl. command line arguments) and environment variables
* heap
  * for dynamic memory
* data segment for:
  * global uninitialised variables (.bss)
  * global initialised variables (.data)
* code segment: typically read-only

Shows memory regions of process <pid>:
```
$ cat /proc/<pid>/maps
```
  
## Global variables (in .bss and .data)

These are the easy ones for the compiler to deal with.

```
#include <stdio.h>
long n = 12345;
char *string = "hello world\n";
int a[256];
...
```
Here
* The global variables **n**, string and the string literal ”**hello world\n**”, will be allocated in **.data**
* The uninitialised global array a will be allocated in **.bss**. The segment .bss is initialised to all zeroes. NB this is a rare case where C will do a default initialisation for the programmer!
  
### The stack
 
