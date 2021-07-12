
## Process information pseudo-filesystem (/proc)

The proc filesystem is a pseudo-filesystem which provides an interface to kernel data structures.  It is commonly mounted at /proc.  Most of it is read-only, but some files allow kernel variables to be changed.

### /proc/[pid]/cmdline
This  read-only file holds the complete command line for the process, unless the process is a zombie.

### /proc/[pid]/coredump_filter
See "man core"

### /proc/[pid]/cpuset
See "man cpuset"

### /proc/[pid]/cwd
This is a symbolic link to the current working directory of the process.  To find out the current working directory of process 20, for instance, you can do this:\
` $ cd /proc/20/cwd; /bin/pwd`

### /proc/[pid]/environ
This file contains the environment for the process. The entries are separated by null bytes ('\0'), and there may be a null byte at the end. 
Thus, to print out the environment of process 1, you would do:\
`$ strings /proc/1/environ`

### /proc/[pid]/exe
This file is a symbolic link containing the actual pathname of the executed command.

### /proc/[pid]/fd/
 This is a subdirectory containing one entry for each file which the process has open, named by its file descriptor, and which is a symbolic link to the actual file.  Thus, 0 is standard input, 1 standard output, 2
              standard error, and so on.

              For file descriptors for pipes and sockets, the entries will be symbolic links whose content is the file type with the inode.  A readlink(2) call on this file returns a string in the format:

                  type:[inode]

              For example, socket:[2248868] will be a socket and its inode is 2248868.  For sockets, that inode can be used to find more information in one of the files under /proc/net/.

              For file descriptors that have no corresponding inode (e.g., file descriptors produced by epoll_create(2), eventfd(2), inotify_init(2), signalfd(2), and timerfd(2)), the entry will be a symbolic link with contents
              of the form

                  anon_inode:<file-type>

              In some cases, the file-type is surrounded by square brackets.

              For example, an epoll file descriptor will have a symbolic link whose content is the string anon_inode:[eventpoll].

### Virtual file that reports the amount of available and used memory.

```
cat /proc/meminfo

MemTotal:       10797240 kB
MemFree:         3904600 kB
MemAvailable:    8335240 kB
Buffers:          243076 kB
Cached:          4309968 kB
SwapCached:        25044 kB
Active:          2808616 kB
Inactive:        3653700 kB
Active(anon):     631308 kB
Inactive(anon):  1297220 kB
Active(file):    2177308 kB
Inactive(file):  2356480 kB
Unevictable:          32 kB
Mlocked:              32 kB
SwapTotal:        999420 kB
SwapFree:         578048 kB
Dirty:                20 kB
Writeback:             0 kB
AnonPages:       1902596 kB
Mapped:           127552 kB
Shmem:             19216 kB
Slab:             284432 kB
SReclaimable:     213112 kB
SUnreclaim:        71320 kB
KernelStack:       10064 kB
PageTables:        32544 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     6398040 kB
Committed_AS:    4620760 kB
VmallocTotal:   34359738367 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      188224 kB
DirectMap2M:     8785920 kB
DirectMap1G:     2097152 kB

```


## Virtual file that reports the amount of available and used memory.

```
free

              total        used        free      shared  buff/cache   available
Mem:       10797240     2054984     3904848       19212     4837408     8335428
Swap:        999420      421408      578012

```


```
vmstat

procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0 421448 3904940 242968 4594336    0    1    10    11    8    2  4  1 95  0  0
```

```
top
```
The top command is useful to check memory and CPU usage per process. It displays information about:
  * uptime
  * average load
  * tasks running
  * number of users logged in
  * number of CPUs/CPU utilization
  * memory/swap system processes

### The detailed description listed below provides an explanation for each value in case you need assistance in analyzing the results.

* Procs
  * r: number of processes waiting for run time.
  * b: number of processes in uninterruptible sleep.

* Memory
  * swpd: amount of virtual memory used.
  * free: amount of idle memory.
  * buff: the amount of memory used as buffers.
  * cache: amount of memory used as cache.
  * 
* Swap
  * si: memory swapped in from disk (/s).
  * so: memory swapped to disk (/s).
* IO
  * bi: Blocks received from a block device (blocks/s).
  * bo: Blocks sent to a block device (blocks/s).
* System
  * in: number of interrupts per second, including the clock.
  * cs: number of context switches per second.

* CPU – These are percentages of total CPU time.
  * us: Time spent running non-kernel code. (user time, including nice time)
  * sy: Time spent running kernel code. (system time)
  * id: Time spent idle. Before Linux 2.5.41, this includes IO-wait time.
wa: Time spent waiting for IO. Before Linux 2.5.41, included in idle.
st: Time stolen from a virtual machine. Before Linux 2.6.11, unknown.
