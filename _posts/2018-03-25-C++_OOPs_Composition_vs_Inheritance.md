---
title:  "C++ OOPs: Why prefer Composition over Inheritance?"
date:   2018-03-25 12:33:22 +0530
categories:
  - C++
  - OOPs
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - OOPs
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will explain OOPs Concepts like Inheritance, Composition. Specially Why prefer Composition over Inheritance whenever possible.


## Inheritance
  - Inheritance means, new Classes can be built on top of the old ones.
  - New Class called Derived Class, can inherit Data and Functions of the Original/Base Class.
    • New Class can add its own Data Elements and Functions in addition to those inherited from the Base Class.
    • Inheritance implies is-a or is-like-a relationship.
    • Avoid Deep Inheritance Trees.
    • As far as possible, it is preferable to Inherit from an Abstract Class.
    • Prefer Composition/Interfaces over Inheritance.

## Composition
    • Composition is the process of making one class a data member of another class.
    • Composition implies has-a or uses-a relationship.
    • Advantages:
        ◦ "Black Box" reuse, as internal details of contained objects are not visible i.e., Good Encapsulation.
        ◦ Fewer implementation dependencies.
        ◦ Each class is focused on just particular task.
    • Disadvantages:
        ◦ Resulting system tend to have more objects.
        ◦ Interface must be carefully defined in order to use many different objects as composition.
    • Example:
        ◦ The data members of Band could consist of objects from the Guitarist, Drummer, and Vocalist classes.
        ◦ These objects are data members of the Band class, but not descendants of parent classes.
        ◦ They are related by composition, not by inheritance.

## Why prefer Composition over Inheritance?
    • Advantage of Inheritance is:
        ◦ Code-Reuse,
        ◦ Avoiding duplicate code in derived classes
        ◦ Overriding base class implementation, against more derived class specific implementations.
        ◦ Declaring interfaces, like abstract classes
    • Main disadvantage of using inheritance is that the two classes (base and inherited class) get tightly coupled. This means one cannot be used independent of each other.
    • Also with time, during maintenance adding new features both base as well as derived classes are required to be changed.
        ◦ If a method is deleted in the "super class", then we will have to re-factor in case of using that method.
        ◦ Here things can get a bit complicated in case of inheritance because our programs will still compile, but the methods of the subclass will no longer be overriding superclass methods. These methods will become independent methods in their own right.
        ◦ It may happen that we have not implemented that deleted method in derived class and now derived class object calls this deleted base class method, getting build errors.
    • Inherited code becomes more complex to read, as multiple class implementations have to be reviewed, to make complete understanding given derived class.
    • With Inheritance, we break the rule of Encapsulation, we include all methods and attributes from parent class and expose to the child class, the child object can access all the methods in parent object and overwrite them.
    • With Inheritance, all methods of base class are inherited by derived classes, even those that are not required to be in derived classes.

##### Reference
  - [std::forward_list](https://www.cplusplus.com/reference/forward_list/forward_list/)


