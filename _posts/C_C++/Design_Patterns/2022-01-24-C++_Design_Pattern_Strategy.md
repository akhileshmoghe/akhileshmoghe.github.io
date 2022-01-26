---
title:  "Design Patterns: Strategy"
date:   2022-01-24 5:33:22 +0530
permalink: /_posts/c-c++/design_patterns/strategy
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
- Enables *<u>selecting an algorithm at runtime</u>*. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.
- Deferring the decision about which algorithm to use until runtime allows the calling code to be more flexible and reusable.
  - For instance,
  - a class that performs <u>validation on incoming data</u> may use the strategy pattern to select a validation algorithm depending on the <u>type of data</u>, the <u>source of the data</u>, <u>user choice</u>, or other discriminating factors.
  - <u>These factors are not known until run-time and may require radically different validation</u> to be performed.
  - The validation algorithms (strategies), encapsulated separately from the validating object, may be used by other validating objects in different areas of the system (or even different systems) without code duplication.
- The strategy pattern stores a __*<u>reference </u>*__*<u>to some code in a data structure</u>* and retrieves it with __*<u>function pointer</u>*__, the __*<u>first-class function</u>*__, __*<u>classes or class instances</u>*__.
  - [*<u>first-class function</u>*](https://en.wikipedia.org/wiki/First-class_function):
    - A programming language is said to have first-class functions if the language supports:
      - *<u>passing functions as arguments to other functions</u>*
      - *<u>returning them as the values from other functions</u>*
      - *<u>assigning them to variables</u>*
      - *<u>storing them in data structures</u>*

## Strategy and SOLID:Open/Closed Principle
- According to the strategy pattern, the *<u>behaviors of a class should not be inherited</u>*. Instead, they *<u>should be encapsulated using interfaces</u>*.
- This is compatible with the [Open/Closed Principle (OCP)](_posts/c-c++/design_patterns/solid_principles#openclosed-principle), which proposes that *<u>classes should be open for extension but closed for modification</u>*.
- The strategy pattern uses [*<u>composition instead of inheritance</u>*](/_posts/c-c++/oops/composition-vs-inheritance).
- In the strategy pattern, *<u>behaviors are defined as separate interfaces</u>* and specific classes implement these interfaces.
- This allows *<u>better decoupling between the behavior and the class that uses the behavior</u>*. The behavior can be changed without breaking the classes that use it, and the classes can switch between behaviors by changing the specific implementation used without requiring any significant code changes. Behaviors can also be changed at run-time as well as at design-time.

## UML Class & Sequence diagrams
![Strategy Pattern UML Class & Sequence diagrams](/assets/images/cpp/design_patterns/observer/Design_Strategy_Design_Pattern_UML.jpg)
- In the above UML class diagram,
  - The `Context class` *<u>doesn't implement an algorithm directly</u>*.
  - Instead, `Context` refers to the `Strategy interface` for performing an algorithm (`strategy.algorithm()`), which makes `Context` independent of how an algorithm is implemented.
  - The `Strategy1` and `Strategy2` classes implement the Strategy interface, that is, *<u>implement (encapsulate) an algorithm</u>*.
- The UML sequence diagram shows the run-time interactions:
  - The `Context object` *<u>delegates an algorithm</u>* to different `Strategy objects`.
  - First, `Context` calls `algorithm()` on a `Strategy1` object, which performs the algorithm and returns the result to `Context`.
  - Thereafter, `Context` changes its strategy and calls `algorithm()` on a `Strategy2` object, which performs the algorithm and returns the result to `Context`.

## Python Reference Implementation:
{% highlight python linenos %}
import abc
from typing import List

class Bill:
    def __init__(self, billing_strategy:"BillingStrategy"):
        self.drinks: List[float] = []
        self._billing_strategy = billing_strategy

    @property
    def billing_strategy(self) -> "BillingStrategy":
        return self._billing_strategy

    @billing_strategy.setter
    def billing_strategy(self, billing_strategy: "BillingStrategy") -> None:
        self._billing_strategy = billing_strategy

    def add(self, price: float, quantity: int) -> None:
        self.drinks.append(self.billing_strategy.get_act_price(price * quantity))

    def __str__(self) -> str:
        return str(f"£{sum(self.drinks)}")

class BillingStrategy(abc.ABC):
    @abc.abstractmethod
    def get_act_price(self, raw_price: float) -> float:
        raise NotImplementedError


class NormalStrategy(BillingStrategy):
    def get_act_price(self, raw_price: float) -> float:
        return raw_price


class HappyHourStrategy(BillingStrategy):
    def get_act_price(self, raw_price: float) -> float:
        return raw_price * 0.5


def main() -> None:
    normal_strategy = NormalStrategy()
    happy_hour_strategy = HappyHourStrategy()

    customer_1 = Bill(normal_strategy)
    customer_2 = Bill(normal_strategy)

    # Normal billing
    customer_1.add(2.50, 3)
    customer_1.add(2.50, 2)

    # Start happy hour
    customer_1.billing_strategy = happy_hour_strategy
    customer_2.billing_strategy = happy_hour_strategy
    customer_1.add(3.40, 6)
    customer_2.add(3.10, 2)

    # End happy hour
    customer_1.billing_strategy = normal_strategy
    customer_2.billing_strategy = normal_strategy
    customer_1.add(3.10, 6)
    customer_2.add(3.10, 2)

    # Print the bills;
    print(customer_1)
    print(customer_2)

if __name__ == "__main__":
    main()
{% endhighlight %}

###### References:
- 
