---
title:  "Python Caching: == operator Vs is-operator"
permalink: /_post/python/python_caching
date:   2022-01-10 1:33:22 +0530
#modify_date: 2021-12-31
categories:
  - Python
  - Memory Management
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Python
  - Memory Management
author: Akhilesh Moghe
show_author_profile: true
#sidebar:
#  nav: parallel-computing
---

## Python Caching

{% highlight python linenos %}
>>>
>>> x = 42
>>> x = 'shrubbery'
>>> 
{% endhighlight %}

- *<u>Python caches and reuses small integers and small strings</u>*.
- the object 42 here is probably not literally reclaimed; instead, it will likely remain in a system table to be reused the next time you generate a 42 in your code.
- Most kinds of objects, though, are reclaimed immediately when they are no longer referenced;
- *<u>The caching mechanism is irrelevant to your code</u>*.

## Equality Check with `==` operator
{% highlight python linenos %}
>>> 
>>> L = [1,2,3]
>>> M = L                 # M and L reference the same object
>>> L == M                # Same values
True
>>> L is M                # Same objects
True
>>> 
{% endhighlight %}

- The first technique here, the *<u>== operator</u>*, tests whether the two referenced objects have the *<u>same values</u>*;
- The second method, the *<u>is operator</u>*, instead tests for *<u>object identity</u>* — it returns *<u>True only if both names point to the exact same object</u>*, so it is a much stronger form of equality testing.
- *<u>is</u>* simply compares the pointers that implement references, and it serves as *<u>a way to detect shared references in your code</u>* if needed. It returns *<u>False if the names point to equivalent but different objects</u>*, as is the case when we run two different literal expressions:

## Equality Check with `is` operator
{% highlight python linenos %}
>>> 
>>> L = [1,2,3]           # M and L reference different objects
>>> M = [1,2,3]
>>> L == M                # Same values
True
>>> L is M                # Different objects
False
>>> 
{% endhighlight %}

- Check another case with small objects like integers:

{% highlight python linenos %}
>>> 
>>> X = 42
>>> Y = 42                # Should be two different objects
>>> 
>>> X == Y
True
>>> 
>>> X is Y                # Same object anyhow: caching at work!
True
>>> 
{% endhighlight %}

- In this interaction, X and Y should be == (same value), but not *is* (same object) because we ran two different literal expressions (42).
- Because small *<u>integers and strings are </u>* __*<u>cached</u>*__*<u> and reused</u>*, though, *<u>is tells us they reference the same single object</u>*.
- In fact, if you really want to look under the hood, you can always *<u>ask Python how many references there are to an object: the</u>* __*<u>getrefcount</u>*__*<u> function in the standard sys module returns the object’s reference count</u>*.

- This object caching and reuse is irrelevant to your code (unless you run the is check!).
- Because you cannot change immutable numbers or strings in place, it doesn’t matter how many references there are to the same object—every reference will always see the same, unchanging value. Still, this behavior reflects one of the many ways Python op-
timizes its model for execution speed.





