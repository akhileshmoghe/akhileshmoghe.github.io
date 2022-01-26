---
title:  "Design Patterns: Observer"
date:   2022-01-21 5:33:22 +0530
permalink: /_posts/c-c++/design_patterns/observer
categories:
  - C++
  - OOPs
  - Design Patterns
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - OOPs
  - Design Patterns
author: Akhilesh Moghe
show_author_profile: true
---

- __*Behavioural Pattern*__
- The observer pattern is a software design pattern in which an *<u>object</u>*, named the __*<u>Subject</u>*__, *<u>maintains a list of its dependents</u>*, called __*<u>Observers</u>*__, and *<u>notifies them automatically of any state changes</u>*, usually by *<u>calling one of their methods</u>*.
- Used for implementing *<u>distributed event handling systems</u>*, in __*event driven*__ software.
- In those systems, the subject is usually named a *<u>stream of events</u>* or *<u>stream source of events</u>*, while the observers are called *<u>sinks of events</u>*.
- Most modern programming-languages comprise built-in "event" constructs implementing the observer-pattern components.
- While not mandatory, most *<u>observers</u>* implementations would *<u>use background threads listening for subject-events</u>* and other support mechanisms provided by the kernel (Linux epoll, ...).
- The Observer pattern captures the lion's share of the __*Model-View-Controller*__ architecture.

## *Problems Addressed*
- The Observer pattern addresses the following problems:
  - A __*one-to-many dependency*__ between objects should be defined *<u>without making the objects tightly coupled</u>*.
    - In some scenarios, Tightly coupled objects can be <u>hard to implement</u>, and <u>hard to reuse</u> because they refer to and know about many different objects with different interfaces.
    - In other scenarios, tightly coupled objects can be a better option since the compiler will be able to detect errors at compile-time and optimize the code at the CPU instruction level.
  - It should be ensured that *<u>when one object changes state</u>*, an open-ended/*<u>any number of dependent objects are updated</u>* automatically.
  - It should be possible that one object can *<u>notify an open-ended number of other objects</u>*.
  - A *<u>large monolithic design does not scale</u>* well as new graphing or monitoring requirements are levied.

## Solution with Subject & Observers
- Observer pattern defines __*Subject*__ and __*Observer*__ objects.
- *<u>When a subject changes state</u>*, all *<u>registered observers are notified</u>* and updated automatically (and probably __*<u>asynchronously</u>*__).
- __Responsibility of Subject__:
  - *<u>Maintain a list of observers</u>*
  - *<u>Notify them of state changes</u>* by *<u>calling their </u>*`update()`*<u> operation</u>*.
- __Responsibility of Observers__:
  - *<u>Register/Unregister</u>* themselves on a subject (to get notified of state changes)
  - *<u>Update their state when notified</u>* (synchronize their state with the subject's state).
- This makes subject and observers loosely coupled.
- Subject and observers have no explicit knowledge of each other.
- Observers can be added and removed independently at run-time.
- This notification-registration interaction is also known as __*<u>publish-subscribe</u>*__.

## Coupled Vs Pub-Sub implementations
- __*Tightly Coupled*__ Implementation
  - Typically, the observer pattern is implemented so the __*<u>subject being observed</u>*__<u> is part of the </u>__*<u>object</u>*__<u> for which state changes are being observed</u>.
  - This type of implementation *<u>forces both the observers and the subject to be aware of each other</u>* and *<u>have access to their internal parts</u>*, creating possible __*issues*__ of *<u>scalability</u>*, *<u>speed</u>*, *<u>message recovery</u>* and *<u>maintenance</u>*, the *<u>lack of flexibility in conditional dispersion</u>*, and possible hindrance to desired security measures.
- __*Pub-Sub*__ Implementations:
  - In some (non-polling) implementations of the publish-subscribe pattern (aka the pub-sub pattern), this is solved by creating a dedicated __*<u>message queue server</u>*__ (and sometimes an extra __message handler object__) as an extra stage between the observer and the object being observed, thus decoupling the components.
  - In these cases, the message queue server is accessed by the *<u>observers</u>* with the observer pattern, *<u>subscribing to certain messages</u>* knowing only about the expected message (or not, in some cases), while *<u>knowing nothing about the message sender</u>* itself; the *<u>sender also may know nothing about the observers</u>*.
  - :x: Other implementations of the publish-subscribe pattern, which achieve a similar effect of notification and communication to interested parties, do not use the observer pattern at all.

## Limitations
- Observer Pattern as described in __GoF__ is very basic and does not address:
  - *<u>Removing interest in changes to the observed "subject"</u>* (Unregister)
  - Special logic to be done by the observed "subject" before or after notifying the observers.
  - Recording of change notifications sent
  - Guaranteeing that notifications are being received.
- These concerns are typically handled in __*message queueing systems*__ of which the observer pattern is only a small part.

## Related patterns:
  - Publish–subscribe pattern
  - Mediator
  - Singleton

## UML Class & Sequence diagrams
  ![Observer Pattern UML Class & Sequence diagrams](/assets/images/cpp/design_patterns/observer/Observer_Design_Pattern_UML.jpg)
- In the above UML class diagram,
  - __*Subject*__ class does not update the state of dependent objects directly.
  - Instead, __*Subject*__ refers to the *<u>Observer interface</u>* `update()` for updating state, which makes the *<u>Subject independent</u>* of how the state of dependent objects is updated.
  - The __*Observer1*__ and __*Observer2*__ classes implement the Observer interface and synchronize their state with subject's state.
- The UML sequence diagram shows the run-time interactions:
  - The __*Observer1*__ and __*Observer2*__ objects call `attach(this)` on Subject1 *<u>to register themselves</u>*.
  - Assuming that the state of Subject1 changes, Subject1 calls `notify()` on itself.
  - `notify()` calls `update()` on the registered Observer1 and Observer2 objects, which request the changed data (`getState()`) from Subject1 to update (synchronize) their state.
- The protocol described above specifies a __*pull*__ interaction model. Instead of the <u>Subject pushing</u> what has changed to all Observers, each *<u>Observer is responsible for </u>*__*<u>pulling</u>*__*<u> its particular "window/topic of interest" from the Subject</u>*.
- The __*push*__ model compromises <u>reuse</u>, while the __*pull*__ model is <u>less efficient</u>.

## Reference Implementations
![Observer Pattern Example UML](/assets/images/cpp/design_patterns/observer/Observer_UML-2.svg)

### Python Reference Implementation:
{% highlight python linenos %}
class Observable:
  def __init__(self):
    self._observers = []

  def register_observer(self, observer):
    self._observers.append(observer)

  def notify_observers(self, *args, **kwargs):
    for obs in self._observers:
      obs.notify(self, *args, **kwargs)

class Observer:
  def __init__(self, observable):
    observable.register_observer(self)

  def notify(self, observable, *args, **kwargs):
    print("Got", args, kwargs, "From", observable)

subject = Observable()
observer = Observer(subject)
subject.notify_observers("test", kw="python")

# prints: Got ('test',) {'kw': 'python'} From <__main__.Observable object at 0x0000019757826FD0>
{% endhighlight %}

  ---

### Without Observer Pattern Implementation Sample
{% highlight c++ linenos %}
// The number and type of "Observer" objects are hard-wired in the Subject class.
// The user has no ability to add more observers without modifying Subject class.

class DivObserver {
   int  m_div;
public:
   DivObserver( int div ) { m_div = div; }    // parameterized constructor
   
   void update( int val ) {
      cout << val << " div " << m_div << " is "
           << val / m_div << '\n';
  }
};

class ModObserver {
   int  m_mod;
public:
   ModObserver( int mod ) { m_mod = mod; }    // parameterized constructor
   
   void update( int val ) {
      cout << val << " mod " << m_mod << " is "
           << val % m_mod << '\n';
  }
};

class Subject {
   int  m_value;
   DivObserver  m_div_obj;                    // Composite Objects
   ModObserver  m_mod_obj;
public:
   Subject() : m_div_obj(4), m_mod_obj(3) { } // invoking parameterized constructors

   void set_value( int value ) {
      m_value = value;
      notify();
   }

   void notify() {
      m_div_obj.update( m_value );            // calling Hard-wired objects' methods
      m_mod_obj.update( m_value );
  }
};

int main( void ) {
   Subject  subj;
   subj.set_value( 14 );
}

// Output:
// 14 div 4 is 3
// 14 mod 3 is 2
{% endhighlight %}

  ---

### Observer Pattern Implemenation
{% highlight c++ linenos %}
// The Subject class is now decoupled from the number and type of Observer objects.
// The client/user has asked for two DivObserver delegates (each configured differently),
// and one ModObserver delegate.

class Observer {                          // Abstract class
public:
   virtual void update( int value ) = 0;  // Pure Virtual Method
};

class Subject {
   int  m_value;
   vector  m_views;                       // List of Observers
public:
   void attach( Observer* obs ) {
      m_views.push_back( obs );           // Add observers to list
   }
   
   void set_val( int value ) {
      m_value = value;  notify();         // notify change
   }
   
   void notify() {
      for (int i=0; i < m_views.size(); ++i)
         m_views[i]->update( m_value );   // Push notification to each observer
                                          // invoke `update()` of respective concrete sub-class
  }
};

class DivObserver : public Observer {         // Concrete sub-class of Observer
   int  m_div;
public:
   DivObserver( Subject* model, int div ) {   // Parameterized constructor
      model->attach( this );                  // Subject/Observable object as input
      m_div = div;
   }
   
   /* virtual */ void update( int v ) {       // Implement Pure-Virtual method
      cout << v << " div " << m_div << " is "
           << v / m_div << '\n';
}  };

class ModObserver : public Observer {         // sub-class of Observer Abstract class
   int  m_mod;
public:
   ModObserver( Subject* model, int mod ) {   // Parameterized constructor
      model->attach( this );                  // Subject/Observable object as input
      m_mod = mod;
   }
   
   /* virtual */ void update( int v ) {       // Implement Pure-Virtual method
      cout << v << " mod " << m_mod << " is "
           << v % m_mod << '\n';
}  };

int main( void ) {
   Subject      subj;
   DivObserver  divObs1( &subj, 4 );          // multiple observers of same sub-class
   DivObserver  divObs2( &subj, 3 );          // Require no modification to any implementation
   ModObserver  modObs3( &subj, 3 );          // i.e. Loosely-coupled implementation

   subj.set_val( 14 );                        // state change
}

// Output:
// 14 div 4 is 3
// 14 div 3 is 4
// 14 mod 3 is 2
{% endhighlight %}

## [ToDo: Observer vs Mediator vs Command vs Chain of Responsibility]
- [Observer: Rules of thumb](http://www.vincehuston.org/dp/observer.html)

###### References:
- [Wiki: Observer Pattern](https://en.wikipedia.org/wiki/Observer_pattern)
- [Vince Huston: Observer](http://www.vincehuston.org/dp/observer.html)
- [SourceMaking: Observer Design Pattern](https://sourcemaking.com/design_patterns/observer)


