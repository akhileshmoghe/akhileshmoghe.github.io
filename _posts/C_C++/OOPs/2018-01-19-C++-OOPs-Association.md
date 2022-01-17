---
title:  "C++ OOPs: Association"
date:   2018-01-19 12:33:22 +0530
permalink: /_posts/c-c++/oops/association
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

## Association

<object data="/assets/docs/c-cpp/oops/association/Association_Relationship.pdf" type="application/pdf" width="900px" height="1000px">
  <embed src="/assets/docs/c-cpp/oops/association/Association_Relationship.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/c-cpp/oops/association/Association_Relationship.pdf">Download PDF</a>.</p>
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

  ---

## Association in brief:
- To qualify as an association, an object and another object must have the following relationship:
  - The associated object (member) is otherwise unrelated to the object (class)
  - The associated object (member) can belong to more than one object (class) at a time
  - The associated object (member) does not have its existence managed by the object (class)
  - The associated object (member) may or may not know about the existence of the object (class)
- Unlike an Aggregation, where the relationship is always Unidirectional,
- in an Association, the *<u>relationship may be</u>* __*<u>Unidirectional</u>*__ or __*<u>Bidirectional</u>*__ (where the two objects are aware of each other).
- Association models as __*uses-a*__ relationship.
- Most often, Associations are implemented using *<u>POINTERS</u>*, where the object points at the associated object.
- In general, you should *<u>avoid bidirectional associations</u>* if a unidirectional one will do, as they add complexity and tend to be harder to write without making errors.

{% highlight c++ linenos %}
#include <iostream>
#include <string>
#include <vector>
 
// Since these classes have a circular dependency, we're going to forward declare Doctor
class Doctor;
 
class Patient
{
private:
  std::string m_name;
  std::vector<Doctor *> m_doctor; // so that we can use it here

  // We're going to make addDoctor private because we don't want the public to use it.
  // They should use Doctor::addPatient() instead, which is publicly exposed
  // We'll define this function after we define what a Doctor is
  // Since we need Doctor to be defined in order to actually use anything from it
  void addDoctor(Doctor *doc);
 
public:
  Patient(std::string name)
      : m_name(name)
  {
  }
 
  // We'll implement this function below Doctor since we need Doctor to be defined at that point
  friend std::ostream& operator<<(std::ostream &out, const Patient &pat);
 
  std::string getName() const { return m_name; }
 
  // We're friending Doctor so that class can access the private addDoctor() function
  // (Note: in normal circumstances, we'd just friend that one function, but we can't
  // because Doctor is forward declared)
  friend class Doctor;
};
 
class Doctor
{
private:
  std::string m_name;
  std::vector<Patient *> m_patient;
 
public:
  Doctor(std::string name):
      m_name(name)
  {
  }
 
  void addPatient(Patient *pat)
  {
    // Our doctor will add this patient
    m_patient.push_back(pat);
		
    // and the patient will also add this doctor
    pat->addDoctor(this);
  }
 
 
  friend std::ostream& operator<<(std::ostream &out, const Doctor &doc)
  {
    unsigned int length = doc.m_patient.size();
    if (length == 0)
    {
      out << doc.m_name << " has no patients right now";
      return out;
    }
 
    out << doc.m_name << " is seeing patients: ";
    for (unsigned int count = 0; count < length; ++count)
      out << doc.m_patient[count]->getName() << ' ';
 
    return out;
  }

  std::string getName() const { return m_name; }
};
 
void Patient::addDoctor(Doctor *doc)
{
  m_doctor.push_back(doc);
}
 
std::ostream& operator<<(std::ostream &out, const Patient &pat)
{
  unsigned int length = pat.m_doctor.size();
  if (length == 0)
  {
    out << pat.getName() << " has no doctors right now";
    return out;
  }
 
  out << pat.m_name << " is seeing doctors: ";
  for (unsigned int count = 0; count < length; ++count)
    out << pat.m_doctor[count]->getName() << ' ';
 
  return out;
}
 
int main()
{
  // Create a Patient outside the scope of the Doctor
  Patient *p1 = new Patient("Dave");
  Patient *p2 = new Patient("Frank");
  Patient *p3 = new Patient("Betsy");
 
  Doctor *d1 = new Doctor("James");
  Doctor *d2 = new Doctor("Scott");
 
  d1->addPatient(p1);
 
  d2->addPatient(p1);
  d2->addPatient(p3);
 
  std::cout << *d1 << '\n';
  std::cout << *d2 << '\n';
  std::cout << *p1 << '\n';
  std::cout << *p2 << '\n';
  std::cout << *p3 << '\n';
 
  delete p1;
  delete p2;
  delete p3;
	
  delete d1;
  delete d2;
 
  return 0;
}
{% endhighlight %}

  ---

## Reflexive Association
- Sometimes objects may have a relationship with other objects of the same type. This is called a Reflexive Association.
- A good example of a Reflexive Association is the relationship between a university course and its prerequisites (which are also university courses).
- This can lead to a chain of Associations (a course has a prerequisite, which has a prerequisite, etc,..)

{% highlight c++ linenos %}
#include <string>
class Course
{
private:
    std::string m_name;
    // 'Course' class itself has a 'Course' member  -->  Reflexive Association
    Course *m_prerequisite;
 
public:
    Course(std::string &name, Course *prerequisite=nullptr):
        m_name(name), m_prerequisite(prerequisite)
    {
    }
 
};
{% endhighlight %}

  ---

## Indirect Association
- In an Association, A pointer is not strictly required to directly link objects together.
- Any kind of data that allows you to link two objects together suffices.
- In the following example, we show how a Driver class can have a unidirectional association with a Car without actually including a Car pointer member.
- we have a CarLot holding our cars.
- The Driver, who needs a car, doesn't have a pointer to his Car, instead, he has the ID of the car, which we can use to get the Car from the CarLot when we need it.

{% highlight c++ linenos %}
#include <iostream>
#include <string>
 
class Car
{
private:
  std::string m_name;
  int m_id;
 
public:
  Car(std::string name, int id)
      : m_name(name), m_id(id)
  {
  }
 
  std::string getName() { return m_name; }
  int getId() { return m_id;  }
};
 
// Our CarLot is essentially just a static array of Cars and a lookup function to retrieve them.
// Because it's static, we don't need to allocate an object of type CarLot to use it
class CarLot
{
private:
  static Car s_carLot[4];
 
public:
//	CarLot() = delete;				// Ensure we don't try to allocate a CarLot
                              // C++ 11 feature, hence here using simple constructor

  CarLot();
 
  static Car* getCar(int id)
  {
    for (int count = 0; count < 4; ++count)
      if (s_carLot[count].getId() == id)
        return &(s_carLot[count]);
		
    return nullptr;
  }
};
 
Car CarLot::s_carLot[4] = { Car("Prius", 4), Car("Corolla", 17), Car("Accord", 84), Car("Matrix", 62) };
 
class Driver
{
private:
  std::string m_name;
  int m_carId; // we're associated with the Car by ID rather than pointer
 
public:
  Driver(std::string name, int carId)
      : m_name(name), m_carId(carId)
  {
  }
 
  std::string getName() { return m_name; }
  int getCarId() { return m_carId; } 
};

int main()
{
  Driver d("Franz", 17); // Franz is driving the car with ID 17
 
  Car *car = CarLot::getCar(d.getCarId()); // Get that car from the car lot
	
  if (car)
    std::cout << d.getName() << " is driving a " << car->getName() << '\n';
  else
    std::cout << d.getName() << " couldn't find his car\n";
 
  return 0;
}
{% endhighlight %}


