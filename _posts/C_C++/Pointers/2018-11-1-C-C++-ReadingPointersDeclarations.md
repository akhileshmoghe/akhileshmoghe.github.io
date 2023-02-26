---
title:  "C, C++ Pointers: Reading & declaring the Pointer declarations the right way"
date:   2018-11-1 12:33:22 +0530
permalink: /_posts/c-c++/pointers/declaring_pointers_the_right_way
categories:
  - C
  - C++
  - Embedded Systems
  - Pointers
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C
  - C++
  - Embedded Systems
  - Pointers
author: Akhilesh Moghe
show_author_profile: true
---

The Pointers are always tricky to declare and to read back once declared, a great cause of confusions and errors in understanding the code. So it's important to declare and read back the Pointers, exactly the right way.

  ---

  - Although C and C++ read mostly from *<u>top-to-bottom</u>* and *<u>left-to-right</u>*, __*pointer declarations read, in a sense, backwards*__.
  
  ```
  static unsigned long int *x[N];        ==>    'array of N elements of pointer to unsigned long int'
  
  declaration specifiers    ==> static unsigned long int
  declarators               ==> *x[N]
  ```

  ---

  __*<u>C, C++ Compiler Rule</u>*__:
  - *<u>The operators in a declarator group accord-ing to the same precedence as they do when they appear in an expression</u>*.
    - you’ll see that `[]` has *<u>higher precedence</u>* than `*`.
    - the function call operator, `()` have the *<u>same precedence</u>* as `[]`.
    - As grouping, `()` have the *<u>highest precedence</u>* of all.

  ---

  - Most of us place storage class specifiers such as static as the first (leftmost) declaration specifier, but it’s just a common convention, not a language requirement.

  ```
  *f(int)                       ==>    f is a function returning a pointer
  
  (*f)(int)                     ==>     f is a pointer to a function
  ```
  
  - `extern` & `static` are the __*storage specifiers*__.
  - `inline` & `virtual` are the __*function specifiers*__.
  - `const` & `volatile` are the __*type specifiers*__.

  ---

  __*<u>C, C++ Compiler Rule</u>*__:
  - *<u>The order in which the declaration speci-fiers appear in a declaration doesn’t matter</u>*.

  ```
  typedef void *VP;
  
  const VP vectorTable[]            ==    VP const vectorTable[]
  
  const void *vectorTable[]         ==    void const *vectorTable[]
  ```

  ---

  __*<u>C, C++ Compiler Rule</u>*__:
  - *<u>The only declaration specifiers that can also appear in declarators are</u>* `const` and `volatile`.

  ```
  void *const vectorTable[]         ==>    Allowed
  
  *const void vectorTable[]        ==>    Error
  ```

  ---

  - Notice the difference:
  
  ```
  T const *p;                       ==>    pointer to const T
  
  T *const p;                       ==>    const pointer to T
  ```

  ---

  - Example of Cause of confusions:

  ```
  typedef void *VP;

  const VP vectorTable[]            ==>    const void *vectorTable[]    ==>    WRONG interpretation
  
  const VP vectorTable[]            ==>    void *const vectorTable[]    ==>    RIGHT interpretation
  ```

  - Correct way of declaration to avoid confusions:
  
  ```
  typedef void *VP;
  
  VP const vectorTable[]            ==>    void *const vectorTable[]    ==>    RIGHT way of decalration and interpretation
  ```

  ---

  - Another Better way of declaration, to avoid confusions
  - *<u>Recognizing the boundary between the last declaration specifier and the declarator is one of the keys to under-standing declarations</u>*.
  
  ```
  const int* p;                    ==>     const int *p;
  ```

  ---

  __Read full original Blog__:
  <object data="/assets/docs/c-cpp/pointers/PointerDeclarations&Confusions.pdf" type="application/pdf" width="1000px" height="1000px">
    <embed src="/assets/docs/c-cpp/pointers/PointerDeclarations&Confusions.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/c-cpp/pointers/PointerDeclarations&Confusions.pdf">Download PDF</a>.</p>
    </embed>
</object>

###### Reference
  - 


