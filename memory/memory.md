

Pring virtual file that reports the amount of available and used memory.
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
