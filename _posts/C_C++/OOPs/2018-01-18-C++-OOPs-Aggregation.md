---
title:  "C++ OOPs: Aggregation"
date:   2018-01-18 12:33:22 +0530
permalink: /_posts/c-c++/oops/aggregation
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

## Aggregation

<object data="/assets/docs/c-cpp/oops/aggregation/Aggregation_Relationship.pdf" type="application/pdf" width="900px" height="1000px">
  <embed src="/assets/docs/c-cpp/oops/aggregation/Aggregation_Relationship.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/c-cpp/oops/aggregation/Aggregation_Relationship.pdf">Download PDF</a>.</p>
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

## Aggregation in brief
- To qualify as an aggregation, a whole object and its parts must have the following relationship:
  - The part (member) is __*part of*__ the object (class)
    - The part (member) __*can belong to more than one object*__ (class) at a time
    - The part (member) __*does not have its existence managed by the object*__ (class)
    - The part (member) __*does not know about the existence of the object*__ (class) (__*Unidirectional Relationship*__)

- When an Aggregation is created, the Aggregation is not responsible for creating the parts.
- When an Aggregation is destroyed, the Aggregation is not responsible for destroying the parts.

- Aggregation models __*has-a*__ relationships (a department has teachers, the car has an engine).

- In a Composition, we typically add our parts to the composition using normal member variables.
- In an Aggregation, we also add parts as member variables. However, these member variables are typically either __*REFERENCES*__ or __*POINTERS*__ that are used to point at objects that have been *<u>created outside the scope of the class</u>*.

- Consequently, an Aggregation usually either takes the objects it is going to point to as Constructor parameters, or it begins empty and the subobjects are added later via access functions(setter()/getter()) or operators.

- Because these parts exist outside of the scope of the class, when the class is destroyed, the *<u>POINTER or REFERENCE member variable will be destroyed</u>* (but __*not deleted*__). Consequently, the *<u>parts themselves will still exist</u>*.

- Summarizing Composition and Aggregation:
  - Compositions:
    - Typically use normal member variables
    - Can use pointer values if the composition class automatically handles allocation/deallocation
    - Responsible for creation/destruction of parts
  - Aggregations:
    - Typically use POINTER or REFERENCE members that point to or reference objects that live outside the scope of the aggregate class
    - Not responsible for creating/destroying parts

- While aggregations can be extremely useful, they are also potentially more dangerous. Because aggregations do not handle deallocation of their parts, that is left up to an external party to do so.
- For this reason, *<u>compositions should be favored over aggregations</u>*.

- In Aggregation, the relationship is always Unidirectional.


## Sample Code
{% highlight c++ linenos %}
#include <string>
#include <iostream>

class Teacher
{
private:
    std::string m_name;

public:
    Teacher(std::string name)
        : m_name(name)
    {
    }
 
    std::string getName() { return m_name; }
};
 
class Department
{
private:
    Teacher *m_teacher; // This dept holds only one teacher for simplicity, but it could hold many teachers
 
public:
    Department(Teacher *teacher = nullptr)
        : m_teacher(teacher)
    {
      std::cout << "Department has a Teacher named " << teacher->getName() << std::endl;
    }
};
 
int main()
{
    // Create a teacher outside the scope of the Department
    Teacher *teacher = new Teacher("Bob"); // create a teacher
    {
        // Create a department and use the constructor parameter to pass
        // the teacher to it.
        Department dept(teacher);
 
    } // dept goes out of scope here and is destroyed
 
    // Teacher still exists here because dept did not delete m_teacher
 
    std::cout << teacher->getName() << " still exists!" << std::endl;
 
    delete teacher;
 
    return 0;
}
{% endhighlight %}


