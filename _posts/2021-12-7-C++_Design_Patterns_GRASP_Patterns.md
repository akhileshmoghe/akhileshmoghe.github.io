---
title:  "Design Patterns: Most Important GRASP Patterns of designing an application or module "
date:   2021-12-7 5:33:22 +0530
categories:
  - C++
  - Design Patterns
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - Design Patterns
#author: Akhilesh Moghe
#show_author_profile: true
---

This write-up will explain different __*<u>GRASP Patterns</u>*__, explained in book: [Applying UML and Patterns](https://www.amazon.com/gp/product/0131489062/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=fluentcpp-20&creative=9325&linkCode=as2&creativeASIN=0131489062&linkId=43cfc4d0a6ea922ef663317e4f91db85), which are more important in day to day programming life than the whole lot of GoF design patterns.
GRASP is a set of 9 fundamental principles in OOPs design and responsibility assignment.
These patterns solve some software problem common to many software development projects.

## Information Expert (Information Hiding)
  - *<u>Problem</u>* it solves: How will you assign a responsibility to a module/class?
  - *<u>Solution</u>*: Assign responsibility to the class that has the information needed to fulfill it.
  - Used to determine where to delegate responsibilities such as methods, computed fields, and so on.
  - *<u>If you have an operation to do, and this operations needs inputs, then you should consider putting the responsibility of carrying out this operation in the class that contains the inputs for it</u>*.
  - Helps keeping the data local, i.e. __*Data Encapsulation*__.
  - Reduces relationships between the classes. (Class is self-sufficient in terms of data to carryout it's tasks.)

## Creator
  - The creation of objects is one of the most common activities in an object-oriented system.
  - Which class is responsible for creating objects is a fundamental property of the relationship between objects of particular classes.
  - 

##### Reference
  - [std::forward_list](https://www.cplusplus.com/reference/forward_list/forward_list/)


