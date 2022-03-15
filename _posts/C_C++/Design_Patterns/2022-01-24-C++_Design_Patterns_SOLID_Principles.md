---
title:  "Design Patterns: SOLID Principles of designing an application or module "
date:   2022-01-24 5:33:22 +0530
permalink: /_posts/c-c++/design_patterns/solid_principles
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

<style>
r { color: Red }
o { color: Orange }
g { color: Green }
y { color: yellow}
gr { color: grey}
</style>

- SOLID is an acronym for five design principles intended to make software designs more understandable, flexible, and maintainable.
- Although the SOLID principles apply to any *<u>object-oriented design</u>*, they can also form a core philosophy for methodologies such as *<u>agile development</u>* or *<u>adaptive software development</u>*.

## [*<u>Single-Responsibility Principle</u>*](https://en.wikipedia.org/wiki/Single-responsibility_principle)
- The single-responsibility principle (SRP) principle states that every *<u>module</u>*, *<u>class</u>* or *<u>function</u>* in a program should have responsibility over a single part of that program's functionality, and it should __*encapsulate*__ that part.
- The reason it is important to keep a class focused on a single concern is that it makes the class more robust. If there is a change to the one of the responsibility of the class, there is a greater danger that the other part of code with a different responsibility will break if it is part of the same class.
- So, the two aspects of the problem that are really two separate responsibilities, and should be in separate classes or modules.
- It would be a bad design to couple two things that change for different reasons at different times.

## [*<u>Open–Closed Principle</u>*](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)
- *<u>Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification</u>*.
- Such an entity can allow its behaviour to be extended without modifying its own source code.
- __*Polymorphic open–closed principle*__:
  - Uses *<u>abstracted interfaces</u>*, where the implementations can be changed and multiple implementations could be created and *<u>polymorphically</u>* substituted for each other.
  - This advocates [*<u>inheritance</u>*](/_posts/c-c++/oops/inheritance) *<u>from abstract base classes</u>*.
  - Interface specifications can be reused through inheritance but implementation need not be.
  - The existing interface is closed to modifications and new implementations must, at a minimum, implement that interface.

## [*<u>Liskov Substitution Principle</u>*](https://www.youtube.com/watch?v=gnKx1RW_2Rk&ab_channel=kudvenkat)
- The principle is based on the concept of __*substitutability*__ – a principle in object-oriented programming stating that *<u>an object (such as a class) and a sub-object (such as a class that extends the first class) must be interchangeable without breaking the program</u>*.
- Liskov's notion of a behavioural subtype defines a notion of substitutability for objects; that is, __*<u>if S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program</u>*__.
- __*<u><r>Derived types must be completely substitutable for their base types</r></u>*__, i.e., Base class object/pointer can be substituted by a Derived class object.
- It is an extension of the Open-Close Principle.
- Implementation Guidelines:
  - *<u>No new exceptions can be thrown by the subtype</u>*.
  - *<u>Client should not know which specific subtype they are calling</u>*, i.e., when base class pointer can accept any of the derived class passed to it at run-time.
  - *<u>New derived classes just extend without replacing the functionality of old classes</u>*.
- *<g>Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it</g>*.

## [*<u>Interface Segregation Principle</u>*](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- The __*Interface Segregation Principle (ISP)*__ states that *<u>no code should be forced to depend on methods it does not use</u>*.
- *<u><g>ISP splits interfaces</g></u>* that are very large into *<u><g>smaller and more specific ones</g></u>* so that clients will only have to know about the methods that are of interest to them. Such shrunken interfaces are also called *<u><g>role interfaces</g></u>*.
- ISP is intended to keep a *<u><g>system decoupled</g></u>* and thus *<u><g>easier to refactor, change, and redeploy</g></u>*.
- ISP is one of the five SOLID principles of object-oriented design, similar to the [*<u>High Cohesion Principle</u>*](/_posts/c-c++/design_patterns/grasp_pattern#high-cohesion) of GRASP.
- Beyond object-oriented design, ISP is also a key principle in the *<u><o>design of distributed systems</o></u>* in general and *<u><o>microservices</o></u>* in particular. ISP is one of the six IDEALS principles for microservice design.
- Within object-oriented design, interfaces provide layers of abstraction that simplify code and create a barrier preventing coupling to dependencies. A system may become so coupled at multiple levels that it is no longer possible to make a change in one place without necessitating many additional changes. Using an *<u>interface</u>* or *<u>an abstract class</u>* can prevent this side effect.

## [*<u>Dependency Inversion Principle</u>*](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
- In object-oriented design, the __*Dependency Inversion Principle*__ is a specific methodology for *<u>loosely coupling software modules</u>*.
- The principle states:
  - *<u><r>High-level modules should not import anything from low-level modules</r></u>*. *<u>Both should depend on abstractions (e.g., interfaces)</u>*.
  - *<u><r>Abstractions should not depend on details</r></u>*. *<u>Details (concrete implementations) should depend on abstractions</u>*.
- The idea behind the two points of this principle is that *<u>when designing the interaction between a high-level module and a low-level one, the interaction should be thought of as an abstract interaction between them</u>*. This not only has implications on the design of the high-level module, but also on the low-level one: the low-level one should be designed with the interaction in mind.
- In <gr>Traditional layers pattern, In conventional application architecture</gr>, lower-level components are designed to be consumed by higher-level components. In this composition, *<gr>higher-level components depend directly upon lower-level components</gr>* to achieve some task. *<gr>This dependency upon lower-level components limits the reuse opportunities of the higher-level components</gr>*.
  ![Traditional_Layers_Pattern](/assets/images/design_patterns/solid_pattern/Traditional_Layers_Pattern.png)
- In *<g>Dependency inversion pattern</g>*, with the addition of an *<u>abstract layer</u>*, both high- and lower-level layers reduce the traditional dependencies from top to bottom. Nevertheless, the "inversion" concept does not mean that lower-level layers depend on higher-level layers directly. Both layers should depend on abstractions (interfaces) that expose the behavior needed by higher-level layers.\
  ![DIPLayersPattern](/assets/images/design_patterns/solid_pattern/DIPLayersPattern.png)
- In a direct application of dependency inversion, *<u>the abstracts are owned by the upper/policy layers</u>*. This architecture *<u>groups the higher components and the abstractions together in the same package</u>*. The *<u>lower-level layers are created by inheritance/implementation of these abstract classes or interfaces</u>*.
- __*Implementations*__:
  - Approach-1:
    - A direct implementation *<u>packages the high-level classes with service abstracts classes in one library</u>*.
    - In this implementation <u>high-level components and low-level components are distributed into separate packages/libraries</u>, where the *<u>interfaces defining the behavior/services are required and owned by and exist within the high-level component's library</u>*.
    - The implementation of the high-level component's interface by the low-level component requires that the *<u><r>low-level component package depend upon the high-level component for compilation, thus inverting the conventional dependency relationship</r></u>*.
    ![Dependency_inversion](/assets/images/design_patterns/solid_pattern/Dependency_inversion.png)
    - Figures 1 and 2 illustrate code with the same functionality, however in Figure 2, *<u>an interface has been used to invert the dependency</u>*. The direction of dependency can be chosen to maximize policy code reuse, and eliminate cyclic dependencies.
    - In this version of DIP, the *<gr>lower layer component's dependency on the interfaces/abstracts in the higher-level layers makes re-utilization of the lower layer components difficult</gr>*. This implementation instead ″inverts″ the traditional dependency from top-to-bottom to the opposite, from bottom-to-top.
  - Approach-2:
    - *<g>A more flexible solution extracts the abstract components into an independent set of packages/libraries</g>*:
    - *<g><u>The separation of each layer into its own package encourages re-utilization of any layer, providing robustness and mobility</u></g>*.\
    ![DIPLayersPattern_v2](/assets/images/design_patterns/solid_pattern/DIPLayersPattern_v2.png)

&nbsp;
###### References:
- [Wiki: SOLID](https://en.wikipedia.org/wiki/SOLID)
- [A very good explaination of SOLID Principles in simple examples](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/)


