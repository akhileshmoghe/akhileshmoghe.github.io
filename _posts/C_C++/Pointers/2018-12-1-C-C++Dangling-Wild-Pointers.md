---
title:  "C, C++ Pointers: Dangling & Wild Pointers"
date:   2018-11-1 12:33:22 +0530
permalink: /_posts/c-c++/pointers/dangling_wild_pointers
categories:
  - C
  - C++
  - Embedded
  - Pointers
  - Secure Coding
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C
  - C++
  - Embedded
  - Pointers
  - Secure Coding
author: Akhilesh Moghe
show_author_profile: true
---

## Dangling Pointers
- Dangling pointer and Wild pointers are pointers that do not point to a valid object of the appropriate type.
- Dangling references and Wild references are references that do not resolve to a valid destination.

## Wild Pointers
- Wild pointers arise when a pointer is *<u>used prior to initialization</u>*.

## Issues
- Unpredictable Behaviour - if the pointed memory location is reallocated and valid
- Segmentation Fault - if the pointed memory location is invalid or unallocated

## Causes
__1)__ &nbsp;&nbsp; In C/C++, *<u>deleting an object</u>* from memory explicitly or by *<u>destroying the stack frame on return</u>* does not alter associated pointers. The *<u>pointer still points to the same location in memory</u>* even though the reference has since been deleted and may now be used for other purposes.

  ```
  {
		   char *dp = NULL;
		   /* ... */
		   {
			   char c;
			   dp = &c;
		   } 
			 /* c falls out of scope */
			 /* dp is now a dangling pointer */
	}
  ```
- Solution:
  - If the operating system is able to detect run-time references to *<u>NULL pointers</u>*, a solution to the above is to *<u>assign 0 (NULL) to dp immediately before the inner block is exited</u>*.
  - Another solution would be to somehow guarantee dp is not used again without further initialization.

  ---

__2)__ &nbsp;&nbsp; Another frequent source of dangling pointers is a jumbled combination of *<u>malloc()</u>* and *<u>free()</u>* library calls: *<u>a pointer becomes dangling when the block of memory it points to is freed</u>*.

  ```
    #include <stdlib.h>
    void func()
    {
      char *dp = (char *)malloc(A_CONST);
      /* ... */
      free(dp);         /* dp now becomes a dangling pointer */
      dp = NULL;        /* dp is no longer dangling */
      /* ... */
    }
  ```
- Solution:
  - one way to avoid this is to make sure to reset the pointer to null after freeing its reference—as demonstrated above.

  ---

__3)__ &nbsp;&nbsp; An all too common misstep is __*<u>returning addresses of a stack-allocated local variable</u>*__:
    once a called function returns, the space for these variables gets deallocated and technically they have "garbage values".
    
    ```
    int *func(void)
    {
      int num = 1234;
      /* ... */
      return &num;
    }
    ```

- Attempts to read from the pointer may still return the correct value (1234) for a while after calling func, but any functions called thereafter will overwrite the stack storage allocated for num with other values and the pointer would no longer work correctly.
- Solution:
  - If a pointer to num must be returned, num must have *scope beyond the function*—it might be declared as *<u>static</u>*.

  ---

__4)__ Wild pointers are created by *<u>omitting necessary initialization prior to first use</u>*. Thus, strictly speaking, every pointer in programming languages which do not enforce initialization begins as a wild pointer. This most often occurs due to jumping over the initialization, not by omitting it.	Most compilers are able to warn about this.

  ```
  int f(int i)
  {
    char *dp;            /* dp is a wild pointer */
    static char *scp;    /* scp is not a wild pointer:

  }
  ```
- static variables are initialized to 0 at start and retain their values from the last call afterwards.
- Using this feature may be considered bad style if not commented.


## Security holes involving dangling pointers
- Like buffer-overflow bugs, dangling/wild pointer bugs frequently become security holes.
  - If the __pointer is used to make a virtual function call__, a *<u>different address (possibly pointing at exploit code) may be called</u>* due to	the vtable pointer being overwritten.
  - If the __pointer is used for writing to memory__, some other data structure may be corrupted.
    - Even if the memory is only read once the pointer becomes dangling, it can lead to *<u>Information Leaks</u>*	(if interesting data is put in the next structure allocated there) or to *<u>Privilege Escalation</u>* (if the now-invalid memory is used in security checks).
  - When a dangling pointer is __used after it has been freed without allocating a new chunk of memory to it__, this becomes known as a "*<u>use after free</u>*" vulnerability.


## Avoiding dangling pointer errors
- The simplest technique is to reset the pointer to __NULL__, immediately after memory is released.
- Among more structured solutions, a popular technique to avoid dangling pointers in C++ is to __*use smart pointers*__. A smart pointer typically *<u>uses reference counting</u>* to reclaim objects.
- Note:
  - In languages like Java, dangling pointers cannot occur because there is no mechanism to explicitly deallocate memory. Rather, the garbage collector may deallocate memory, but only when the object is no longer reachable from any references.


## Dangling pointer detection
- To expose dangling pointer errors, one common programming technique is to set pointers to the null pointer or to an invalid address once the storage they point to has been released. When the null pointer is dereferenced (in most languages) the program will immediately terminate—there is no potential for data corruption or unpredictable behavior. This makes the underlying programming mistake easier	to find and resolve. (This technique does not help when there are multiple copies of the pointer.)


###### Reference
  - 


