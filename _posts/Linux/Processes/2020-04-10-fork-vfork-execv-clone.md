---
title:  "Linux Process Creation: fork, vfork, execv, clone"
date:   2020-04-10 12:33:22 +0530
permalink: /_posts/linux/processes/fork-vfork-execv-clone
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

## fork():
- Makes a *<u>Duplicate of the current process</u>*, Identical in almost every way.
- *<u>New process (child) gets a Different Process ID (PID)</u>* and PID of the old process (parent) as its *<u>Parent PID (PPID)</u>*.
- Two processes are now running exactly the same code.
- *<u>Child and Parent Processes use </u>*__*<u>different Virtual Address</u>*__ spaces, *<u>which is initially populated by the same memory pages</u>*.

### *<u>Copy-On-Write</u>*:
  - As both processes are executed, the Virtual Address spaces begin to differ more and more, because the operating system performs a __*lazy copying of memory pages*__ that are being written by either of these two processes and *<u>assigns an independent copies of the modified pages of memory for each process</u>*. This technique is called *<u>Copy-On-Write</u>* (COW).
- Return value of fork()
  - *<u>Child</u>* process gets `0` in return.
  - *<u>Parent</u>* gets the new `PID of the child` in return.

## vfork():
- Basic difference between `vfork()` and `fork()` is that when a new process is created with `vfork()`:
  - the __*Parent Process is Temporarily Suspended*__.
  - the *<u>Child Process might borrow the Parent's Address Space</u>*.
- *<u>Child and Parent processes share the </u>*__*<u>same Virtual Address Space</u>*__, i.e. Parent and Child use the __*<u>same</u>*__ *<u>Stack</u>*, *<u>Stack Pointer</u>*, *<u>Instruction Pointer</u>*.
- As Virtual Address Space is shared between Parent and Child, to *<u>prevent unwanted interference the Parent Process is Frozen/Suspended</u>*, until:
  - the *<u>Child Process</u>* will call either `exec()` (*<u>create a new virtual address space</u>* and transition to a different stack)
  - or `_exit()` (termination of the process execution).
- When *<u>Child Process</u>* either `exit()`, or calls `execve()`, then __*Parent Process Continues*__.
- :warning: *<u>Child Process</u>* of a `vfork()` must be careful to *<u>Avoid Unexpectedly Modifying Variables of the Parent Process</u>*.
- :warning: *<u>Child Process</u>* __*Must Not call*__ `exit()`, if it needs to exit, it should use `_exit()`;
  - because with `vfork()` child is potentially borrowing its Parent's Address Space.
  - Anything `vfork()` does, besides calling `execve()` or `_exit()`, has a great *<u>potential to mess up the parent</u>*.
  - In particular, `exit()` calls `atexit()` handlers and other "finalizers", e.g: it *<u>flushes std I/O streams</u>*.
- :warning: *<u>Child Process </u>*__*Must Not Return*__*<u> from the function containing the </u>* `vfork()` call.
  - Returning from a `vfork()` Child would potentially (same caveat as before) *<u>mess the Parent's Stack</u>*.

### use-case for vfork()
- The `vfork()` function can be used to *<u>create new processes without fully copying the address space of the old process</u>*.
- If a forked process is simply going to call `exec`, the data space copied from the parent to the child by `fork()` is not used.
- This is particularly *<u>inefficient in a paged environment</u>*, making `vfork()` particularly useful. *<u>Depending upon the size of the parent's data space</u>*, `vfork()` can give a *<u>significant performance improvement</u>* over `fork()`.

## execv():
- __*Replaces the entire Current Process with a New Program*__.
- `exec()` replaces the Current Process with a the executable pointed by the function.
- *<u>Control never returns to the original program</u>* *unless there is an *`exec()` *error*.
- This system call with `fork()` system call together form a classical UNIX process management model called __*fork-and-exec*__.

## clone(fn(arg)):
- `clone()`, as `fork()`, __*creates a new process*__.
- Unlike `fork()`, `clone()` allow the __*Child Process to share parts of its execution context with the calling process*__:
  - such as the *<u>Memory Space</u>*, the table of *<u>File Descriptors</u>*, and the table of *<u>Signal Handlers</u>*.
  - *<u>The Difference between </u>*`fork()` and `clone()`*<u> is which data structures (Memory Space, Processor State, Stack, PID, Open Files, etc) are </u>*__*shared*__ or __*not*__.
- Child Process is created with `clone()`, *<u>executes the function application </u>* `fn(arg)`.
  - This differs from `fork`, where execution continues in the child from the point of the original `fork` call.
- The `fn` argument is a *<u>Pointer to a Function</u>* that is called by the Child Process at the beginning of its execution.
- The `arg` argument is passed to the `fn` function.
- When the `fn(arg)` function application returns, the child process terminates.
- The integer returned by `fn` is the exit code for the child process.
- The child process may also terminate explicitly by calling `exit(2)` or *<u>after receiving a fatal signal</u>*.


###### Reference:
- [the-difference-between-fork-vfork-exec-and-clone](https://stackoverflow.com/questions/4856255/the-difference-between-fork-vfork-exec-and-clone)
- [fork(2) — Linux manual page](https://man7.org/linux/man-pages/man2/fork.2.html)
- [vfork(2) — Linux manual page](https://man7.org/linux/man-pages/man2/vfork.2.html)
- [vfork - usecase](https://pubs.opengroup.org/onlinepubs/009696799/functions/vfork.html#:~:text=The%20vfork()%20function%20can,fork()%20is%20not%20used.&text=The%20vfork()%20function%20can%20normally%20be%20used%20just%20like%20fork().)
- [clone(2) — Linux manual page](https://man7.org/linux/man-pages/man2/clone.2.html)


