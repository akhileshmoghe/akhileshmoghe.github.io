---
title:  "C++ OOPs: Composition"
date:   2018-01-17 12:33:22 +0530
permalink: /_posts/c-c++/oops/composition
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

## Composition

<object data="/assets/docs/c-cpp/oops/composition/Composition_Relationship.pdf" type="application/pdf" width="900px" height="1000px">
  <embed src="/assets/docs/c-cpp/oops/composition/Composition_Relationship.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/c-cpp/oops/composition/Composition_Relationship.pdf">Download PDF</a>.</p>
  </embed>
</object>

  ---

## Composition vs aggregation vs association summary

  |---
  | Property | Composition | Aggregation | Association |
  |:-|:-|:-|:-|
  | Relationship type | Whole/part | Whole/part | Otherwise unrelated |
  | Members can belong to multiple classes | No | Yes | Yes |
  | Members existence managed by class | Yes | No | No |
  | Directionality | Unidirectional | Unidirectional | Unidirectional or bidirectional |
  | Relationship verb | Part-of | Has-a | Uses-a |
  |---

## Composition in brief
- Types of Composition:
  - Composition
  - Aggregation

- To qualify as a composition, an object and a part must have the following relationship:
  - The part (member) is __*part of*__ the object (class)
  - The part (member) can __*only belong to one object*__ (class) at a time
  - The part (member) has its __*existence managed by the object*__ (class)			(__*Death Relationship*__)
  - The part (member) __*does not know about the existence of the object*__ (class)	(__*Unidirectional Relationship*__)

- Composition models __*part-of*__ relationships.

- Composition has nothing to say about the transferability of parts.

- The parts of a composition can be *<u>Singular</u>* or *<u>Multiplicative</u>*.
  - for example, a heart is a singular part of the body, but a body contains 10 fingers (which could be modeled as an array).

- If you can design a class using Composition, you should design a class using Composition.
- Classes designed using Composition are straightforward, flexible, and robust (in that they clean up after themselves nicely).

- Variants on the Composition theme:
  - A Composition may defer creation of some parts until they are needed.
    - For example, a string class may not create a dynamic array of characters until the user assigns the string some data.
- A Composition may opt to use a part that has been given to it as INPUT rather than create the part itself.
- A Composition may DELEGATE Destruction of its parts to some other object (e.g. to a garbage collection routine).

- The key point here is that the *<u>Composition should manage it's parts without the user object needing to manage anything</u>*.

## Sample Code
{% highlight c++ linenos %}
#include <string>
#include <iostream>

class Point2D
{
  private:
    // these integers are a "part-of" Point2D class/object.
    // there existance depends on Point2D object.
    int m_x;
    int m_y;

  public:
    // A default constructor
    Point2D()
        : m_x(0), m_y(0)
    {
    }

    // A specific constructor
    Point2D(int x, int y)
        : m_x(x), m_y(y)
    {
    }

    // An overloaded output operator
    friend std::ostream& operator<<(std::ostream& out, const Point2D &point)
    {
        std::cout << "operator << in Point2D class" << std::endl;
        out << "(" << point.m_x << ", " << point.m_y << ")";
        return out;
    }

    // Access functions
    void setPoint(int x, int y)
    {
        m_x = x;
        m_y = y;
    }

};

class Creature
{
  private:
    std::string m_name;
    Point2D m_location;

  public:
  /*
    A composition may opt to use a part that has been given to it as input rather than create the part itself.
      - Here, "Point2D" is an input to "Creature" class.
      - Life Cycle, memory management of this incoming "Point2D/location" object is responsibility of "main()" function, where it is actually created.
      - Creature class will manage life span of "m_location" object.
  */
    Creature(const std::string &name, const Point2D &location)
        : m_name(name), m_location(location)
    {
    }

    friend std::ostream& operator<<(std::ostream& out, const Creature &creature)
    {
        std::cout << "operator << in Creature class" << std::endl;
        out << creature.m_name << " is at " << creature.m_location;
        return out;
    }

    void moveTo(int x, int y)
    {
        m_location.setPoint(x, y);
    }
};

int main()
{
    std::cout << "Enter a name for your creature: ";
    std::string name;
    std::cin >> name;
    Creature creature(name, Point2D(4, 7));

    while (1)
    {
        // print the creature's name and location
        std::cout << creature << '\n';

        std::cout << "Enter new X location for creature (-1 to quit): ";
        int x=0;
        std::cin >> x;
        if (x == -1)
            break;

        std::cout << "Enter new Y location for creature (-1 to quit): ";
        int y=0;
        std::cin >> y;
        if (y == -1)
            break;
	  
        creature.moveTo(x, y);
    }

    return 0;
}
{% endhighlight %}


