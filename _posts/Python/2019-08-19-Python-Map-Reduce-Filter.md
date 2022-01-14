---
title: "Python, C++: Map, Reduce, Filter functions"
permalink: /_post/python/map_reduce_filter
date:   2019-08-19 1:33:22 +0530
categories:
  - Python
  - C++11
  - C++14
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Python
  - C++11
  - C++14
author: Akhilesh Moghe
show_author_profile: true
sidebar:
  nav: parallel-computing
---

## Higher-order-functions
- A higher-order function is a function that does at least one of the following:
  - takes one or more *<u>functions as arguments</u>* (i.e. a procedural parameter, which is *<u>a parameter of a procedure/function that is itself a procedure/function</u>*),
  - *<u>returns a function</u>* as its result.
- All other functions are first-order functions.
- In mathematics higher-order functions are also termed *<u>operators</u>* or *<u>functionals</u>*.
- Examples:
  - The *<u>differential operator in calculus</u>* is a common example, since it maps a function to its derivative, which is also a function.
  - __*<u>Map</u>*__ function, found in many functional programming languages, is one example of a higher-order function.
    - It takes as *<u>arguments a function f</u>* and *<u>a collection of elements</u>*,
    - and as the result, *<u>returns a new collection with f applied to each element</u>* from the collection.
  - __*<u>Sorting functions</u>*__, which take *<u>a comparison function as a parameter</u>*, allowing the programmer to separate the sorting algorithm from the comparisons of the items being sorted.
    - The C standard function __*<u>qsort</u>*__ is an example of higher-order-functions.
  - __*<u>Filter</u>*__
  - __*<u>Fold/Reduce</u>*__
  - __*<u>Callback</u>*__
  - __*<u>Tree Traversal</u>*__
- Python Example:
  ```python
  >>> def twice(f):
  ...     def result(x):
  ...         return f(f(x))
  ...     return result

  >>> plus_three = lambda i: i + 3

  >>> g = twice(plus_three)
      
  >>> g(7)
  13
  ```

- C++11 Example:
  ```c++
  #include <iostream>
  #include <functional>

  auto twice = [](const std::function<int(int)>& f)
  {
      return [&f](int x) {
          return f(f(x));
      };
  };

  auto plus_three = [](int i)
  {
      return i + 3;
  };

  int main()
  {
      auto g = twice(plus_three);

      std::cout << g(7) << '\n'; // 13
  }
  ```

- With generic lambdas provided by C++14:
  ```c++
  #include <iostream>

  auto twice = [](const auto& f)
  {
      return [&f](int x) {
          return f(f(x));
      };
  };

  auto plus_three = [](int i)
  {
      return i + 3;
  };

  int main()
  {
      auto g = twice(plus_three);

      std::cout << g(7) << '\n'; // 13
  }
  ```

## Map (higher-order-functions)
- Map is a higher-order function that
  - *<u>applies a given function to each element of a functor, e.g. a list</u>*,
  - *<u>returns a list of results in the same order</u>*.
- It is often called *<u>apply-to-all</u>* when considered in functional form.
- Illustration:\
  ![Map functionality](/assets/images/python/built-in-functions/map/mapping-steps.gif){:.shadow}

- *<u>Note</u>*: This `map` function should not be confused with similarly-titled abstract data type composed of (key,value) pairs.
### *<u>Map operation in C++</u>*
  - *<u>Transform range</u>*: `#include <algorithm>`
    - Applies an operation/function sequentially to the elements of one or two ranges (~list/vector) and stores the result in the range/list that begins at result (~list/vector).
  - Map with 1 list/iterator:
    - __*`std::transform(begin, end, result, func)`*__
    - Applies `func` to each of the elements in the range *<u>[begin,end)</u>* and stores the value returned by each operation in the range that begins at `result`.
  - Map with 2 lists/iterators:
    - __*`std::transform(begin1, end1, begin2, result, func)`*__
    - Calls `func` using each of the elements in the *<u>range [begin1,end1) as first argument</u>*, and the *<u>respective argument in the range that begins at</u>* `begin2` *<u>as second argument</u>*.
    - The value returned by each call is stored in the range that begins at `result`.
#### Example:
  <script src="https://gist.github.com/akhileshmoghe/ca2f027cba83575809d300fa8d944ec7.js"></script>

### *<u>Map operation in Python</u>*
- Map with 1 list/iterator:
  - __*`map(func, list)`*__
- Map with 2 lists/iterators:
  - __*`map(func, list1, list2)`*__
- Map with n lists/iterators:
  - __*`map(func, list1, list2, ...)`*__
- Returns: a `list` in __Python2__.
- Returns: an `iterator` in __Python3__.
- `zip()` and `map()` (Python3.x) stops after the shortest list ends.
- whereas `map()` (Python2.x) and `itertools.zip_longest()` (Python3.x) extends the shorter lists with `None` items.

#### *<u>Built-in map() function</u>*
- It return an iterator that applies function to every item of iterable, yielding the results.
- If additional iterable arguments are passed, function must take that many arguments and function is applied to the items from all iterables in parallel.
- With multiple iterables, the iterator stops when the shortest iterable is exhausted.
- __*<u>lambda</u>*__ Anonymous Function is usually used with __*map()*__.
- It's not mandatory to use lambda function with map(), we can pass any user defined function also.
##### Example:

  ```python
    >>> List = [4,3,2,1]
    >>> SquaredList = list(map(lambda x: x**2, List))
    >>> 
    >>> print('List:', List, 'SquaredList:', SquaredList)
    ('List:', [4, 3, 2, 1], 'SquaredList:', [16, 9, 4, 1])
    >>> 
    >>> 
    >>> Tuple = 1,2,3,4
    >>> MultiplyListTupleElementally = list(map(lambda x,y: x*y, List, Tuple))
    >>> 
    >>> print('Multiply List & Tuple Elementally: ', MultiplyListTupleElementally, 'List: ', List, 'Tuple: ', Tuple)
    ('Multiply List & Tuple Elementally: ', [4, 6, 6, 4], 'List: ', [4, 3, 2, 1], 'Tuple: ', (1, 2, 3, 4))
  ```

#### *<u>itertools.starmap(function, iterable)</u>*
- Make an iterator that computes the function using arguments obtained from the iterable.
- Used instead of __*<u>map()</u>*__ when argument parameters are already grouped in tuples from a single iterable (the data has been “pre-zipped”).
- The difference between map() and starmap() parallels the distinction between function(a,b) and function(*c).
- Roughly equivalent to:

  ```python
    def starmap(function, iterable):
        # starmap(pow, [(2,5), (3,2), (10,3)]) --> 32 9 1000
        for args in iterable:
            yield function(*args)
  ```

#### *<u>Parallel Map function: Pool.map()</u>*
- In Python, one can create *<u>a pool of processes</u>* which will carry out tasks submitted to it with the __*Pool*__ class.
- A process pool object which controls a pool of worker processes to which jobs can be submitted. It supports *<u>asynchronous results</u>* with timeouts and *<u>callbacks</u>* and has a *<u>parallel map implementation</u>*.
- This method *<u>chops the iterable into a number of chunks</u>* which it submits to the process pool as separate tasks.
- The (approximate) size of these chunks can be specified by setting *<u>chunksize</u>* to a positive integer.
- Note that it *may cause high memory usage* for very long iterables. Consider using [__*<u>imap()</u>*__](https://docs.python.org/3/library/multiprocessing.html#multiprocessing.pool.Pool.imap) or [__*<u>imap_unordered()</u>*__](https://docs.python.org/3/library/multiprocessing.html#multiprocessing.pool.Pool.imap_unordered) with *<u>explicit chunksize option for better efficiency</u>*.
- [Parallel Processing: Map](/_post/parallel_computing#map)

  ---

## Reduce
- In functional programming, __*fold*__ (also termed __*reduce*__, __*accumulate*__, __*aggregate*__, __*compress*__, or __*inject*__) refers to a family of *<u>higher-order functions</u>* that *<u>analyze a recursive data</u>* structure and through use of a given combining operation, *<u>recombine the results of recursively processing</u>* its constituent parts, building up a return value.
### *<u>Reduce/Accumulate function in C++</u>*
  - __*<u>std::accumulate</u>*__: [`#include <numeric>`]
    - Accumulate values in range
    - __*`std::accumulate (InputIterator first, InputIterator last, T init, BinaryOperation binary_op);`*__
    - Returns the result of accumulating all the values in the range `[first,last)` to `init`.
    - `init`: Initial value for the accumulator.
    - `binary_op`:
      - Binary operation taking an element of type `T` as first argument and an element in the range as second, and which returns a value that can be assigned to type `T`.
      - This can either be *<u>a function pointer</u>* or a *<u>function object</u>*.
      - The *operation shall not modify the elements passed as its arguments*.
      - The default operation is to add the elements up, but a different operation can be specified as binary_op.
#### Example
  <script src="https://gist.github.com/akhileshmoghe/00c3698c164cc7402a3f5cf4311f77fa.js"></script>

### *<u>Reduce/Accumulate function in Python</u>*
  - The [functools](https://docs.python.org/3/library/functools.html) module is for higher-order functions: functions that act on or return other functions.
  - __*<u>functools.reduce(function, iterable[, initializer])</u>*__:
    - Syntax:
      - `functools.reduce(func, list, initval)`
      - `functools.reduce(lambda x,y: func(y,x), reversed(list), initval)`
      - `functools.reduce(func, list)`
      - `functools.reduce(lambda x,y: func(y,x), reversed(list))`
    - Apply function of two arguments cumulatively to the items of iterable (~lists/vectors), from left to right, so as to reduce the iterable to a single value.
    - For example, `reduce(lambda x, y: x+y, [1, 2, 3, 4, 5])` calculates `((((1+2)+3)+4)+5)`.
    - The left argument, __x__, *<u>is the accumulated value</u>* and the right argument, __y__, *<u>is the update value from the iterable</u>*.
    - If the optional __*initializer*__ is present, it is placed before the items of the iterable in the calculation, and *<u>serves as a default</u>* when the iterable is empty.
    - If initializer is not given and iterable contains only one item, the first item is returned.
    - `functools.reduce()` applies same `function` to items in a sequence.
    - Uses result of 1st operation as 1st parameter input to 2nd operation.
    - Returns an item, not a list.

#### Example
    ```python
      >>> from functools import reduce
      >>> List = [4,3,2,1]
      >>> allElementAddition = reduce(lambda x,y: x+y, List)
      >>> print('allElementAddition: ', allElementAddition, 'List: ', List)

      Output:
      >>> 'allElementAddition: ', 10, 'List: ', [4, 3, 2, 1]
    ```

  ---

## *<u>Filter</u>*
- In *<u>functional programming</u>*, filter is a *<u>higher-order function</u>* that processes a data structure (usually a *<u>list</u>*) in some order to produce a new data structure containing exactly those elements of the original data structure for which a given *<u>predicate</u>* (~filter operation) returns the boolean value *<u>true</u>*.
- Illustration:\
  ![filter-steps](/assets/images/python/built-in-functions/filter/Filter-steps.gif){:.shadow}
### *<u>Filter operation in C++</u>*
#### *std::remove_copy_if*
    - `#include <algorithm>`
    - __*`std::remove_copy_if(begin, end, result, prednot)`*__
    - Copy range removing values.
    - Copies the elements in the range `[begin,end)` to the range beginning at `result`, except those elements for which `pred` returns `true`.
    - The resulting range is shorter than [first,last) by as many elements as matches, which are "removed".
    - *<u>pred</u>*:
      - *<u>Unary function</u>* that accepts an element in the range as argument, and returns a value convertible to bool.
      - The value returned indicates whether the element is to be removed from the copy (if true, it is not copied).
      - The function shall not modify its argument.
      - This can either be a *<u>function pointer</u>* or a *<u>function object</u>*.
    - *<u>result</u>*:
      - Output iterator to the initial position of the range where the resulting sequence is stored.

##### *<u>Example</u>*:
{% highlight c++ linenos %}
// remove_copy_if example
#include <iostream>     // std::cout
#include <algorithm>    // std::remove_copy_if
#include <vector>       // std::vector

bool IsOdd (int i) { return ((i%2)==1); }

int main () {
  int myints[] = {1,2,3,4,5,6,7,8,9};
  std::vector<int> myvector (9);

  std::remove_copy_if (myints,myints+9,myvector.begin(),IsOdd);

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
  administrator@PTL011669:std_remove_copy_if$ ./std_remove_copy_if 
  myvector contains: 2 4 6 8 0 0 0 0 0
  administrator@PTL011669:std_remove_copy_if$ 

{% endhighlight %}
  
#### *std::replace_copy_if*
- `#include <algorithm>`
- __*`OutputIterator replace_copy_if (InputIterator first, InputIterator last, OutputIterator result, UnaryPredicate pred, const T& new_value);`*__
- Copy range replacing value.
- Copies the elements in the range `[first,last)` to the range beginning at `result`, replacing those for which `pred` returns `true` by `new_value`.

##### Example:
{% highlight c++ linenos %}
// replace_copy_if example
#include <iostream>     // std::cout
#include <algorithm>    // std::replace_copy_if
#include <vector>       // std::vector

bool IsOdd (int i) { return ((i%2)==1); }

int main () {
  std::vector<int> foo,bar;

  // set some values:
  for (int i=1; i<10; i++) foo.push_back(i);          // 1 2 3 4 5 6 7 8 9

  bar.resize(foo.size());   // allocate space
  std::replace_copy_if (foo.begin(), foo.end(), bar.begin(), IsOdd, 0);
                                                        // 0 2 0 4 0 6 0 8 0

  std::cout << "bar contains:";
  for (std::vector<int>::iterator it=bar.begin(); it!=bar.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
  administrator@PTL011669:std_remove_copy_if$ ./std_remove_copy_if 
  myvector contains: 2 4 6 8 0 0 0 0 0
  administrator@PTL011669:std_remove_copy_if$ 

{% endhighlight %}

#### *std::copy_if*
- `#include <algorithm>`
- __*`std::copy_if(begin, end, result, pred)`*__  [C++11]
- Copy certain elements of range.
- Copies the elements in the range `[begin,end)` for which `pred` returns `true` to the range beginning at `result`.
- pred
  - *<u>Unary function</u>* that accepts an element in the range as argument, and returns a value convertible to `bool`.
  - The value returned indicates whether the element is to be copied (if true, it is copied).
  - The function shall not modify any of its arguments.
  - This can either be a function pointer or a function object.

##### Example:
{% highlight c++ linenos %}
// copy_if example
#include <iostream>     // std::cout
#include <algorithm>    // std::copy_if, std::distance
#include <vector>       // std::vector

int main () {
  std::vector<int> foo = {25,15,5,-5,-15};
  std::vector<int> bar (foo.size());

  // copy only positive numbers:
  auto it = std::copy_if (foo.begin(), foo.end(), bar.begin(), [](int i){return !(i<0);} );
  bar.resize(std::distance(bar.begin(),it));  // shrink container to new size

  std::cout << "bar contains:";
  for (int& x: bar) std::cout << ' ' << x;
  std::cout << '\n';

  return 0;
}
{% endhighlight %}

### *<u>Filter function in Python</u>*
- Syntax:
  - `filter(<function>, iterable)`
- Construct an iterator from those elements of iterable for which function returns `true`.
- Iterable may be either a sequence, a container which supports iteration, or an iterator.
- If function is 'None', the identity function is assumed, that is, all elements of iterable that are false are removed.
- See [itertools.filterfalse()](https://docs.python.org/3/library/itertools.html#itertools.filterfalse) for the complementary function that returns elements of iterable for which function returns `false`.

#### Example:
{% highlight python linenos %}
>>> 
>>> 
>>> List = [0, 1,2,3,4,5]
>>> filteredList = list(filter(lambda x: x>2, List))
>>> print('Filtered List(>2): ', filteredList, 'List: ', List)
('Filtered List(>2): ', [3, 4, 5], 'List: ', [0, 1, 2, 3, 4, 5])
>>> 
>>> 
>>> print('--- <function> is None ----')
--- <function> is None ----
>>> defaultFilterList = list(filter(None, List))
>>> print('defaultFilterList: ', defaultFilterList, 'List: ', List)
('defaultFilterList: ', [1, 2, 3, 4, 5], 'List: ', [0, 1, 2, 3, 4, 5])
>>> 
>>> 
>>> numberList = [-4,-3,-2,0,1,3,5,7]
>>> filterNegativeNumbersLambdaFunc = lambda x: x>0
>>> positiveList = list(filter(filterNegativeNumbersLambdaFunc, numberList))
>>> print('Negative Numbers Filtered: ', positiveList, 'numberList: ', numberList)
('Negative Numbers Filtered: ', [1, 3, 5, 7], 'numberList: ', [-4, -3, -2, 0, 1, 3, 5, 7])
>>> 
>>> 
>>> numberList = [-4,-3,-2,0,1,3,5,7]
>>> def filterNegativeNumbersFunc(number):
...     newList = []
...     if number>0:
...         newList.append(number)
...     return newList
... 
>>> 
>>> positiveList = list(filter(filterNegativeNumbersFunc, numberList))
>>> print('Negative Numbers Filtered: ', positiveList, 'numberList: ', numberList)
('Negative Numbers Filtered: ', [1, 3, 5, 7], 'numberList: ', [-4, -3, -2, 0, 1, 3, 5, 7])
>>> 
>>> 
{% endhighlight %}


###### References:
  - [Higher-order_function](https://en.wikipedia.org/wiki/Higher-order_function)
  
  - [Map (higher-order function)](https://en.wikipedia.org/wiki/Map_(higher-order_function))
    - [std::transform](https://www.cplusplus.com/reference/algorithm/transform/)
    - [Built-in map() function](https://docs.python.org/3/library/functions.html#map)
    - [itertools.starmap](https://docs.python.org/3/library/itertools.html#itertools.starmap)
    - [multiprocessing.pool.Pool.map](https://docs.python.org/3/library/multiprocessing.html#multiprocessing.pool.Pool.map)
  
  - [Fold/Reduce (higher-order function)](https://en.wikipedia.org/wiki/Fold_(higher-order_function))
    - [std::accumulate](https://www.cplusplus.com/reference/numeric/accumulate/)
    - [functools.reduce](https://docs.python.org/3/library/functools.html#functools.reduce)
  
  - [Filter (higher-order-function)](https://en.wikipedia.org/wiki/Filter_(higher-order_function))
    - [std::remove_copy_if](https://www.cplusplus.com/reference/algorithm/remove_copy_if/)
    - [std::replace_copy_if](https://www.cplusplus.com/reference/algorithm/replace_copy_if/)
    - [std::copy_if](https://www.cplusplus.com/reference/algorithm/copy_if/)
    - [Python Built-in function: filter](https://docs.python.org/3/library/functions.html#filter)

  
  
  
