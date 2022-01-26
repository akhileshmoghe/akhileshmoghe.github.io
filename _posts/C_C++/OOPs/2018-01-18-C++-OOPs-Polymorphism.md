---
title:  "C++ OOPs: Polymorphism"
date:   2018-01-21 12:33:22 +0530
permalink: /_posts/c-c++/oops/polymorphism
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

- Polymorphism:	meaning *<u>having Many Forms</u>*
- Polymorphism is about an *<u>object's ability to provide different meaning or usage to Methods or Operators in different Contexts</u>* when they are called on Object.

## Function/Operator Overloading:
- Polymorphism represents possibility to have *<u>multiple implementations of the same functions</u>*.
- Simple example of Polymorphism in C++ is __*Overloading*__.
- A *<u>function with the Same Name</u>* can have *<u>different behavior according to the context of its call</u>* i.e. with *<u>different function signatures</u>* i.e. with *<u>different number / type / sequence of parameters</u>* or *<u>different return types</u>*.

## Function Overriding:
- When *<u>Same Function/Operator is defined in different classes</u>*, which Function/Operator gets called at Run-time will depend on which class's object is actually being referenced.
	
	
## Polymorphism & Inheritance:
- The most interesting concepts of Polymorphism are related to Inheritance.
- :heavy_check_mark: A pointer of Base class can be used as pointer of Derived class, i.e. *<u>Base class pointer can contain the pointer of it's Derived Class</u>*. It's valid.
  - We can use pointer of a Base class as a pointer of the Derived class:
  - Such assignment of *<u>Derived class object to Base class pointer</u>* has following <u>Problems</u>:
  	- When we try to *<u>call a Member Function of Derived class which is not available with Base class</u>*.
      This is because Base class is not aware of Derived class all functions. This results in *<u>compilation error</u>*.
    - When we try to call *<u>a Member Function</u>* of Base class, it can be __*<u>Overridden</u>*__*<u> in Derived class</u>*.
      - Base class pointer will only call Base class member function, even if it is overridden in derived class, if polymorphism is not used by declaring member function in base class to be `virtual`.
- :x: But *<u>a pointer of Derived class cannot contain object of Base class</u>*. You will get an error like: `Cannot convert from base to derived class`

### NoPolymorphism_NoVirtualFunction.cpp
{% highlight c++ linenos %}
#include <iostream>
using namespace std;

class baseClass
{
public:
  baseClass(int val) :someValue(val) {	}

  void info()	{
    cout << "Info member function of base class" << endl;
  }
protected:
  int someValue;
};

class derivedClass1 : public baseClass
{
public:
  derivedClass1(int val) :baseClass(val) {	}

  void info() {
    cout << "Info member function of derived class 1" << endl;
  }
};

class derivedClass2 : public baseClass
{
public:
  derivedClass2(int val) :baseClass(val) {	}

  void info()	{
    cout << "Info member function of derived class 2" << endl;
  }
};

int main()
{
  derivedClass1	child1(1);
  derivedClass2	child2(2);
  baseClass   parent(10);

  //pointers to base class
  baseClass*    basePtr1;
  baseClass*    basePtr2;
  baseClass*    basePtr3;

  //make pointers to base class point to objects of derived classes
  basePtr1 = &child1;
  basePtr2 = &child2;
  basePtr3 = &parent;
	
//  derivedClass1*  derivedPtr1;
//  derivedPtr1 = &parent;        // error C2440: '=' : cannot convert from 'baseClass *' to 'derivedClass1 *'
                                  // Cast from base to derived requires dynamic_cast or static_cast

  //calling info function
  // No Polymorphism happens here, both base class pointers call base class info() implementation.
  // Reason is there is no virtual function.
  basePtr1->info();
  basePtr2->info();

  basePtr3->info();

  getchar();
}

/*
  Output: No Polymorphism
  administrator@PTL011669:Polymorphism$ ./NoPolymorphism_NoVirtualFunction 
  Info member function of base class
  Info member function of base class
  Info member function of base class
  administrator@PTL011669:Polymorphism$ 

/*
{% endhighlight %}

### NoPolymorphism_StaticDynamicCasting.cpp
{% highlight c++ linenos %}
#include <iostream>
using namespace std;

class baseClass
{
public:
  baseClass(int val) :someValue(val) {	}
  
  void info() {
    cout << "Info member function of base class" << endl;
  }
protected:
  int someValue;
};

class derivedClass1 : public baseClass
{
public:
  derivedClass1(int val) :baseClass(val) {	}

  void info() {
    cout << "Info member function of derived class 1" << endl;
  }
};

class derivedClass2 : public baseClass
{
public:
  derivedClass2(int val) :baseClass(val) {	}

  void info() {
    cout << "Info member function of derived class 2" << endl;
  }
};

int main()
{
  derivedClass1 child1(1);
  derivedClass2 child2(2);
  baseClass   parent(10);

  //pointers to base class
  baseClass*    basePtr1;
  baseClass*    basePtr2;
  baseClass*    basePtr3;
  derivedClass1*  derivedPtr1;

  //make pointers to base class point to objects of derived classes
  basePtr1 = &child1;
  basePtr2 = &child2;
  basePtr3 = &parent;
  // or below is also exactly ok, `dynamic_cast` is just adding `Run-time checks (RTTI)` before type casting as against above direct assignment.
  basePtr1 = dynamic_cast<baseClass*> (&child1);
  basePtr2 = dynamic_cast<baseClass*> (&child2);

  //calling info function
  basePtr1->info();
  basePtr2->info();

  // if `dynamic_cast` works, then `static_cast` is definitely gonna work.
  basePtr1 = static_cast<baseClass*> (&child1);
  basePtr2 = static_cast<baseClass*> (&child2);

//	derivedPtr1 = &parent;	  // error C2440: '=' : cannot convert from 'baseClass *' to 'derivedClass1 *'
							                // Cast from base to derived requires `dynamic_cast` or `static_cast`

  //calling info function
  basePtr1->info();
  basePtr2->info();

  // use static cast and call `info()` from derived class 1
  // here we are converting `basePtr1` (which contains child1 i.e. derived class object) to again derived class pointer.
  static_cast<derivedClass1*> (basePtr1)->info();

  // here, we are casting `basePtr3` (which contains parent i.e. base class object) to derived class pointer.
  // the result proves that content can be anything, the resultant casting will be in derived class and it's function will be called.
  static_cast<derivedClass2*> (basePtr3)->info();

  getchar();
}

/*
  Output: No Polymorphism
  administrator@PTL011669:Polymorphism$ ./NoPolymorphism_StaticDynamicCasting 
  Info member function of base class
  Info member function of base class
  Info member function of base class
  Info member function of base class
  Info member function of derived class 1
  Info member function of derived class 2
  administrator@PTL011669:Polymorphism$
*/
{% endhighlight %}

  ---

## Virtual Function:
- Virtual functions are functions those are *<u>expected to be overridden in the derived class</u>*.
- By declaring *<u>Base class function</u>* as `virtual`, we can *<u>call functions of a Derived class using pointer of the Base class</u>*.
- Declaring Base class function `virtual` is important, `virtual` keyword is *not mandatory* in Derived class.
- But if Derived class function is declared virtual, it will pass on Polymorphism to it's own derived class.
- See below code:
  - As we have defined `info()` function in Base class as `virtual`, compiler first looks for `info()` member function in the appropriate Derived class.
  - If it cannot find member function in the Derived class, it will call member function of the Base class.

### Polymorphism_VirtualFunction.cpp
{% highlight c++ linenos %}
#include <iostream>
using namespace std;

class baseClass
{
public:
  baseClass(int val) :someValue(val) {	}

  virtual void info() {
    cout << "Info member function of base class" << endl;
  }
protected:
  int someValue;
};

class derivedClass1 : public baseClass
{
public:
  derivedClass1(int val) :baseClass(val) {	}

  // virtual keyword here, is important w.r.t. derived class of derivedClass1
  virtual void info() {
    cout << "Info member function of derived class 1" << endl;
  }
};

class derivedClass2 : public baseClass
{
public:
  derivedClass2(int val) :baseClass(val) {	}

  // defining virtual function has effect on it's derived classes,
  // in terms that base class pointer can call appropriate derived class function
  void info() {
    cout << "Info member function of derived class 2" << endl;
  }
};

class derivedDerivedClass1 : public derivedClass1
{
public:
  derivedDerivedClass1(int val) :derivedClass1(val) {	}

  void info() {
    cout << "Info member function of derived derived class 1" << endl;
  }
};

int main()
{
  derivedClass1 child1(1);
  derivedClass2 child2(2);
  derivedDerivedClass1  childChild(3);
  baseClass   parent(10);

  //pointers to base class
  baseClass*    basePtr1;
  baseClass*    basePtr2;
  baseClass*    basePtr3;
  baseClass*    basePtr4;
  derivedClass1*  derivedPtr1;
  derivedClass1*  derivedPtr2;

  //make pointers to base class point to objects of derived classes
  basePtr1 = &child1;
  basePtr2 = &child2;
  basePtr3 = &parent;
  basePtr4 = &childChild;
  derivedPtr1 = &child1;
  derivedPtr2 = &childChild;

//  derivedPtr1 = &parent;    // error C2440: '=' : cannot convert from 'baseClass *' to 'derivedClass1 *'
                              // Cast from base to derived requires dynamic_cast or static_cast

  //calling info function
  // Polymorphism into effect as we have virtual info() function in base class
  basePtr1->info();
  basePtr2->info();
  basePtr4->info();           // obj of derived class's derived class (grandChild)
  derivedPtr2->info();        // obj of derived class's derived class (child)

  // normal pointers containing objects of same class
  basePtr3->info();
  derivedPtr1->info();

  getchar();
}

/*
  Output: Polymorphism
  administrator@PTL011669:Polymorphism$ ./Polymorphism_VirtualFunction 
  Info member function of derived class 1
  Info member function of derived class 2
  Info member function of derived derived class 1
  Info member function of derived derived class 1
  Info member function of base class
  Info member function of derived class 1
  administrator@PTL011669:Polymorphism$ 
*/
{% endhighlight %}

## Late Binding (Run-time Polymorphism)
-	Also known as __*Dynamic*__ or __*Run-time*__ Binding.
- Late Binding happens when `virtual` keyword is used in member function declaration.
Example:
```
	printObjInfo(derivedClass *derivedPtr)
	{
		baseClass *basePtr = derivedPtr;
		basePtr->info();
	}
```
- This will cause Late Binding as:
  1. `info()` is declared as `virtual` in baseClass.
  2. `info()` is overridden in both derivedClass1 & derivedClass2.
  3. `basePtr` will call respective derivedClass implementation of `info()`, based on which derivedClass's object it receives as input in `printObjInfo()`.
  4. Compiler cannot know at compile time about which derivedClass object will be passed to `printObjInfo()`.
  5. So compiler will use Late Binding and binding will happen at run-time.

- __*Late Binding Mechanism*__:
  - C++ implementation of `virtual` Function uses Late Binding which uses __*Virtual Table(VTable)*__.
  - When a class declares a `virtual` function, most of the compilers add a hidden member variable that represents a pointer to __*Virtual Method Table(VMT/VTable)*__.
  - We can call it as `vptr`.
  - This table represents an array of pointers to `virtual` functions.
  - At compile time, there is no information about which function will be called.
  - At run-time, pointers from `VTable` will point to right `virtual` functions.
  ![Virtual Method Table](/assets/images/cpp/oops/polymorphism/Virtual_Method_Table_vptr.png){:.shadow}

## Virtual Destructor
- When you have *<u>a hierarchy of class</u>*, it is strongly recommended to have virtual destructor.
- Virtual Destructor is needed, when we try to __*<u>delete a Derived Class Object with a Base Class Pointer</u>*__, otherwise __*<u>Derived class destructor will never be called</u>*__.
  - like:
  ```
    BaseClass *basePtr = new DerivedClass;
    delete basePtr;
  ```
- See example for better understanding of need of virtual destructor:

### NoVirtualDestructorProblem.cpp
{% highlight c++ linenos %}
#include<iostream>
using namespace std;

class BaseClass
{
public:
  BaseClass() {
    cout << "BaseClass Constructor..." << endl;
  }

  // No Virtual Destructor
  ~BaseClass() {
    cout << "BaseClass Destructor..." << endl;
  }
};

class DerivedClass: public BaseClass
{
public:
  DerivedClass() {
    cout << "DerivedClass Constructor..." << endl;
  }

  ~DerivedClass() {
    cout << "DerivedClass Destructor..." << endl;
  }
};

int main()
{
  {
    BaseClass	*basePtr, baseObj;
    basePtr = &baseObj;
    getchar();
  }
  cout << "-- All Ok BaseClass Constructor & Destructor called properly --\n";
	
  {
    DerivedClass *derivedPtr, derivedObj;
    derivedPtr  = &derivedObj;
    getchar();
  }
  cout << "-- All Ok Base, Derived class Constructor & Derived, Base Destructor called properly --\n";

  {
    BaseClass *basePtr;
    DerivedClass	derivedObj;
    basePtr	= &derivedObj;
    getchar();
  }
  cout << "-- All Ok Base, Derived class constructor & Derived, Base Destructor called properly --\n";
	
/*
  {
    DerivedClass	*derivedPtr;
    BaseClass		baseObj;
    derivedPtr = &baseObj;    // error C2440: '=' : cannot convert from 'baseClass *' to 'derivedClass1 *'
                              // Cast from base to derived requires dynamic_cast or static_cast
    getchar();
  }
  cout << "-----------------------------\n";
*/

  DerivedClass	derivedObj;
  {
    BaseClass *basePtr = &derivedObj;
    getchar();
//  delete basePtr;   // Runtime Error: Debug Assertion Failed
                      // as basePtr value derivedObj is not allocated using 'new' on Heap rather it's on Stack, so cannot use 'delete'
  }
  cout << "-- All Ok, Base, Derived class Constructor called & Derived, Base Destructor will be called at last properly --\n";

  {
    BaseClass	*basePtr = new DerivedClass;
    delete basePtr;
  }
  cout << "-- Here's the problem, Base, Derived class Constructor is called properly,\n\
   but only Base class Destructor gets called & Derived class Destructor never got called --\n";

  getchar();
}

/*
  Output:
  administrator@PTL011669:2_Virtual_Destructor$ ./NoVirtualDestructorProblem 
  BaseClass Constructor...

  BaseClass Destructor...
  -- All Ok BaseClass Constructor & Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  DerivedClass Destructor...
  BaseClass Destructor...
  -- All Ok Base, Derived class Constructor & Derived, Base Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  DerivedClass Destructor...
  BaseClass Destructor...
  -- All Ok Base, Derived class constructor & Derived, Base Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  -- All Ok, Base, Derived class Constructor called & Derived, Base Destructor will be called at last properly --
  BaseClass Constructor...
  DerivedClass Constructor...
  BaseClass Destructor...
  -- Here's the problem, Base, Derived class Constructor is called properly,
     but only Base class Destructor gets called & Derived class Destructor never got called --

  DerivedClass Destructor...
  BaseClass Destructor...
  administrator@PTL011669:2_Virtual_Destructor$
*/
{% endhighlight %}

### VirtualDestructor.cpp
{% highlight c++ linenos %}
#include<iostream>
using namespace std;

class BaseClass
{
public:
  BaseClass() {
    cout << "BaseClass Constructor..." << endl;
  }

  // virtual Destructor
  virtual ~BaseClass() {
    cout << "BaseClass Destructor..." << endl;
  }
};

class DerivedClass: public BaseClass
{
public:
  DerivedClass() {
    cout << "DerivedClass Constructor..." << endl;
  }

  ~DerivedClass() {
    cout << "DerivedClass Destructor..." << endl;
  }
};

int main()
{
  {
    BaseClass *basePtr, baseObj;
    basePtr = &baseObj;
    getchar();
  }
  cout << "-- All Ok BaseClass Constructor & Destructor called properly --\n";

  {
    DerivedClass  *derivedPtr, derivedObj;
    derivedPtr  = &derivedObj;
    getchar();
  }
  cout << "-- All Ok Base, Derived class Constructor & Derived, Base Destructor called properly --\n";

  {
    BaseClass *basePtr;
    DerivedClass  derivedObj;
    basePtr	= &derivedObj;
    getchar();
  }
  cout << "-- All Ok Base, Derived class constructor & Derived, Base Destructor called properly --\n";

/*
  {
    DerivedClass  *derivedPtr;
    BaseClass baseObj;
    derivedPtr = &baseObj;  // error C2440: '=' : cannot convert from 'baseClass *' to 'derivedClass1 *'
                            // Cast from base to derived requires dynamic_cast or static_cast
    getchar();
  }
  cout << "-----------------------------\n";
*/

  DerivedClass  derivedObj;
  {
    BaseClass *basePtr = &derivedObj;
    getchar();
//  delete  basePtr;  // Runtime Error: Debug Assertion Failed
                      // as basePtr value derivedObj is not allocated using 'new' on Heap rather it's on Stack, so cacnot use 'delete'
  }
  cout << "-- All Ok, Base, Derived class Constructor called & Derived, Base Destructor will be called at last properly --\n";

  {
    BaseClass *basePtr = new DerivedClass;
    delete basePtr;
  }
  cout << "-- Here was the problem, Base, Derived class Constructor was called properly,\n\
  but only Base class Destructor was called in NoVirtualDestructorProblem example & Derived class Destructor never got called\n\
  This problem is solved here by Declaring BaseClass Destructor as 'virtual' --\n";
  getchar();
}

/*
  Output:
  administrator@PTL011669:2_Virtual_Destructor$ ./VirtualDestructor 
  BaseClass Constructor...

  BaseClass Destructor...
  -- All Ok BaseClass Constructor & Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  DerivedClass Destructor...
  BaseClass Destructor...
  -- All Ok Base, Derived class Constructor & Derived, Base Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  DerivedClass Destructor...
  BaseClass Destructor...
  -- All Ok Base, Derived class constructor & Derived, Base Destructor called properly --
  BaseClass Constructor...
  DerivedClass Constructor...

  -- All Ok, Base, Derived class Constructor called & Derived, Base Destructor will be called at last properly --
  BaseClass Constructor...
  DerivedClass Constructor...
  DerivedClass Destructor...
  BaseClass Destructor...
  -- Here was the problem, Base, Derived class Constructor was called properly,
     but only Base class Destructor was called in NoVirtualDestructorProblem example & Derived class Destructor never got called
     This problem is solved here by Declaring BaseClass Destructor as 'virtual' --

  DerivedClass Destructor...
  BaseClass Destructor...
  administrator@PTL011669:2_Virtual_Destructor$
*/
{% endhighlight %}

## Abstract Class & Pure Virtual Function
- An Abstract class is the one which has __*<u>at least one Pure Virtual Function</u>*__.
- Pure Virtual Function: `virtual void info() = 0;`
- When we use *<u>Pure Virtual function</u>*, then we *<u>must override it in Derived Class</u>*.
- We *<u>cannot create an object of Abstract Class</u>*.
- But we can *<u>use Pointer of Base class to point object of Derived Class</u>*.
- When a function is declared pure virtual, it simply means that this function cannot get called dynamically, through a virtual dispatch mechanism.
- Yet, this very same function can easily be called statically, non-virtually, directly (without virtual dispatch).
- Pure Virtual unction can be *<u>callled Statically</u>* with __*Scope Specifier*__, as:
  `baseAbstractClass::printClassName();`
- __*Recommendation*__:
  - Do not implement Pure Virtual Function in Declaration itself,
  - We can define it outside class definition, as in case of `virtual ~baseAbstractClass()`
  - *<u>Pure Virtual Function body should be outside class definition</u>* is the Standard, but VC++ allows it somehow.

  ### *Pure Virtual Destructor in Abstract Class*
  - C++ provides possibility to create Pure Virtual Destructor.
  - Pure Virtual Destructor also make class an Abstract class.
  - *<u>Difference between any Pure Virtual Function and Pure Virtual Destructor</u>* is that, __*<u>Abstract class must have to implement Pure Virtual Destructor</u>*__.
  - Normally Abstract class does not implement Pure Virtual Functions, rather it's Derived classes must implement it.
  - We have to *<u>implement each and every Pure Virtual Function of Abstract Class in it's Derived Classes</u>* to make their objects, with an __*<u>exception of Pure Virtual Destructor of Abstract Class</u>*__.

    #### Use case for Pure Virtual Destructor
    - If you create *<u>a class with default implementations for its virtual methods</u>* and *<u>want to make it abstract</u>*, *<u>without forcing anyone to override any specific method</u>*, you can __*make the destructor pure virtual*__.
    - I don't see much point in it but it's possible.
    - *<u>Pure Virtual Destructor must be implemented</u>*, otherwise we get __*linking error*__.
    - Note that since the compiler will generate an implicit destructor for derived classes, if the class's author does not do so, any derived classes will not be abstract. Therefore having the pure virtual destructor in the base class will not make any difference for the derived classes. It will only make the base class abstract.
	
  ### Pure Virtual Function with function body
  - Yes, Pure Virtual Function can have a function body in Abstract class.
  - Syntax for calling Abstract class Pure Virtual Function: `baseAbstractClass::printClassName();` (See example)
  - Usage:
    - Sometime we can have *<u>common functionality to be performed by all derived classes</u>* in a Pure Virtual Function.
    - In such case, we can *<u>define common functionality in Abstract class's Pure Virtual Function body</u>* and *<u>call this function from other derived classes</u>* using above syntax.
    - This way, we are reusing the code properly.

### AbstractClass_PureVirtualDestruct_Override.cpp
{% highlight c++ linenos %}
#include<iostream>
using namespace std;

class baseAbstractClass
{
  int someValue;
public:
  baseAbstractClass(int val):someValue(val) {	}
	
  // Override Method
  virtual void overridedFunction() {
    cout << "Override Base class Method\n";
  }

  // pure virtual function
  virtual void info() = 0;

  // pure virtual function with Function body
  // `g++ error: pure-specifier on function-definition`
  // but works fine in Visual Studio.
  // Recommendation: Do not implement Pure Virtual Function in Declaration itself,
  // We can define it outside class definition, as in case of virtual ~baseAbstractClass()
  // Pure Virtual Function body should be outside class definition is the Standard, but VC++ allows it somehow.
  virtual void printClassName() = 0
  {
    cout << "Class Name = baseAbstractClass" << endl;
  }

  // pure virtual destructor, it must be implemented otherwise we get linking error: 
  // `error LNK2019: unresolved external symbol "public: virtual __thiscall baseAbstractClass::~baseAbstractClass(void)"`
  // Implementation can be here itself just like printClassName() rather than outside declaration.
  virtual ~baseAbstractClass() = 0;
};

baseAbstractClass:: ~baseAbstractClass()
{
  cout << "Inside baseAbstractClass Destructor" << endl;
}

class derivedClass: public baseAbstractClass
{
public:
  derivedClass(int val):baseAbstractClass(val) {	}

  void derivedClassFunc()
  {
    cout << "Inside derivedClassFunc" << endl;
  }

  // `error C3668: 'derivedClass::overridedFunction' : method with override specifier 'override' did not override any base class methods`
  // The reason is `change in signature` of overrideFunction(), we have added a parameter here which is not the case in Base class
  void overridedFunction(int count) override
  {
    cout << "Overrided Derived class function\n";
  }

  // Without override identifier, we are free to change the virtual functions signature in derived class
  // this violates the very basic idea of declaring virtual method in base class. override identifier plays very good role here.
  void overridedFunction(float count)
  {
    cout << "Overrided Derived class function\n";
  }

  // Implementation of pure virtual function, a must for derivedClass
  virtual void printClassName() override
  {
    // calling Abstract Base class printClassName()
    baseAbstractClass::printClassName();
    cout << "Class Name = derivedClass" << endl;
  }

  // Implementation of pure virtual function, a must for derivedClass
  void info() override  // `error C2259: 'derivedClass' : cannot instantiate abstract class`
  {                     // This error appears, if we do not implement Pure Virtual `info()` in derivedClass.
    cout << "Inside Derived Class Info()" << endl;
  }
};

int main()
{
  baseAbstractClass *basePtr;
  derivedClass  derivedObj(20); // `error C2259: 'derivedClass' : cannot instantiate abstract class`
  basePtr = &derivedObj;        // This error appears, if we do not implement Pure Virtual `info()` in derivedClass.
  basePtr->info();
  cout << "-------------------------------------\n";
  basePtr->printClassName();
  cout << "-------------------------------------\n";

//  baseAbstractClass	baseObj(10);  // `error C2259: 'baseAbstractClass' : cannot instantiate abstract class`

  // to check Pure Virtual Destructor invocation
  baseAbstractClass *basePtr1 = new derivedClass(40);
  basePtr1->info();
  delete basePtr1;

  getchar();
}
{% endhighlight %}

## override Identifier
- It shows the reader of the code that *<u>this is a virtual method, that is overriding a virtual method of the base class</u>*.
- The compiler also knows that it's an `override`, so it can "check" that you are not altering/adding new methods that you think are overrides.
- In below eaxample, in `derived2` the compiler will issue an `error for "changing the type"`.
- Without `override`, at most some compiler would give a `warning for "you are hiding virtual method by same name"`.

{% highlight c++ linenos %}
class base {
  public:
  virtual int foo(float x) = 0; 
};
class derived: public base {
   public:
   int foo(float x) override { ... do stuff with x and such ... }
}
class derived2: public base {
   public:
   int foo(int x) override { ... } 
};
{% endhighlight %}


###### References:
- https://www.tutorialcup.com/cplusplus/polymorphism.htm

	
	
	
