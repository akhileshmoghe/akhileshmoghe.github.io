---
title:  "C++ STL: Container Adapters and Use cases"
date:   2018-11-5 12:33:22 +0530
categories:
  - C++
  - Data Structures
  - STL
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - Data Structures
  - STL
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will explain different __*<u>Container Adapters</u>*__, like __*<u>Stack</u>*__, __*<u>Queue</u>*__, __*<u>Priority Queue</u>*__ their respective features and mainly their appropriate use-cases.
  - Container Adapters are NOT full container classes.
  - But the classes that provide a *<u>specific interface</u>* relying on an object of one of the Container class (such as deque/list) to handle the elements.
  - The underlying container is encapsulated in a way that its elements are accessed by the members of the container adapter independent of the underlying container class.

## Stack
  - Operate in a __*<u>LIFO</u>*__ context (*<u>last-in first-out</u>*)
  - Elements are __Inserted/Extracted__ *<u>Only from</u>* __*<u>ONE END</u>*__ of the container.
  - Elements are __pushed/popped__ from the *<u>back</u>* of the specific container, which is known as the *<u>TOP</u>* of the stack.
  - By *<u>default</u>*, standard container __std::deque__ is used, if no container class is specified for a particular stack class instantiation.
  - Otherwise, __std::vector__, __std::list__ also used as underlying container.
  - <u>Use-cases</u>:
    - First-In Last-Out operations
    - Reversal of elements
  - Example:
  ```
    std::stack<int> first;                                  // empty stack
    ---------------------
    
    std::deque<int> mydeque (3,100);                        // deque with 3 elements
    std::stack<int> second (mydeque);                       // stack initialized to copy of deque
    ---------------------
    
    std::stack<int,std::vector<int> > third;                // empty stack using vector
    ---------------------
    
    std::vector<int> myvector (2,200);                      // vector with 2 elements
    std::stack<int,std::vector<int> > fourth (myvector);
  ```

###	Operations:
  ```
    * empty()
    * size()
    * top()
    * pop()
    * push()
    ------------
    
    C++11:
    * emplace()
    * swap()
  ```

### Time Complexity Stack Operations

  | Operation | Time Complexity |
  |-----------|:---------------:|
  | Push      |   __*O(1)*__    |
  | Pop       |   __*O(1)*__    |
  | Top       |   __*O(1)*__    |

### Stack Decision Criteria
  - :heavy_check_mark: __Order__ is Important.
  - :heavy_check_mark: *<u>Last-in, First-out</u>* (__LIFO__) accepted/required.
  - :x: Size varies widely - __Dynamic__
  - :x: Need to find Nth element - __Random Access__
  - :x: __Insert/Erase__ at the *<u>Front/Back</u>*
  - :x: __Insert/Erase__ at *<u>Mid-positions</u>*
  - :x: __Key:Value__ pair

##### References:
  - [std::stack](https://www.cplusplus.com/reference/stack/stack/)


## Queue
  - Operate in __*<u>FIFO</u>*__ (*<u>First-in, First-out</u>*)
  - Elements are __Inserted__ *<u>from one end</u>* of the container and __Extracted__ *<u>from the other end</u>*.
  - Elements are __Pushed__ from the *<u>back</u>* of the specific container and __Popped__ from its *<u>front</u>*.
  - By *<u>default</u>*, standard container '__*std::deque*__' is used, if no container class is specified for a particular queue class instantiation.
  - Otherwise, __*std::vector*__, __*std::list*__ also used as underlying container.
  - <u>Use-cases</u>:
    - First-In First-Out operations
    - Simple online ordering system (first come first served)
    - Semaphore queue handling
    - CPU scheduling (FCFS)
  - Example:
  ```
    std::queue<int> first;                                // empty queue
    ------------
    
    std::deque<int> mydeck (3,100);                       // deque with 3 elements
    std::queue<int> second (mydeck);                      // queue initialized to copy of deque
    ------------
    
    std::queue<int,std::list<int> > third;                // empty queue with list as underlying container
    ------------
    
    std::list<int> mylist (2,200);                        // list with 2 elements
    std::queue<int,std::list<int> > fourth (mylist);
  ```

### Operations:
  ```
    * empty()
    * size()
    * front()
    * back()
    * pop()
    * push()
    --------------
    
    C++11:
    * emplace()
    * swap()
  ```

### Time Complexity Queue Operations

  | Operation | Time Complexity |
  |-----------|:---------------:|
  | Push      |   __*O(1)*__    |
  | Pop       |   __*O(1)*__    |
  | Top       |   __*O(1)*__    |

### Queue Decision Criteria
  - :heavy_check_mark: __Order__ is Important.
  - :heavy_check_mark: *<u>First-in, First-out</u>* (__FIFO__) accepted/required.
  - :x: Size varies widely - __Dynamic__
  - :x: Need to find Nth element - __Random Access__
  - :x: __Insert/Erase__ at the *<u>Front/Back</u>*
  - :x: __Insert/Erase__ at *<u>Mid-positions</u>*
  - :x: __Key:Value__ pair


## Priority_Queue
  - Its First element is always the "__*<u>Greatest of the elements</u>*__" it contains, according to some "*<u>strict weak ordering</u>*" criterion.
    - Similar to a *<u>heap</u>*, where elements can be inserted at any moment, and *<u>only the</u>* __*<u>max heap</u>*__ *<u>element</u>* can be retrieved (the one at the top in the priority queue).
  - Elements are __Popped__ from the *<u>back</u>* of the specific container, which is known as the *<u>top</u>* of the priority queue.
  - The underlying container shall be accessible through __Random Access__ Iterators
    - Support of Random Access Iterators is required to keep a "*<u>Heap structure</u>*" internally at all times.
    - This is done automatically by the container adapter by automatically calling the algorithm functions:
      - make_heap,
      - push_heap and
      - pop_heap
  - By *<u>default</u>*, standard container '__*std::vector*__' is used, if no container class is specified for a particular priority_queue class instantiation.
  - Otherwise, '__std::deque__' can also be used as underlying container.

### Operation:
  ```
    * empty()
    * size()
    * top()
    * push()
    * pop()
    ------------
    
    C++11:
    * emplace()
    * swap()
  ```

### Priority Queue Decision Criteria
  - :heavy_check_mark: __Order__ is Important.
  - :heavy_check_mark: *<u>First-in, First-out</u>* (__FIFO__) accepted/required.
  - :x: Size varies widely - __Dynamic__
  - :x: Need to find Nth element - __Random Access__
  - :x: __Insert/Erase__ at the *<u>Front/Back</u>*
  - :x: __Insert/Erase__ at *<u>Mid-positions</u>*
  - :x: __Key:Value__ pair

##### Reference
  - [std::stack](https://www.cplusplus.com/reference/stack/stack/)
  - [std::queue](https://www.cplusplus.com/reference/queue/queue/)
  - [std::priority_queue](https://www.cplusplus.com/reference/queue/priority_queue/)


