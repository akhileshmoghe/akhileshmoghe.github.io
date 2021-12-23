---
title:  "C++ STL: Sequence Containers and Use cases"
date:   2018-11-1 12:33:22 +0530
permalink: /_posts/c-c++/stl/sequence_containers
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

This write-up will explain different __*<u>Sequence containers</u>*__, their respective features and mainly their appropriate use-cases.

## Array
  - (C++11)	(Sequence)
  - Features:
    - Contiguous memory - allows *<u>Constant time</u>* __Random access__ to elements.
    - Strict __Ordered__ *<u>Linear</u>* Sequence.
    - *<u>Fixed-size</u>* Sequence container: they hold a specific number of elements ordered in a strict linear sequence.
      The container uses Implicit Constructors and Destructors to *<u>allocate</u>* the required space *<u>Statically</u>*.
    - Its size is *<u>Compile-time constant</u>.
    - No memory or time overhead.
    - This Array class merely adds a layer of member and global functions to it, so that arrays can be used as standard containers.
    - cannot be expanded or contracted dynamically.
    - Zero-sized arrays are valid, but they should not be dereferenced.
    - Swapping two array containers is a Linear and considerably less efficient operation, as it involves swapping of all members individually.
    - Another unique feature of array containers is that they can be treated as 'tuple'(C++11) objects.

### Advantages over traditional C/C++ Arrays
  - __std::array__ classes knows its own Size, whereas C-style arrays lack this property.
    - So when passing to functions, we *<u>don’t need to pass Size of Array as a separate parameter</u>*.
  - With *<u>C-style array there is more risk of array being Decayed into a pointer</u>*.
    - std::array classes don’t decay into pointers.
    - See: Array_Decay_Avoiding.cpp
  - std::array classes are generally more efficient, light-weight and reliable than C-style arrays.
  - Arrays are Fixed-Size Sequence Containers:
	  - they hold a specific number of elements Ordered in a strict Linear Sequence.
  - std::array class internally DOES NOT contain any Data other than Elements it hold.(not even its Size)
	  - std::array class merely adds a layer of member and global functions to it, so that arrays can be used as standard containers.
  - Unlike the other standard containers, Arrays have a __Fixed Size__ and they CANNOT be expanded or contracted dynamically.
  - Zero-sized arrays are valid, but they should not be dereferenced.
  - Unlike with the other containers in the Standard Library, Swapping two array containers is a Linear Operation that involves Swapping All the Elements in the ranges Individually, which generally is a considerably Less Efficient operation. On the other side, this allows the iterators to elements in both containers to keep their original container association.
	- Another unique feature of std::array containers is that they can be treated as __tuple__ objects:
    - The __std::array__ header overloads the __get<>__ function to access the elements of the array as if it was a tuple, as well as specialized __tuple_size__ and __tuple_element__ types.

##### References:
  - [std::array](https://www.cplusplus.com/reference/array/array/)
  - [What is Array Decay in C++? How can it be prevented?](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/)

## Deque
  - Double-Ended queue
  - __Sequence Container__ - Ordered in a strict Linear Sequence
  - __Dynamic Size__: Expanded/Contracted on both ends(front/back)
  - __Random Access__ Iterators - *<u>Access to individual elements</u>* - in *<u>Constant Time</u>*
  - similar to *<u>vectors</u>*, but with efficient insertion/deletion of elements not only at the end, but at front/mid-positions also.
  - __DOES NOT__ guaranteed to store all its elements in *<u>Contiguous Storage</u>* locations:
    - elements of `deque` can be scattered in different chunks of storage, container keeps track of it for Random access.\
    - :x: <u>Accessing elements in a deque</u> by *<u>offsetting a pointer</u>* to another element causes *<u>undefined behavior</u>*.
    - :x: DO NOT access elements with pointer+offset.
  - deques perform worse for operations that involve frequent insertion/deletion of elements at positions other than the beginning or the end 
    - :x: i.e. deque should NOT be used for insertion/deletion at mid-positions.

### vector vs deque:
  - Both vectors and deques provide a very similar interface and can be used for similar purposes, but internally both work in quite different ways:
    - *<u>vectors</u>* - use a <u>contiguous storage locations<u> that needs to be occasionally reallocated for growth,
    - *<u>deque</u>* - <u>Elements can be scattered in different chunks of storage</u>, with the container keeping the necessary information internally to provide direct access to any of its elements in constant time and with a uniform sequential interface (through iterators).
  - Therefore, *<u>deques</u>* are a little more complex internally than vectors, but this *<u>allows them to grow more efficiently</u>* under certain circumstances, especially with very long sequences, where reallocations become more expensive.
  - *<u>deques</u>* are efficient with <u>insertion and deletion</u> of elements also *<u>at the beginning & end</u>* of the sequence.
  - *<u>vectors</u>* are efficient with <u>insertion and deletion</u> of elements __ONLY__ *<u>at the end</u>* of the sequence.

### Deque Decision Criteria
  - :heavy_check_mark:  Size varies widely - __Dynamic__
  - :heavy_check_mark:  Need to find Nth element - __Random Access__
  - :heavy_check_mark:  __Insert/Erase__ at the *<u>Front/Back</u>*
  - :x: __Insert/Erase__ at *<u>Mid-positions</u>*
  - :x: __Key:Value__ pair
  - :x: __Order__ is Important

##### References
  - [Deque in C++ Standard Template Library (STL)](https://www.geeksforgeeks.org/deque-cpp-stl/)
  - [std::deque](https://www.cplusplus.com/reference/deque/deque/)


## Vector
  - __Sequence__
  - __Ordered__
  - __Random Access__
  - __Insert/Erase__ *<u>at the End</u>*
  - Features:
    - A vector is a __Sequence__
      - Sequence Container - Ordered in a strict Linear Sequence
    - __Random Access__ to elements - in *<u>Constant time</u>*
    - *<u>Contiguous Memory</u>*.
      - which means that their elements can also be accessed using offsets on regular pointers to its elements.
    - __Insertion/Deletion__:
      - *<u>Constant time</u>* - __AT the END__
      - *<u>Linear time</u>* - __at the beginning/mid-positions__
    - __Dynamic Size__ - An Advantage over Arrays. - Arrays that can *<u>change Size</u>*.
    - __Reallocation__:
      - vectors internally use dynamically allocated arrays
      - :warning: To modify size dynamically, arrays needs to reallocated, and all elements needs to be moved, this is a *<u>less efficient</u>*, *<u>costly operation</u>* to perform frequently.
      - so, vectors allocate some extra memory for accommodation.
      - so, *<u>actual 'capacity' of vectors is greater than 'size' of vectors</u>*.
      - Vectors consume more memory than Arrays, due to above reasons.
      - When it is necessary to *<u>increase capacity()</u>*, vector usually *<u>increases it by</u>* a __factor of two__.
      - :warning: Reallocation INVALIDATES any iterators that point into the vector.
      - *<u>reserve()</u>* causes a reallocation manually.
        - The main reason for using reserve() is <u>efficiency</u>.
        - using reserve(), you can control the INVALIDATION of iterators.
      - :warning: Inserting or Deleting an element *<u>in the middle</u>* of a vector INVALIDATES all iterators.
      - :heavy_check_mark: Insertion/Deletion *<u>at the end</u>* of the vector, prevents vector iterators from being invalidated.
        - i.e. vectors are designed to be expanded at 'END'
    - Vectors more efficient in accessing Random elements, than 'deque'/'lists'/'forward_lists'

### Time Complexity Vector Operations

  | Operation          | Time Complexity |
  |--------------------|:---------------:|
  | Insert at Head     |   __*O(n)*__    |
  | Insert at Index    |   __*O(n)*__    |
  | Insert at Tail     |   __*O(1)*__    |
  | Remove at Head     |   __*O(n)*__    |
  | Remove at Index    |   __*O(n)*__    |
  | Remove at Tail     |   __*O(1)*__    |
  | Find Index         |   __*O(1)*__    |
  | Find/Search Object |   __*O(n)*__    |


### Vector Decision Criteria
  - :heavy_check_mark: Insert/Erase at the End
  - :heavy_check_mark: when Nth element is to be found - Random Access
  - :x: when Order is important
  - :x: Key:Value pair required
  - :x: When there's need of Insert/Erase in Middle/Front, as it invalidates iterators.
  - :x: when Size will vary widely, as it causes Reallocation, which in turn invalidates iterators.

##### References
  - [std::vector](https://www.cplusplus.com/reference/vector/vector/)


## Lists
  - __Sequence__
  - __Insert/Erase at the Front/Back/Mid__
  - __NO Random Access__
  - __Iterate => Forward/Backward__
  - Features:
    - Sequence container
    - Doubly Linked List.	-- Bidirectional Iterators.
    - Iterate/Traverse in Both Forward/Backward.
    - __Insertion/Deletion__:
      - :heavy_check_mark: *<u>Constant time</u>* - __at the Beginning/End/Mid-position__ i.e. ANYWHERE.
        - i.e. We can 8<u>insert(iterator pos, const T& x)</u>* at a specific position
        - and *<u>erase(iterator pos)</u>* at the specific position.
    - :heavy_check_mark: Lists have the important property that *<u>Insertion</u>* and *<u>Splicing</u>* __DO NOT invalidate iterators__ to list elements
      - i.e. same iterators can be used, they get themselves aligned to updated list.
    - :x: *<u>Memory</u>* allocation is __NOT Contagious__.
    - __*<u>forward_list</u>*__ performs better than *<u>arrays/vectors/deque</u>* in actions as *<u>Inserting/Deleting/Extracting/Moving</u>* elements within the container.
    - :x: __*<u>forward_list</u>*__ and __*<u>list</u>*__ *<u>Do NOT</u>* have Direct/__Random Access__ to elements.
      - Forward_list & List has to Iterate element by element to desired element.
      - i.e. *<u>Linear Time</u>*
		
### Time Complexity List Operations

  | Operation          | Time Complexity |
  |--------------------|:---------------:|
  | Insert at Head     |   __*O(1)*__    |
  | Insert at Index    |   __*O(n)*__    |
  | Insert at Tail     |   __*O(1)*__    |
  | Remove at Head     |   __*O(1)*__    |
  | Remove at Index    |   __*O(n)*__    |
  | Remove at Tail     |   __*O(1)*__    |
  | Find Index         |   __*O(1)*__    |
  | Find/Search Object |   __*O(n)*__    |

### List Decision Criteria
  - :heavy_check_mark: If Insert/Erase is required in middle/front/end i.e. ANYWHERE.
  - :heavy_check_mark: Forward and Backward Traversal is required.
  - :heavy_check_mark: If required to merge collections.					--> Study more.
  - :x: Random Access is required.
  - :x: If Order is important.
  - :x: Key:Value pair required.

##### Reference
  - [std::list](https://www.cplusplus.com/reference/list/list/)

## Forward List
  - __Sequence__ - *<u>Singly Linked List</u>*
  - __Iterate__ => Forward
  - __Insert/Erase__ => Front/Back/Mid
  - Features:
    - Singly Linked List: a list where each element is linked to the next element, but not to the previous element.
      -- Forward Iterators
      - Singly Linked Lists are smaller and *<u>faster</u>* than double linked lists.
      - It is a Sequence that supports forward but not backward traversal.
    - __Insertion/Deletion__
      - *<u>Constant time</u>* - at the __Beginning__ or the __End__, or in the __Middle__.
        - i.e. We can *<u>insert/insert_after(iterator pos, const T& x)</u>* at a specific position
        - and *<u>erase/erase_after(iterator pos)</u>* at the specific position.
    - like __std::list__, __std::forward_list__ also has the important property that Insertion and Splicing __DO NOT__ *<u>Invalidate</u>* iterators to list elements.
      - i.e. same iterators can be used, they get themselves alligned to updated list.
    - This is the only container, that __DO NOT__ have *<u>size()</u>* member function, because:
      - for efficiency considerations:
      - Being a Linked List, for having a size() member that takes constant time would require it to keep an internal counter for its size (as std::list does).
      - This would consume some extra storage and make insertion and removal operations slightly less efficient.

### Important Functionality Difference between std::list & std::forward_list:
  - *<u>insert()</u>* & *<u>erase()</u>*:
    - Using these member functions carelessly, can result in disastrously slow programs.
    - The problem is that insert() inserts the new element(s) before pos.
    - This means that insert must find the iterator just before pos; this is a constant-time operation for std::list, since it is bidirectional.
    - but for std::forward_list it must find that iterator by traversing the list from the beginning up to pos.
    - In other words: *insert()* and *erase()* are *<u>slow operations anywhere but near the beginning</u>* of the std::forward_list.
				
  - *<u>insert_after()</u>* & *<u>erase_after()</u>*:
    - insert_after() and erase_after() are *<u>constant time</u>* operations:
      - you should always use insert_after() and erase_after() whenever possible.
      - If you find that insert_after() and erase_after() aren't adequate for your needs, and that you often need to use insert() and erase() in the middle of the list, then you should probably use std::list instead of std::forward_list.

  - std::forward_list is __more efficient__ than std::list
    - As std::list consumes additional storage per element
    - std::list has with a slight higher time overhead inserting and removing elements, as it has to fill both the pointers correctly.

### Forward List Decision Criteria
  - :heavy_check_mark: If Insert/Erase is required in middle/front/end i.e. ANYWHERE.
  - :heavy_check_mark: If required to merge collections.					--> Study more.
  - :x: Backward Traversal is required.
  - :x: Random Access is required.
  - :x: If Order is important.
  - :x: Key:Value pair required.

##### Reference
  - [std::forward_list](https://www.cplusplus.com/reference/forward_list/forward_list/)


