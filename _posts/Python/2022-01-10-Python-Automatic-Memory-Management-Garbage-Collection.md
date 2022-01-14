---
title:  "Python Automatic Memory Management & Garbage Collection"
permalink: /_post/python/memory_management_garbage_collection
date:   2022-01-10 1:33:22 +0530
#modify_date: 2021-12-31
categories:
  - Python
  - Memory Management
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Python
  - Memory Management
author: Akhilesh Moghe
show_author_profile: true
#sidebar:
#  nav: parallel-computing
---

## Automatic memory management
  - Python automatically allocates objects and reclaims (“garbage collects”) them when they are no longer used, and most objects can grow and shrink on demand.
  - In Python, when we lose the last reference to the object—by assigning its variable to something else, for example—all of the memory space occupied by that object’s structure is automatically cleaned up.
  - In standard Python (a.k.a. CPython), the space is reclaimed immediately, as soon as the last reference to an object is removed.

## Dynamic Typing model:
- In Python, types are determined automatically at runtime, not in response to declarations in your code.
- A variable is created when your code first assigns it a value.
- Future assignments change the value of the already created name.
- A variable never has any type information or constraints associated with it. The notion of type lives with objects, not names.
- Variables are generic in nature; they always simply refer to a particular object at a particular point in time.
- Further, all variables must be explicitly assigned before they can be used; referencing unassigned variables results in errors. for example, must be initialized to zero before you can add to them.
- Variables always link to objects and never to other variables, but larger objects may link to other objects (for instance, a list object has links to the objects it contains).
- All variable assignments are based on references (including __*<u>function argument passing</u>*__).


### References
- The links from variables to objects are called __references__ in Python
- A reference is a kind of association, implemented as a pointer in memory.
- Whenever the variables are later used (i.e., referenced), Python automatically follows the variable-to-object links.

#### Shared References
- When a variable is assigned to another variable, we have multiple names/variables referencing the same object, this is usually
called *<u>a shared reference</u>* (and sometimes just a shared object).


#### Cyclic References
- References are implemented as pointers, it’s possible for an object to reference itself, or reference another object that references the same object. This is a case of *<u>Cyclic References</u>*.
- Python’s garbage collection is based mainly upon reference counters; however, it also has a component that detects and reclaims objects with cyclic references in time.
- This component can be disabled if you’re sure that your code doesn’t create cycles, but it is enabled by default.

#### Weak References
- In short, a weak reference, implemented by the __*<u>weakref</u>*__ standard library module, *<u>is a reference to an object that does not by itself prevent the referenced object from being garbage-collected</u>*.
- If the last remaining references to an object are weak references, the object is reclaimed and the weak references to it are automatically deleted (or otherwise notified).
- This can be *<u>useful in dictionary-based caches of large objects</u>*, for example; otherwise, the cache’s reference alone would keep the object in memory indefinitely.
- For more details, see Python’s library manual.


### Objects
- Objects have more structure than just enough space to represent their values.
- Each object also has two standard header fields: *<u>a type designator</u>* used to mark the type of the object, and *<u>a reference counter</u>* used to determine when it’s OK to reclaim the object.

## Garbage Collection
- In Python, whenever a variable name is assigned to a new object, the space held by the prior object is reclaimed if it is not referenced by any other name or object. This automatic reclamation of objects’ space is known as *<u>garbage collection</u>*.
- Internally, Python accomplishes Garbage Collection by keeping a counter in every object that keeps track of the number of references currently pointing to that object. As soon as this counter drops to zero, the object’s memory space is automatically reclaimed.

## Garbage Collection Module
- An interface to the optional garbage collector.
- Ability to disable the collector, tune the collection frequency, and *<u>set debugging options</u>*.
- It also provides *<u>access to unreachable objects that the collector found but cannot free</u>*.
- Since the collector supplements the reference counting already used in Python, you can disable the collector if you are sure your program does not create reference cycles.
- Automatic collection can be disabled by calling `gc.disable()`.
- To *<u>debug a leaking program</u>* call `gc.set_debug(gc.DEBUG_LEAK)`. Notice that this includes `gc.DEBUG_SAVEALL`, causing garbage-collected objects to be saved in `gc.garbage` for inspection.
- Further Reading:
  - [gc — Garbage Collector interface](https://docs.python.org/3/library/gc.html)


###### References:
- Book: Learning Python





