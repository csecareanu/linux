
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
This is a subdirectory containing one entry for each file which the process has open, named by its file descriptor, and which is a symbolic link to the actual file.  Thus, 0 is standard input, 1 standard output, 2 standard error, and so on.

```
ls -l
lr-x------    1 ic       ic              64 Jul 12 00:33 0 -> /dev/null
l-wx------    1 ic       ic              64 Jul 12 00:33 1 -> /dev/console
lrwx------    1 ic       ic              64 Jul 12 00:33 10 -> anon_inode:[eventfd]
lrwx------    1 ic       ic              64 Jul 12 00:33 11 -> anon_inode:[eventfd]
lrwx------    1 ic       ic              64 Jul 12 00:33 13 -> socket:[16270]
lrwx------    1 ic       ic              64 Jul 12 00:33 14 -> /dev/shm/onoffstate
```

For file descriptors for pipes and sockets, the entries will be symbolic links whose content is the file type with the inode.  A `readlink` call on this file returns a string in the format:\
`type:[inode]`

For example, `socket:[2248868]` will be a socket and its inode is 2248868.  For sockets, that inode can be used to find more information in one of the files under `/proc/net/`.

For file descriptors that have no corresponding inode (e.g., file descriptors produced by `epoll_create`, `eventfd`, `inotify_init`, `signalfd`, and `timerfd`, the entry will be a symbolic link with contents of the form:\
`anon_inode:<file-type>`

In some cases, the file-type is surrounded by square brackets.

For example, an epoll file descriptor will have a symbolic link whose content is the string `anon_inode:[eventpoll]`.

### /proc/[pid]/fdinfo/ 
This is a subdirectory containing one entry for each file which the process has open, named by its file descriptor. The files in this directory are readable only by the owner of the process. The contents of each file can be read to obtain information about the corresponding file descriptor. The content depends on the type of file referred to by the corresponding descriptor.
 
For regular files and directories, we see something like:\
```
$ cat /proc/12015/fdinfo/4
pos:    1000
flags:  01002002
mnt_id: 21
```

The `pos` field is a decimal number showing the current file offset. The `flags` field is an octal number that displays the file access mode and file status flags (see `open`).\
The `mnt_id` field is the ID of the mount point containing this file.\
See the description of `/proc/[pid]/mountinfo`.

For `eventfd` file descriptors (see `eventfd`), we see the following fields:\
```
pos: 0
flags:    02
mnt_id:   10
eventfd-count:               40
```
`eventfd-count` is the current value of the eventfd counter, in hexadecimal.

For `epoll` file descriptors (see `epoll`), we see the following fields:
```
pos: 0
flags:    02
mnt_id:   10
tfd:        9 events:       19 data: 74253d2500000009
tfd:        7 events:       19 data: 74253d2500000007
```

Each of the lines beginning `tfd` describes one of the file descriptors being monitored via the epoll file descriptor (see `epoll_ctl` for some details).  The `tfd` field is the number of the  file  descriptor. 

For `signalfd` file descriptors (see `signalfd`), we see the following fields:
```
pos: 0
flags:    02
mnt_id:   10
sigmask:  0000000000000006
```

`sigmask` is the hexadecimal mask of signals that are accepted via this `signalfd` file descriptor.  (In this example, bits 2 and 3 are set, corresponding to the signals `SIGINT` and `SIGQUIT`; see `signal`.)

### /proc/[pid]/io
This file contains I/O statistics for the process, for example:
```
# cat /proc/3828/io
rchar: 323934931 // The  number  of bytes which this task has caused to be read from storage.  
                 //This is simply the sum of bytes which this process passed to read(2) and similar system calls. 
                 // It includes things such as terminal I/O and is unaffected by whether or not actual physical disk I/O was required
wchar: 323929600 // The number of bytes which this task has caused, or shall cause to be written to disk.  Similar caveats apply here as with rchar.
syscr: 632687    // Attempt to count the number of read I/O operations—that is, system calls such as read(2) and pread(2).
syscw: 632675    // Attempt to count the number of write I/O operations—that is, system calls such as write(2) and pwrite(2).
read_bytes: 0    // Attempt to count the number of bytes which this process really did cause to be fetched from the storage layer.  
                 // This is accurate for block-backed filesystems.
write_bytes: 323932160 //  Attempt to count the number of bytes which this process caused to be sent to the storage layer.
cancelled_write_bytes: 0
```

### /proc/[pid]/maps
A file containing the currently mapped memory regions and their access permissions.  See `mmap` for some further information about memory mappings.

The format of the file is:
```
address           perms offset  dev   inode       pathname
00400000-00452000 r-xp 00000000 08:02 173521      /usr/bin/dbus-daemon
00651000-00652000 r--p 00051000 08:02 173521      /usr/bin/dbus-daemon
00652000-00655000 rw-p 00052000 08:02 173521      /usr/bin/dbus-daemon
00e03000-00e24000 rw-p 00000000 00:00 0           [heap]
00e24000-011f7000 rw-p 00000000 00:00 0           [heap]
...
35b1800000-35b1820000 r-xp 00000000 08:02 135522  /usr/lib64/ld-2.15.so
35b1a1f000-35b1a20000 r--p 0001f000 08:02 135522  /usr/lib64/ld-2.15.so
35b1a20000-35b1a21000 rw-p 00020000 08:02 135522  /usr/lib64/ld-2.15.so
35b1a21000-35b1a22000 rw-p 00000000 00:00 0
35b1c00000-35b1dac000 r-xp 00000000 08:02 135870  /usr/lib64/libc-2.15.so
35b1dac000-35b1fac000 ---p 001ac000 08:02 135870  /usr/lib64/libc-2.15.so
35b1fac000-35b1fb0000 r--p 001ac000 08:02 135870  /usr/lib64/libc-2.15.so
35b1fb0000-35b1fb2000 rw-p 001b0000 08:02 135870  /usr/lib64/libc-2.15.so
...
f2c6ff8c000-7f2c7078c000 rw-p 00000000 00:00 0    [stack:986]
...
7fffb2c0d000-7fffb2c2e000 rw-p 00000000 00:00 0   [stack]
7fffb2d48000-7fffb2d49000 r-xp 00000000 00:00 0   [vdso]
```

The address field is the address space in the process that the mapping occupies. The perms field is a set of permissions:
```
r = read
w = write
x = execute
s = shared
p = private (copy on write)
```

The offset field is the offset into the file/whatever; dev is the device (major:minor); inode is the inode on that device.  0 indicates that no inode is associated with the memory region, as would be the case with BSS (uninitialized data).

The pathname field will usually be the file that is backing the mapping.  For ELF files, you can easily coordinate with the offset field by looking at the Offset field in the ELF program headers (readelf -l).

There are additional helpful pseudo-paths:
```
     [stack]
            The initial process's (also known as the main thread's) stack.

     [stack:<tid>] (since Linux 3.4)
            A thread's stack (where the <tid> is a thread ID).  It corresponds to the /proc/[pid]/task/[tid]/ path.

     [vdso] The virtual dynamically linked shared object.

     [heap] The process's heap.
```

If  the  pathname  field  is  blank,  this  is  an  anonymous  mapping as obtained via the `mmap` function.  There is no easy way to coordinate this back to a process's source, short of running it through `gdb`, `strace`, or similar.


### /proc/[pid]/mountinfo
This file contains information about mount points.

### /proc/[pid]/mounts
This  is a list of all the filesystems currently mounted in the process's mount namespace.

### /proc/[pid]/mountstats
This file exports information (statistics, configuration information) about the mount points in the process's mount namespace.

### /proc/[pid]/oom_adj
This  file can be used to adjust the score used to select which process should be killed in an out-of-memory (OOM) situation.

### /proc/[pid]/oom_score
This  file  displays  the  current score that the kernel gives to this process for the purpose of selecting a process for the OOM-killer.

### /proc/[pid]/pagemap
This file shows the mapping of each of the process's virtual pages into physical page frames or swap area.  It contains one 64-bit value for each virtual page, with the bits set as follows:
```
63     If set, the page is present in RAM.
62     If set, the page is in swap space
61     The page is a file-mapped page or a shared anonymous page.
54-0   If the page is present in RAM (bit 63), then these bits provide the page frame number, which can be used to index /proc/kpageflags and /proc/kpagecount.
       If the page is present in swap (bit  62),  then bits 4-0 give the swap type, and bits 54-5 encode the swap offset.
```
To employ `/proc/[pid]/pagemap` efficiently, use `/proc/[pid]/maps` to determine which areas of memory are actually mapped and seek to skip over unmapped regions.

### /proc/[pid]/root
UNIX  and Linux support the idea of a per-process root of the filesystem, set by the `hroot` ystem call.  This file is a symbolic link that points to the process's root directory.

### /proc/[pid]/smaps
This  file  shows  memory consumption for each of the process's mappings.  (The pmap(1) command displays similar information, in a form that may be easier for parsing.)  For each mapping there is a series of lines such as the following:
```
00400000-0048a000 r-xp 00000000 fd:03 960637       /bin/bash
Size:                552 kB
Rss:                 460 kB
Pss:                 100 kB
Shared_Clean:        452 kB
Shared_Dirty:          0 kB
Private_Clean:         8 kB
Private_Dirty:         0 kB
Referenced:          460 kB
Anonymous:             0 kB
AnonHugePages:         0 kB
Swap:                  0 kB
KernelPageSize:        4 kB
MMUPageSize:           4 kB
Locked:                0 kB
```

The first of these lines shows the same information as is displayed for the mapping in /proc/[pid]/maps.  The remaining lines show the size of the mapping, the amount of the mapping that is currently  resident  in RAM  ("Rss"), the process' proportional share of this mapping ("Pss"), the number of clean and dirty shared pages in the mapping, and the number of clean and dirty private pages in the mapping.  "Referenced" indicates the amount of memory currently marked as referenced or accessed.  "Anonymous" shows the amount of memory that does not belong to any file.  "Swap" shows how much would-be-anonymous memory is also  used,  but out on swap.

The "KernelPageSize" entry is the page size used by the kernel to back a VMA.  This matches the size used by the MMU in the majority of cases.  However, one counter-example occurs on PPC64 kernels whereby a kernel using 64K as a base page size may still use 4K pages for the MMU on older processors.  To distinguish, this patch reports "MMUPageSize" as the page size used by the MMU.

The "Locked" indicates whether the mapping is locked in memory or not.

"VmFlags" field represents the kernel flags associated with the particular virtual memory area in two letter encoded manner.  The codes are the following:
```
rd  - readable
wr  - writable
ex  - executable
sh  - shared
mr  - may read
mw  - may write
me  - may execute
ms  - may share
gd  - stack segment grows down
pf  - pure PFN range
dw  - disabled write to the mapped file
lo  - pages are locked in memory
io  - memory mapped I/O area
sr  - sequential read advise provided
rr  - random read advise provided
dc  - do not copy area on fork
de  - do not expand area on remapping
ac  - area is accountable
nr  - swap space is not reserved for the area
ht  - area uses huge tlb pages
nl  - non-linear mapping
ar  - architecture specific flag
dd  - do not include area into core dump
sd  - soft-dirty flag
mm  - mixed map area
hg  - huge page advise flag
nh  - no-huge page advise flag
mg  - mergeable advise flag
```
 
### /proc/[pid]/stack
This file provides a symbolic trace of the function calls in this process's kernel stack.

```
[<c006cb50>] futex_wait_queue_me+0xf4/0x158
[<c006d318>] futex_wait+0xf0/0x250
[<c006ec54>] do_futex+0xd4/0xa5c
[<c006f720>] SyS_futex+0x144/0x164
[<c000e87c>] __sys_trace_return+0x0/0x24
[<ffffffff>] 0xffffffff
```

### /proc/[pid]/stat
Status information about the process. This is used by `ps`.  It is defined in the kernel source file fs/proc/array.c.

###  /proc/[pid]/statm
Provides information about memory usage, measured in pages.  The columns are:
```
size       (1) total program size
           (same as VmSize in /proc/[pid]/status, measured in pages)
resident   (2) resident set size
           (same as VmRSS in /proc/[pid]/status, measured in pages)
share      (3) shared pages (i.e., backed by a file)
text       (4) text (code)
lib        (5) library
data       (6) data + stack
dt         (7) dirty pages
```

```
cat statm
43740 2020 1642 175 0 37596 0
```

### /proc/[pid]/status
Provides much of the information in /proc/[pid]/stat and /proc/[pid]/statm in a format that's easier for humans to parse.  Here's an example:
```
$ cat /proc/$$/status
    * Name: Command run by this process.
    Name:   bash
    
    * State: Current state of the process.  One of "R (running)", "S (sleeping)", "D (disk sleep)", "T (stopped)", "T (tracing stop)", "Z (zombie)", or "X (dead)".
    State:  S (sleeping)
    
    * Tgid: Thread group ID (i.e., Process ID).
    Tgid:   3515
    
    * Pid: Thread ID (see gettid(2)).
    Pid:    3515
    
    * PPid: PID of parent process.
    PPid:   3452
    
    TracerPid:      0
    
    * Uid, Gid: Real, effective, saved set, and filesystem UIDs (GIDs).
    Uid:    1000    1000    1000    1000
    Gid:    100     100     100     100
    
    * FDSize: Number of file descriptor slots currently allocated.
    FDSize: 256
    
    Groups: 16 33 100
    
    * VmPeak: Peak virtual memory size
    VmPeak:     9136 kB
    
    * VmSize: Virtual memory size.
    VmSize:     7896 kB
    
    * VmLck: Locked memory size (see mlock(3))
    VmLck:         0 kB
    
    * VmPin: Pinned memory size. These are pages that can't be moved because something needs to directly access physical memory.
    VmPin:         0 kB
    
    * VmHWM: Peak resident set size ("high water mark").
    VmHWM:      7572 kB
    
    * VmRSS: Resident set size.
    VmRSS:      6316 kB
    
    * VmData, VmStk, VmExe: Size of data, stack, and text segments.
    VmData:     5224 kB
    VmStk:        88 kB
    VmExe:       572 kB
    
    * VmLib: Shared library code size.
    VmLib:      1708 kB
       
    * VmPTE: Page table entries size
    VmPTE:        20 kB
    
    * VmPMD: Size of second-level page tables
    VmPMD:         4 kB
    
    * VmSwap: Swapped-out virtual memory size by anonymous private pages; shmem swap usage is not included
    VmSwap:        0 kB
    
    * Threads: Number of threads in process containing this thread.
    Threads:        1
    
    SigQ:   0/3067
    SigPnd: 0000000000000000
    ShdPnd: 0000000000000000
    SigBlk: 0000000000010000
    SigIgn: 0000000000384004
    SigCgt: 000000004b813efb
    CapInh: 0000000000000000
    CapPrm: 0000000000000000
    CapEff: 0000000000000000
    CapBnd: ffffffffffffffff
    CapAmb:   0000000000000000
    Seccomp:        0
    
    * Cpus_allowed: Mask of CPUs on which this process may run
    Cpus_allowed:   00000001
    
    * Cpus_allowed_list: Same as previous, but in "list format"
    Cpus_allowed_list:      0
    
    * Mems_allowed: Mask of memory nodes allowed to this process
    Mems_allowed:   1
    
    * Mems_allowed_list: Same as previous, but in "list format" 
    Mems_allowed_list:      0
    
    * voluntary_ctxt_switches, nonvoluntary_ctxt_switches: Number of voluntary and involuntary context switches
    voluntary_ctxt_switches:        150
    nonvoluntary_ctxt_switches:     545
``` 
 
### /proc/[pid]/task
The name of each subdirectory is the numerical thread ID of the thread.
 
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
