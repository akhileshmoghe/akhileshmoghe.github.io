---
title:  "Why prefer Static memory allocation over Dynamic in Embedded Firmmware"
date:   2021-11-12 12:33:22 +0530
categories:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will gather the pointers and conclude on why Static memory allocation is preferred over Dynamic memory in Embedded world.

## Options for Memory Allocation, Pros & Cons
Embedded systems have limited amount of Random Access Memory (RAM) which will be used across different types of memory sections like Stack, Heap, data, variables sections.

### Static memory allocation, without Stack or Heap
  - If all of the memory is allocated Statically, the the amount of RAM being used can be determined at Compile time.
  - This avoids the memory-related issues like: memory leaks, dangling pointers, segmentation fault, etc.
  - We have following options to allocate memory for all kind of data:
    - Global variables.
    - File Static variables.
    - Function Static Variables.
    - Local variables inside functions.
  - 8-bit Microcontrollers like 8051 & PIC are usually designed to work with only Static memory.
  - If there is no support for stack in MCU hardware, all the local variables are stored in the same memory location and that memory location is not used by any other function, even if the function is not running.
  - This prohibits the use of recursion or any other mechanism that requires reentrant code.
    - e.g. an interrupt routine can't call a function that may also be called by the main flow of execution.
    - but this guarantees, no memory related run-time issues.
  - This provides robustness of the program at the cost of programming flexibility & efficiency.
  - This is only feasible for MCUs which are supposed to run a small system & where memory usage is very limited and can be predetermined exactly.
  - For larger systems, this approach of No Stack, No Heap is not feasible, as it will need enourmous amount of RAM for every individual memory allocation, which each need different memory location.
  
### Stack based memory management
  - A block of memory is allocated on Stack for each invocation of a function and the same is regained after function exits.
  - It's difficult for a recursive program to predict how much Stack size will be in worst case.
  - A multi-tasking system needs to have a separate Stack for each task.
  - Plus an extra on efor Interrupts.
  - Stack size is usually pre-determined and statically fixed.
  - Stack Overflow is an untimely issue in static stack size allocation, a task may run into it, which the stack of other task is empty at that time.
  - Best Practice:
    - Allocate the Stack Size to be 50% more than the worst case seen during testing.
    - Many RTOS systems provide a stack size tracing feature.
    - Check for infinite recursion cases, if any.
    - Check for local variables sizes, specially for large sizes, it may extend beyond the top of stack (stack overflow).
    - Check for large variable arrays & recursive functions, they determine the pattern of stack usage.

### Heap based memory management
  - Heap memory is assigned with malloc() and regained with free() in C.
  - Stack management, i.e. memory allocation and regaining is managed by compiler, but the onus of Heap management relies with the programmer.
  - Heap is a major reason of devious bugs in C programs.
    - You freed the memory with free(), but continue to access it with a pointer that is nor NULLED after free() and neither being checked for NULL. This is perfect receipe for untimely crash, unexpected behaviour.
    - You did not freed the memory for later access, but the pointers to that memory itself have gone out of scope. Now you are staring at Memory Leaks, the memory allocations that cannot be regained. It just a matter of time, you will run out of memory due to memory leak tending towards infinity with each call to that buggy code.
  - Memory Fragmentation:
    - This cannot be corrected at the application level.
    - Caused by the blocks of memory available being broken down into smaller pieces as many allocations and frees are performed.
    - [Memory Heap Fragmentation](https://www.youtube.com/watch?v=_G4HJDQjeP8&ab_channel=cpp4arduino)
  - The conclusion is that heap use does involve an element of risk, which the programmer may choose to accept in return for a more flexible, RAM-efficient system.

### Memory Pools
  - Pools, or partitions, of fixed-size memory blocks can be used to completely eliminate the potential for fragmentation.
  - A compromise between static allocation and a general purpose heap.
  - This heap can be tuned at design time for the size of the requests that will be made, as Embedded programs are usually addressing and desined for a particular problem or use-case, not for general purposes.
  - Each pool contains an array of blocks. Unused blocks can be linked together in a list. The pools themselves are declared as arrays.
  - Size is fixed for each pool.
  - This system must be tuned by deciding which size blocks to make available and how many blocks to provide in each pool.
  - Defining pools at sizes which are powers of two (that is, 2, 4, 8, 16, 32, 64, etc.) is a good starting point.

### References
 - [how bad is it to use dynamic datastuctures on an embedded system?](https://stackoverflow.com/questions/1725923/how-bad-is-it-to-use-dynamic-datastuctures-on-an-embedded-system)
 - [Memory Areas and Using malloc()](http://www.nongnu.org/avr-libc/user-manual/malloc.html)
 - [How to Allocate Dynamic Memory Safely](https://barrgroup.com/embedded-systems/how-to/malloc-free-dynamic-memory-allocation)
 - [Use malloc()? Why not?](https://www.embedded.com/use-malloc-why-not/)
 - [Dynamic memory allocation in embedded C](https://stackoverflow.com/questions/33430900/dynamic-memory-allocation-in-embedded-c)
 - [The Concept of Heap and Its Usage in Embedded Systems](https://open4tech.com/concept-heap-usage-embedded-systems/)
 - [Dynamic Memory Allocation - Just Say No](https://www.embeddedcomputing.com/technology/software-and-os/ides-application-programming/dynamic-memory-allocation-just-say-no)
 - [Why shouldn't we have dynamic allocated memory with different size in embedded system](https://stackoverflow.com/questions/21370410/why-shouldnt-we-have-dynamic-allocated-memory-with-different-size-in-embedded-s)
 - [Mastering stack and heap for system reliability: Part 1 – Calculating stack size](https://www.embedded.com/mastering-stack-and-heap-for-system-reliability-part-1-calculating-stack-size/)
 - [Dynamic memory and heap contiguity](https://www.embedded.com/dynamic-memory-and-heap-contiguity/)
 - [use of malloc in embedded c [closed]](https://stackoverflow.com/questions/37812732/use-of-malloc-in-embedded-c/37814174)
