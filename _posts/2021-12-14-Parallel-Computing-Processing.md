---
title:  "Parallel Processing/Computing Paradigm"
# permalink: /_post/
date:   2021-12-14 1:33:22 +0530
categories:
  - Parallel Processing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Parallel Processing
author: Akhilesh Moghe
show_author_profile: true
---

## Parallel Computing
  - Uses multiple computer cores to attack several operations at once.
  - Parallel Architecture breaks down a job into its component parts that can work in parallel, are mostly independent and multi-task them.
  - These parts are allocated to different processors which execute them simultaneously.
  - The first multi-core processors for Android and iPhone appeared in 2011 [1].
  - IBM released the first multi-core processors for computers ten years before that in 2001 [2].
  - Why???
    - Exponential growth of processing and network speeds.
    - Dual-core, quad-core, 8-core, and even 56-core chips are available[3].
    - With __*Big data*__ and the __*IoT*__ adoptions, applications will soon be required to crunch trillions of data points at once. e.g., in healthcare sector, AI/ML tools will be rifling through the heart rates of a hundred million patients.
  - Parallel Computing ways:
    - Multithreading
      - POSIX, C++11 Threads
        - POSIX Threads of the same process can be scheduled and run on different processors/cores.
    - Multiprocessing
    
## Multiprocessing Vs Parallel Processing
  - Multiprocessing is the use of two or more central processing units (CPUs) within a single computer system. The term also refers to the ability of a system to support more than one processor and/or the ability to allocate tasks between them.
  - parallel processing is the processing of program instructions by dividing them among multiple processors with the objective of running a program in less time with speed and efficiency.
  
## Multiprocessing Vs Distributed Processing
  - Multiprocessing works on the same physical machine/node with multiple processors/cores.
  - Distributed Processing uses Multiprocessing, but Distributed processing needs to have the same program/processes running on multiple machines/nodes, connected over networks.
  
## Concurrency Vs Parallelism
  - Concurrency means that an application is making progress on more than one task at the same time (concurrently).
  - Concurrency means executing multiple tasks at the same time but not necessarily simultaneously.
  - Parallelism means that an application splits its tasks up into smaller subtasks which can be processed in parallel, for instance on multiple CPUs at the exact same time.
  - In single-core CPU, you may get concurrency but NOT parallelism.
  - A system is said to be concurrent if it can support two or more actions __*in progress*__ at the same time.
  - A system is said to be parallel if it can support two or more actions executing __*simultaneously*__.
  - Concurrency and Parallelism are conceptually overlapped to some degree, but “in progress” clearly makes them different.
  - Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.
  
## Cores Vs Processors & Hardware Threads
- A core is the most basic entity within a processor (CPU) that can completely handle execution of a program.
- CPU/Processor is an electronic circuit inside the computer that carries out instruction to perform arithmetic, logical, control and input/output operations
- While the core is an execution unit inside the CPU that receives and executes instructions.

### Cores Vs Processors

  ![Cores Vs Processors](/assets/images/cores-vs-processors-2.png)

  ---

  An image may say more than a thousand words:
  ![Cores Vs Processors](/assets/images/cores-vs-processors.png)

### Hyperthreading or Simultaneous Multi-Threading (SMT)
- Hyperthreading - Intel
- Simultaneous Multi-Threading (SMT) - AMD
- Adding a second core to the CPU adds about 10% to the size and complexity, while doubling the performance.
- By adding hypertrheading to the mix you are improving that performance advantage to about 230%—all while using the same measly 15W of power.
- More cores and threads your CPU has, the more tasks it’s able to perform Concurrently & Simultaneously.
- Hyperthreading or Simultaneous Multi Threading (SMT) allows a core to store two processes at the same time, and operate on them separately. Each of these processes are stored in a *<u>hardware thread</u>*, also known as *<u>Hart</u>* or a *<u>thread</u>*.
- A CPU with 2 cores and 4 threads is a dual-core processor, which normally has only 2 threads but with hyperthreading/simultaneous multi-threading has 4 threads. So instead of being limited to running 2 instructions/tasks efficiently, it can run 4.
- A single complete “CPU” may be replicated onto a chip die, with each replication called a “core”. so a “CPU chip” with 2 such replications is said to have 2 cores. furthermore, each core has at least 1 “thread” = instruction stream (program), but it may have 2 or more threads. “2 cores and 4 threads” would usually mean each of the 2 CPU cores has 2 threads each. “threads” are the modern way of improving performance through parallel code execution (>=2 programs running concurrently).


## Tools/APIs/Libraries for Parallel Processing
### OpenMP
- OpenMP (Open Multi-Processing) are APIs that support multi-platform __*shared-memory multiprocessing*__ programming in __*C*__, __*C++*__ on many platforms, *<u>instruction-set architectures</u>* and *<u>operating systems</u>*, including *Solaris*, *AIX*, *FreeBSD*, *HP-UX*, *Linux*, *macOS*, and *Windows*.
- It consists of a set of __*compiler directives*__, __*library routines*__, and environment variables that *<u>influence run-time behavior</u>*.
- Used for __*Parallel Processing*__ applications for desktops and super-computers.
- OpenMP is used for *<u>parallelism within a (multi-core) node</u>*, while __*Message Passing Interface(MPI)*__ is used for *<u>parallelism between nodes in a cluster</u>*.

#### Design
- OpenMP is an implementation of __*multithreading*__, a method of parallelizing whereby a primary thread (a series of instructions executed consecutively) forks a specified *<u>number of sub-threads</u>* and the system divides a task among them. The threads then *<u>run concurrently</u>*, with the runtime environment *<u>allocating threads to different processors</u>*.
- The *<u>section of code that is meant to run in parallel is marked accordingly</u>*, with a __*compiler directive*__ that will cause the threads to form before the section is executed.
- Each __*thread*__ has an __*id*__ attached to it which can be obtained using a function (called __*omp_get_thread_num()*__).
- The thread id is an integer, and the primary thread has an id of 0.
- After the execution of the parallelized code, the threads *<u>join back</u>* into the primary thread, which continues onward to the end of the program.
- By default, *<u>each thread executes the parallelized section of code independently</u>*.
- Work-sharing constructs can be used to divide a task among the threads so that each thread executes its allocated part of the code.
- Both __*task parallelism*__ and __*data parallelism*__ can be achieved using __*<u>OpenMP</u>*__ in this way.
- The runtime environment allocates threads to processors depending on usage, machine load and other factors. The runtime environment can assign the number of threads based on environment variables, or the code can do so using functions.
- The OpenMP functions are included in a header file labelled __*omp.h*__ in C/C++.


## Distributed Shared Memory (DSM) 
- [Distributed Shared Memory (DSM)](https://en.wikipedia.org/wiki/Distributed_shared_memory) is a form of memory architecture where *<u>physically separated memories</u>* can be addressed as *<u>one logically shared address space</u>*.
- "Shared" does not mean that there is a single centralized memory, but that the *<u>address space is "shared"</u>* (same physical address on two processors refers to the same location in memory).
- __*Distributed global address space (DGAS)*__, is a similar term for a wide class of software and hardware implementations, in which *<u>each node of a cluster has access to shared memory in addition to each node's non-shared private memory</u>*.


###### References:
  - 
  
  
  
