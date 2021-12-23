---
title:  "C++ OOPs: Inheritance"
date:   2018-03-24 12:33:22 +0530
permalink: /_posts/c-c++/oops/inheritance
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

## Inheritance

  <object data="/assets/docs/c-cpp/oops/inheritance/C++ Inheritance.pdf" type="application/pdf" width="1000px" height="1000px">
    <embed src="/assets/docs/c-cpp/oops/inheritance/C++ Inheritance.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/c-cpp/oops/inheritance/C++ Inheritance.pdf">Download PDF</a>.</p>
    </embed>
</object>

## Class Keyword vs Struct Keyword w.r.t. Inheritance
- If No access level(Default) is specified for the inheritance, the compiler assumes "private" for classes declared with keyword "class" and "public" for those declared with "struct".

## Public Vs Private Vs Protected Inheritance:
- Actually, most use cases of inheritance in C++ should use public inheritance.
- When other access levels are needed for base classes, they can usually be better represented as member variables instead.

## What is Inherited from Base Class?
- In principle, a publicly derived class inherits access to every member of a base class __*EXCEPT*__:
  - Base class constructors and Base class destructor
  - Base class assignment operator members (operator=)
  - Base class friends
  - Base class private members
- Even though access to the constructors and destructor of the base class is not inherited as such, they are automatically called by the constructors and destructor of the derived class.
- Unless otherwise specified, the constructors of a derived class calls the default constructor of its base classes (i.e., the constructor taking no arguments).

  ```
  //will call default constructor of base class `Person` automatically, assume Student is derived from Person class
  Student(string szName, int iYear, string szUniversity)
  {
    m_szUniversity = szUniversity;
  }
	```

- Calling a different constructor of a base class is possible, using the same syntax used to initialize member variables in the constructor-initialization list.

  ```
  If you want to call the parametrized(user defined) constructor of a base class from a derived class
  then you need to write a parametrized constructor of a derived class as below
  
    Student(string szName, int iYear, string szUniversity) :Person(szName, iYear)
    {
	    m_szUniversity = szUniversity;
    }
  ```

## Public Inheritance:
- Private, Protected, Public members accessibility with respect to Public Inheritance.

  <script src="https://gist.github.com/akhileshmoghe/4d9d14a007eb2fbe0c6d27d150401baa.js"></script>

## Protected Inheritance:
- Private, Protected, Public members accessibility with respect to Protected Inheritance.

  <script src="https://gist.github.com/akhileshmoghe/8e53b1f01b11f2403ee95f4dcc838d11.js"></script>

## Diamond Problem solved with Virtual Inheritance

  <script src="https://gist.github.com/akhileshmoghe/ed25651e17fd3a3b41ef11bfde78d046.js"></script>

## Virtual Inheritance Delegation to Sister Class

  <script src="https://gist.github.com/akhileshmoghe/b77d1293bc63b9fa5321fe24d1122e17.js"></script>

###### Reference
  - 


