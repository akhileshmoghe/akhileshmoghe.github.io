---
title:  "How to limit a process to run on One CPU core in Linux"
date:   2021-12-6 12:33:22 +0530
permalink: /_posts/linux/processes/limit_process_to_cpu_cores
categories:
  - Linux
  - Systems Engineering
  - OS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Systems Engineering
  - OS
author: Akhilesh Moghe
show_author_profile: true
---

This write-up explains different ways to restrict a program to run on a specific CPU core(s). This might be required for some high priority tasks/programs to run dedicatedly on few core on a multi-core/processor systems. This is used to achieve performance benefits from multi-processor systems.

## sched-setaffinity()
  - Linux System Call
  - A process's CPU __*affinity mask*__ determines the *<u>set of CPUs on which it is eligible to run</u>*.
  - Dedicating one CPU to a particular process can be acheved as:
    - setting the affinity mask of that process to specify a single CPU.
    - and setting the affinity mask of all other processes to exclude that CPU.
  - *<u>Restricting a process to run on a single CPU also avoids the performance cost caused by the cache invalidation that occurs when a process ceases to execute on one CPU and then recommences execution on a different CPU</u>*.
  - These restrictions on the actual set of CPUs on which the process will run are silently imposed by the kernel.
  - The affinity mask is actually a per-thread attribute that can be adjusted independently for each of the threads in a thread group.
    - use `-a` option with __*taskset*__ command to add affinity mask to all threads of the process.
  - A child created via fork(2) inherits its parent's CPU affinity mask. The affinity mask is preserved across an execve(2).

### Python 3: sched_setaffinity()
  - Python 3 `os module` supports sched-setaffinity() method
  - Example usage:
  ```
    >>> import os
    >>> os.sched_getaffinity(0)
    {0, 1, 2, 3}
    ----------------

    >>> os.sched_setaffinity(0, {1, 3})
    >>> os.sched_getaffinity(0)
    {1, 3}
    ----------------

    >>> x = {i for i in range(10)}
    >>> x
    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    ----------------

    >>> os.sched_setaffinity(0, x)
    >>> os.sched_getaffinity(0)
    {0, 1, 2, 3}
  ```
  
##### Reference
  - [sched_setaffinity(2)](https://linux.die.net/man/2/sched_setaffinity)
  - [Python 3: sched_setaffinity()](https://docs.python.org/dev/library/os.html#os.sched_setaffinity)
  - [Why does multiprocessing use only a single core after I import numpy?](https://stackoverflow.com/questions/15639779/why-does-multiprocessing-use-only-a-single-core-after-i-import-numpy)

## taskset
  - Linux Command
  - set or retrieve a process's __*<u>CPU affinity*</u>__.
  - Works on a running process and can also launch a new process.
  - CPU affinity is a scheduler property that "bonds" a process to a given set of CPUs on the system.
  - The Linux scheduler will honor the given CPU affinity and the process will not run on any other CPUs.
  - :warning: Note that the Linux scheduler also supports natural CPU affinity:
    - the scheduler attempts to keep processes on the same CPU as long as practical for performance reasons.
    - Therefore, forcing a specific CPU affinity is useful only in certain applications.
  - The CPU affinity is represented as a __bitmask__,
    - with the lowest order bit corresponding to the first logical CPU.
    - and the highest order bit corresponding to the last logical CPU.
  - :memo: Not all CPUs may exist on a given system but a mask may specify more CPUs than are present.
  - :memo: A retrieved mask will reflect only the bits that correspond to CPUs physically on the system.
  - Examples:
  
  ```
  taskset -c 0 mycommand --option    # start a command with the given affinity.
  -------------------

  taskset -c -pa 0 1234              # set the affinity of a running process.
                                     # -a for applying affinity mask to all the threads of the process.
  -------------------

  0x00000001
     is processor #0,
  -------------------

  0x00000003
     is processors #0 and #1,
  -------------------

  0xFFFFFFFF
     is processors #0 through #31,
  -------------------

  32
     is processors #1, #4, and #5,
  -------------------

  --cpu-list 0-2,6
  or
  - c 0-2,6
     is processors #0, #1, #2, and #6.
  -------------------

  --cpu-list 0-10:2
  or
  -c 0-10:2
     is processors #0, #2, #4, #6, #8 and #10. The suffix ":N"
     specifies stride in the range, for example 0-10:3 is
     interpreted as 0,3,6,9 list.
  ```

##### Reference
  - [taskset](https://man7.org/linux/man-pages/man1/taskset.1.html)



## Reference
  - [How to limit a process to one CPU core in Linux?](https://unix.stackexchange.com/questions/23106/how-to-limit-a-process-to-one-cpu-core-in-linux)



  
