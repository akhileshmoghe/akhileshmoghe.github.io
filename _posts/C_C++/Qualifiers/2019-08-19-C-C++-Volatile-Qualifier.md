---
title:  "C, C++ Qualifiers: Volatile"
date:   2019-08-19 12:33:22 +0530
permalink: /_posts/c-c++/qualifiers/volatile
categories:
  - C
  - C++
  - Embedded
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C
  - C++
  - Embedded
author: Akhilesh Moghe
show_author_profile: true
---

## Volatile Qualifier
- Volatile qualifier tells the compiler *<u>not to optimize anything that has to do with the volatile variable</u>*.
- This way the compiler is forced to do what you wrote, __*NO Optimizations*__ as following:
  - It can't *<u>remove the memory assignments</u>*,
  - It can't *<u>cache variables in registers</u>*,
  - It can't *<u>change the order of assignments</u>* either.
	
- It tells the compiler that the *<u>value of the variable may change</u>* at any time-without any action being taken by the code the compiler finds nearby. Variable might be updated *<u>outside code, by any other Function, Signal Handler, Interrupt</u>*.

- The main reason to use volatile keyword is when *<u>interfacing with hardware in Embedded C Programming</u>*.

## Syntax:
  - Volatile variables:
  ```c++
     "volatile int" are Declaration Specifiers, the sequence does not matter for them.
     
       volatile int foo;
     or
       int volatile foo;
  ```

  - __*<u>Pointer to a Volatile variable</u>*__, Pointer itself is *<u>not</u>* Volatile:
  ```c++
      volatile int *foo;
    or	
      int volatile *foo;
  ```

  - __*<u>Volatile pointer</u>*__ *<u>to a non-volatile variable</u>* are very *<u>Rare</u>* to use:
  ```c++
      int *volatile foo;
  ```

  - *<u>Volatile pointer</u>* to a *<u>volatile variable</u>*:
  ```c++
    int volatile *volatile foo;
  ```

- If you apply volatile to a struct or union, the entire contents of the struct/union are volatile.

## Use-cases for Volatile Qualifier
- A variable should be declared volatile whenever its value could change unexpectedly. In practice, only three types of variables could change:

### *<u>Memory-mapped Peripheral Registers</u>*
- Embedded systems with sophisticated Peripherals contain Registers whose values may change Asynchronously to the program flow.
- As an example, consider an 8-bit status register at address 0x1234.
- It is required that you poll the status register until it becomes non-zero. The nave and incorrect implementation is as follows:
  ```c++
    {
        UINT1 *ptr = (UINT1 *) 0x1234;

        // Wait for register to become non-zero.
        while (*ptr == 0);
        // Do something else.
    }
  ```
- This will almost certainly fail as soon as you turn the optimizer on.
- The rationale of the optimizer is quite simple: having already read the variable ptr's value into the accumulator, there is no need to reread it, since the value will always be the same. Thus, we end up with an infinite while loop.
- To force the compiler to do what we want, we modify the declaration to:
  ```c++
    {
	    UINT1 volatile *ptr = (UINT1 volatile *) 0x1234;
	    
	    // Wait for register to become non-zero.
	    while (*ptr == 0);
	    // Do something else.
    }
  ```
- Subtler problems tend to arise with registers that have special properties.
- For instance, a lot of peripherals contain registers that are cleared simply by reading them.
- Extra (or fewer) reads than you are intending can cause quite unexpected results in these cases.

### *<u>Global variables</u>* modified by an *<u>Interrupt Service Routine(ISR) or Signal Handler</u>*
- Interrupt Service Routines often set variables that are tested/declared/initialized in main line code.
- For example, a Serial Port Interrupt may test each received character to see if it is an ETX character (presumably signifying the end of a message). If the character is an ETX, the ISR might set a global flag.
- An incorrect implementation of this might be:
  ```c++
    {
	    int etx_rcvd = FALSE;

	    void main()
	    {
		    ...
		    while (!ext_rcvd)
		    {
			    // Wait
		    }
		    ...
	    }

	    interrupt void rx_isr(void)
	    {
		    ...
		    if (ETX == rx_char)
		    {
			    etx_rcvd = TRUE;
		    }
		    ...
	    }
    }
  ```
- With optimization turned off, this code might work.	However, any half decent optimizer will "break" the code.
- The problem is that the compiler has no idea that etx_rcvd can be changed within an ISR. As far as the compiler is concerned, the expression !ext_rcvd is always true, and, therefore, you can never exit the while loop. Consequently, all the code after the while loop may simply be removed by the optimizer.
- If you are lucky, your compiler will warn you about this.
- If you are unlucky (or you haven't yet learned to take compiler warnings seriously), your code will fail miserably. Naturally, the blame will be placed on a "lousy optimizer."
- *<u>The solution is to declare the variable etx_rcvd to be volatile</u>*.

### *<u>Global variables</u>* within a *<u>Multi-Threaded Application</u>*
- Despite the presence of Queues, Pipes, and other scheduler-aware communications mechanisms in real-time operating systems, it is still fairly common for two tasks to exchange information via a *<u>Shared Memory location</u>* (that is, a global variable).
- When you add a pre-emptive scheduler to your code, your compiler still has no idea what a context switch is, when one might occur. Thus, another task modifying a Shared Global is conceptually identical to the problem of Interrupt Service Routines.
- So *<u>all Shared Global variables should be declared volatile</u>*.

- *<u>Volatile does not guarantee</u>* __*atomic reads or writes*__.
  - The atomicity is at the whim of the compiler. There's nothing in the C or C++ standards that says it has to be atomic.
	
## const volatile
- Que: when I can use const and volatile key word together???
- Ans: `int const volatile x =0;` is perfectly correct, no compiler error.
  - Let's assume that the value of variable x might be changed by some external force outside of this particular C source file (e.g., another thread might alter its contents).
  - Let's further assume that you want to be able to see this change in x, but you don't want to alter the value yourself. In other words, someone else might asynchronously alter the value, and you only want to see the changes (i.e., you just want *<u>read-only access to this variable</u>*).
  - By using `const`, you are telling the compiler that *<u>your code will not be changing the value of this variable</u>*. If you attempt to change it directly, the compiler should issue a warning.
  - By using `volatile`, you are telling the compiler that *<u>someone else (e.g., another thread) might change the value at any time</u>*, so the compiler will not just optimize away repeated reads of the variable.
  - By using `const` and `volatile` together, you are saying that *<u>you won't be changing the value (const), but someone else might (volatile)</u>*.
  - In close to hardware programming a typical use of `const volatile` would be to describe a *<u>read-only hardware register</u>*. You can only read it, but the hardware can change it at any time.
  - e.g. `UINT volatile *const ptr = (UINT volatile *) 0x1234;`
    - `ptr` is `const`, register 0x1234 is `volatile`.


###### References
  - [Introduction to the volatile keyword](http://www.embedded.com/electronics-blogs/beginner-s-corner/4023801/Introduction-to-the-Volatile-Keyword)
  - [Why is volatile needed in C?](http://stackoverflow.com/questions/246127/why-is-volatile-needed-in-c)


