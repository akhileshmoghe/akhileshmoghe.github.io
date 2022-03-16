---
title:  "Design Patterns: Singleton"
date:   2020-03-2 5:33:22 +0530
permalink: /_posts/c-c++/design_patterns/singleton
categories:
  - C++
  - C++11
  - OOPs
  - Design Patterns
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - C++11
  - OOPs
  - Design Patterns
author: Akhilesh Moghe
show_author_profile: true
---

- __*<u>Aim</u>*__: *<u>To use single instance of a class throughout the lifetime of an application</u>*.
- Ways to achieve this:
  - __*<u>Global Object/Instance</u>*__:
    - In C++, it is possible to declare a global object, which can be used anywhere inside the program.
    - But a good object-oriented design strictly prohibits use of global variables/methods, since they are against fundamental principles of object orientation like data encapsulation or data hiding.
  - __*<u>A class with all Static Methods</u>*__:
    - Another practical solution to get a single object is by declaring a class, which contains only static methods.
    - A static class is loaded into memory when the execution of the program starts, and it remains there till the application ends.
    - Remember that for invoking a static method of a class, it is not necessary to create an instance of the class. But a class with only static methods and variables are not a good object-oriented design.
  
## Singleton Pattern:
- Used to create only one instance of a class in a truly object-oriented fashion.
- Used where only one instance of an object is needed throughout the lifetime of an application.
- The Singleton class is *<u>instantiated at the time of first access</u>* and same instance is used thereafter till the application quits.
- The Singleton Pattern comes under that classification of __*Creational Patterns*__, which deals with the best ways to create objects.
- Usually *<u>used along with other patterns</u>* e.g. [Abstract Factory](), [Builder](), [Prototype](), [Facade](), etc.
![Singleton_design](/assets/images/design_patterns/singleton/Singleton_design.jpg)

### <u>Usage</u>:
- The Singletons are often used to *<u>control access to resources</u>* such as "database connections" or "sockets". Suppose we have license for only one connection for our database. A Singleton connection object makes sure that only one connection can be made at any time.
- Your code creates *<u>multiple instances</u>* of an object, and that *<u>uses too much memory</u>* or slows system performance. Replace the multiple instances with a Singleton. Users of your system are complaining about *<u>system performance</u>*.
- Your profiler tells you that you can improve performance by not instantiating certain objects over and over again.
- The *<u>objects you want to share have no state or contain state that is sharable</u>*.

### <u>Cautions</u>:
- *<u>Misuse of Singleton</u>*:
  - Singletons are not same as Global Variables.
  - Singletons should be used to ensure that the object has only one instance and not to get that instance from everywhere.
- *<u>Benefits and Liabilities</u>*:
  - :heavy_check_mark: Improves performance
  - :x: Is easily accessible from anywhere. In many cases, this may indicate a design flaw.
  - :x: Is not useful when an object has state that can't be shared.

### <u>Object Pool</u>
- It’s a variation of Singleton Pattern.
- A set of objects exists e.g. a fixed number of database connections.
- Threads acquire and release instances.
- The Resource Pool to allocate and free connections is a singleton.

### <u>Implementing Singleton Pattern</u>:
- *<u>A private constructor</u>*: To create instance of the class
- *<u>A static method</u>*: To return same instance of the class
- *<u>A static instance variable</u>*: being static, it will persist in memory till program runs.

#### <u>C++ Reference Implementation</u>:
{% highlight c++ linenos %}
/*
  Creational Pattern: SINGLETON
  Author: Rajesh V.S
  Language: C++
  Email: rajeshvs@msn.com
*/

#include <iostream>
using namespace std;

class Singleton
{
private:
  static bool instanceFlag;   // A Static flag/bool variable, to make sure Instance variable is created only once,
                              // subsequent calls to getInstance() will just return already created instance.
  static Singleton *single;   // A Static Instance Variable
  Singleton() {
      // private constructor
  }
	
public:
  static Singleton* getInstance();    // A Static Method
  void method();
  ~Singleton()
  {
      instanceFlag = false;
  }
};

bool Singleton::instanceFlag = false;   // This is the only way of Initializing the Static Class Variable.
Singleton* Singleton::single = NULL;    // A static variable must be Initialized in this way before accessing, or it will give error:
                                        // error LNK2001: unresolved external symbol "private: static class Singleton * Singleton::single" (?single@Singleton@@0PAV1@A)
                                        // error LNK2001: unresolved external symbol "private: static bool Singleton::instanceFlag" (?instanceFlag@Singleton@@0_NA)


// In Definition of a Method, Static keyword should not be used, it gives error:
// error C2724: 'Singleton::getInstance' : 'static' should not be used on member functions defined at file scope
Singleton* Singleton::getInstance()
{
  if(! instanceFlag)
  {
    single = new Singleton();
    instanceFlag = true;
    return single;
  }
  else
  {
    return single;
  }
}

void Singleton::method()
{
  cout << "Method of the singleton class" << endl;
}

int main()
{
  Singleton *sc1,*sc2;
//  sc1 = new Singleton;    // error C2248: 'Singleton::Singleton' : cannot access private member declared in class 'Singleton'
                            // Error: Singleton::Singleton() is inaccessible
                            // Till the time we create object of a class, we cannot access any of it's members
                            // that's where we need Static Member function, it will be class function and can be called following way:

  sc1 = Singleton::getInstance();
  sc1->method();
  sc2 = Singleton::getInstance();
  sc2->method();
  std::cout << "Are both Singletons same? " << ((sc1==sc2)?"Yes":"No") << std::endl;
  return 0;
}
{% endhighlight %}

#### <u>C++11 Reference Implementation</u>
{% highlight c++ linenos %}
#include <iostream>
using namespace std;

// This code will work in c++11.
// Ensure your compiler is fully c++11 complaint for this code.

class Singleton {
  Singleton() {}    // Object cannot be created
  ~Singleton() {}   // Object cannot be deleted

public:
  void process() { cout << "Singleton used\n"; }
  static Singleton *getInstance();
};

Singleton *Singleton::getInstance() {
  static Singleton instance;    // Singleton will be created the first time function invoked.
  return &instance;             // Thread-safety of singleton creation guaranteed by C++11
}

void singleton_main() {
  Singleton *a = Singleton::getInstance();
  a->process();
  Singleton *b = Singleton::getInstance();
  std::cout << "Are both Singletons same? " << ((a==b)?"Yes":"No") << std::endl;
}
{% endhighlight %}

  ---

###### References:
- [Singleton Pattern & its implementation with C++](https://www.codeproject.com/Articles/1921/Singleton-Pattern-its-implementation-with-C)
- GoFPatterns.pdf: Page-90


