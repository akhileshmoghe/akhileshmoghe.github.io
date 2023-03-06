---
title:  "Design Patterns: GRASP Patterns of designing an application or module "
date:   2021-12-7 5:33:22 +0530
permalink: /_posts/c-c++/design_patterns/grasp_pattern
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

This write-up will explain different __*<u>GRASP Patterns</u>*__ i.e., *<u>General Responsibility Assignment Software Patterns (or Principles)</u>*, explained in book: [Applying UML and Patterns](https://www.amazon.com/gp/product/0131489062/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=fluentcpp-20&creative=9325&linkCode=as2&creativeASIN=0131489062&linkId=43cfc4d0a6ea922ef663317e4f91db85), which are more important in day to day programming life than the whole lot of __GoF design patterns__.
GRASP is a <u>set of 9 fundamental principles in OOPs design</u> and responsibility assignment.
These patterns solve some software problem common to many software development projects.

## Information Expert (Information Hiding)
- *<u>Problem Addressed</u>*: How will you assign a responsibility to a module/class?
- *<u>Solution</u>*:
  - Assign responsibility to the class that has the information or data needed to fulfill it.
  - *<u>If you have an operation to do, and this operations needs some input data, then you should consider assigning the responsibility of carrying out this operation in the class that contains the input data for it</u>*.
- This pattern is used to determine where to delegate responsibilities such as methods, computed fields, and so on.
- It helps keeping the data localized, i.e. __*<u>Data Encapsulation</u>*__.
- It reduces relationships between the classes. (Classes are self-sufficient in terms of data to carryout their tasks.)
- <u>Related Pattern or Principle</u>:
  - __*Low Coupling*__
  - __*High Cohesion*__

## Creator
- The creation of objects is one of the most common activities in an object-oriented system.
- Which class is responsible for creating the objects is a fundamental property of the relationship between objects of those particular classes.
- *<u>Problem Addressed</u>*: Which class should creates object of another class `(A)`?
- *<u>Solution</u>*:
  - In general, Assign the responsibility to create object of class`(A)` to class `(B)` if one, or preferably more, of the following apply:
    - Instances of `(B)` __*contain*__ or [compositely aggregate](/_posts/c-c++/oops/aggregation) instances of `(A)`
    - Instances of `(B)` __*records or keeps track of*__ instances of `(A)`
    - Instances of `(B)` __*closely use*__ instances of `(A)`
    - Instances of `(B)` have the __*initializing information*__ for instances of `(A)` and pass it on creation.
- If you put together two part of the code that are semantically close ( the construction of `(A)`, and the code that works a lot with `(A)`), then they become easier to reason about than if they were far apart.
- <u>Related Pattern or Principle</u>:
  - __*Low Coupling*__
  - __*Factory pattern*__

## Low Coupling
- Coupling is a measure of how strongly one element is *<u>connected to</u>*, or *<u>has knowledge of</u>*, or *<u>relies on</u>* other elements.
- *<u>Coupling introduces complexity</u>*, if only because the code can then no longer be understood in isolation.
- The design principle of <u>low coupling encourages to keep coupling/interactions/dependence between classes low</u>.
- Low coupling is an *<u>evaluative pattern</u>* that dictates how to assign responsibilities for the following benefits:
  - __*lower dependency between the classes*__
  - __*change in one class having a lower impact on other classes*__
  - __*higher reuse potential*__

## Protected variations
- The protected variations pattern *<u>protects elements from the variations in other elements</u>* (objects, systems, subsystems) by wrapping the cause of instability within an __*interface*__ and using __*polymorphism*__ *<u>to create various implementations of this interface</u>*.
- <u>Problem Addressed</u>: How to design objects, subsystems, and systems so that the variations or instability in these elements does not have an undesirable impact on other elements?
- <u>Solution</u>:
  - Identify points of predicted variation or instability; assign responsibilities to create a stable interface around them.
  - The principle of Protected variations helps reducing the impacts of the changes of the code of one part `(A)` on another part `(B)`. The code of part `(B)` is protected against the variations of the code of part `(A)`, by organizing the responsibilities around __*stable interfaces*__.

## Indirection
- The Indirection pattern is another way to *<u>reduce coupling</u>* by creating an *<u>intermediary class</u>* (or any kind of component) between two classes `(A)` and `(B)`. This way, the changes in each one of `(A)` and `(B)` don’t affect the other one. The intermediary class absorbs the impact by adapting its code rather than `(A)` or `(B)` (or more other classes).
- The indirection pattern supports __*<u>low coupling</u>*__ and reuses potential between two elements by *<u>assigning the responsibility of mediation between them to an intermediate object</u>*.
- An example of this is the *<u>introduction of a controller component</u>* for mediation between *<u>data (model)</u>* and its *<u>representation (view)</u>* in the __*<u>model-view-controller</u>*__ pattern. This ensures that *<u>coupling between them remains low</u>*.
- <u>Problem Addressed</u>:
  - Where to assign responsibility, to avoid direct coupling between two (or more) things?
  - How to de-couple objects so that low coupling is supported and reuse potential remains higher?
- <u>Solution</u>:
  - Assign the responsibility to *<u>an intermediate object to mediate between other components</u>* or services so that they are not directly coupled.
  - *<u>The intermediary creates an indirection between the other components</u>*.
- This relates a lot to the Adapter design pattern, even though the __*<u>Adapter design pattern</u>*__ <u>is rather made to connect two existing</u> *<u>incompatible interfaces</u>*. But it also has the effect of protecting each one against the changes of the other.
- Difference between __*Protected variations*__ and __*Indirection*__:
  - __*Protected variations*__ is about *<u>designing </u>*__*<u>interfaces</u>*__*<u> in the existing components</u>*.
  - whereas __*Indirection*__ is about *<u>introducing a </u>*__*<u>new component</u>*__*<u> in the middle</u>*.

## Polymorphism
- According to the polymorphism principle, responsibility for defining the *<u>variation of behaviors based on</u>* __*<u>type of object</u>*__ is assigned to the type for which this variation happens. This is achieved using polymorphic operations.
- The user of the type should *<u>use</u>* __*<u>polymorphic operations</u>*__ *<u>instead of explicit branching based on type</u>*.
- <u>Problem</u>:
  - How to handle alternatives based on type?
  - How to create pluggable software components?
- <u>Solution</u>:
  - When related alternatives or *<u>behaviors vary by type (class), assign responsibility for the behavior — using</u>* __*<u>polymorphic operations</u>*__ — to the types for which the behavior varies. (Polymorphism has several related meanings. In this context, it means "giving the same name to services in different objects".)
- The usage for polymorsphism is when there are several ways to accomplish a task, and you want to decouple the clients of this task from the various pieces of code that implement the various ways to perform it.
- The Polymorphism principle is very close to the GoF __*<u>Strategy pattern</u>*__, if not identical.
- Polymorphism contributes to the __*<u>Low Coupling</u>*__ principle by:
  - Reducing the behavioural branching implementation in one class
  - Instead creating different classes according to behaviour
  - And thus reducing impact on other classes of a change in one class.

## High Cohesion
- High cohesion is an *<u>evaluative pattern</u>* that attempts to keep __*<u>objects appropriately focused, manageable and understandable</u>*__.
- High cohesion is generally used in *<u>support of</u>* __*<u>low coupling</u>*__.
- High cohesion means that the __*<u>responsibilities</u>*__ *<u>of a given element / object / class are strongly related and</u>* __*<u>highly focused</u>*__.
- This is the principle of __*<u>do one thing and do it well</u>*__.
- The principle of high cohesion also *<u>applies to</u>* other elements of the code, such as:
  - *<u>functions</u>*
  - *<u>modules</u>*
  - *<u>systems</u>*
- *<u>Breaking programs into classes and subsystems</u>* is an example of activities that increase the __*cohesive*__ properties of a system.
- Alternatively, __*low cohesion*__ is a situation in which a given element/object/class has *<u>too many unrelated responsibilities</u>*. Elements with low cohesion often suffer from being hard to comprehend, reuse, maintain and change.

## Pure fabrication
- Usually, our code's objects/classes map to the real concepts/objects in the project's domain.
  - e.g., in IoT project, we tend to have *<u>CloudManager</u>*, *<u>SystemUpdater</u>* classes in the code.
- *<u>Problem Addressed</u>*:
  - Sometimes you have a responsibility to assign, and it seems to not fit well in any domain class.
  - And according to the principle of __*<u>High cohesion</u>*__ above, you shouldn’t force a responsibility into a class that is already doing something else.
  - That’s when the principle of __*Pure fabrication*__ comes into play.
- *<u>Solution</u>*:
  - *<u>Create an independent class that does not map to a domain object, and let it achieve this new responsibility in a cohesive way</u>*.
- Related concept: __Services__
- <u>Related Patterns and Principles</u>:
  - __*Low Coupling*__
  - __*High Cohesion*__

## Controller
- The controller pattern assigns the *<u>responsibility of dealing with events</u>* may be *<u>system events</u>* to a non-UI class that represents the overall system or a use case scenario.
- A controller object is a *<u>non-user interface object</u>* responsible for receiving or handling a system event.
- <u>Problem</u>: Who should be responsible for handling an input system event?
- <u>Solution</u>:
  - A use case controller should be used to deal with all system events of a use case.
    - For example, A Log Monitor or System Health Monitor class/object.
  - And use case controller may be used for more than one use case.
    - For instance, for the use cases Create User and Delete User, one can have a single class called UserController, instead of two separate use case controllers.
- The controller is defined as the first object beyond the UI layer that receives and coordinates ("controls") a system operation.
- The controller should *<u>delegate the work that needs to be done to other objects</u>*; it coordinates or controls the activity. It *<u>should not do much work itself</u>*.
  - For example, the Brokers as in Message Queuing applications like MQTT Brokers.
- The __*GRASP Controller*__ can be thought of as being a *<u>part of the application or service layer</u>* (assuming that the application has made an explicit distinction between the application or service layer and the domain layer) in an object-oriented system with common layers in an information system logical architecture.
- <u>Related Pattern or Principle</u>:
  - __*Command*__
  - __*Facade*__
  - __*Layers*__
  - __*Pure Fabrication*__

  ---

###### References
  - [GRASP: 9 Must-Know Design Principles for Code](https://www.fluentcpp.com/2021/06/23/grasp-9-must-know-design-principles-for-code/)
  - [GRASP (object-oriented design)](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design))

  ---
