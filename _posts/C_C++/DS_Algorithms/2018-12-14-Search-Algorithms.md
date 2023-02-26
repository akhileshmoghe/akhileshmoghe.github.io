---
title:  "Search Algorithms"
date:   2018-12-14 1:33:22 +0530
permalink: /_posts/c-c++/ds_algorithms/search
categories:
  - C++
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
author: Akhilesh Moghe
show_author_profile: true
---

## Search Algorithms
### Linear Search
- Linear Search is very Rarely used.
- *<u>Time Complexity</u>* is __*O(n)*__.

<script src="https://gist.github.com/akhileshmoghe/f6b73c8ec3aec1da0f29ecc188b3b09e.js"></script>

  ---

### Binary Search
- Binary Search needs __*SORTED array*__.
- *<u>Time Complexity</u>* is __*O(Log n)*__.
- Binary Search Logic:
  - 1st Compare x with Middle element, if matched return.
  - if x is greater than Middle element, search in right half of array.
  - if x is smaller than Middle element, search in left half of array.
- *<u>Iterative Vs Recursive</u>*:
  - Both approaches will have the same time complexity “__*O(log(n))*__”, but they will different in term of space usage.
  - :x: &nbsp;&nbsp; *<u>Recursive</u>* May reach to __*log(n)*__ space (because of the stack),
  - :heavy_check_mark: &nbsp;&nbsp; *<u>Iterative</u>* BS should be done in __*O(1)*__ space complexity.

<script src="https://gist.github.com/akhileshmoghe/dedb71da501ae090d7db19c67861d2d3.js"></script>

  ---

### Jump Search
- Jump Search Algorithm is for __*SORTED arrays*__.
- This Algorithm check 'fewer elements' (than linear search) by *<u>jumping ahead by fixed steps</u>* or skipping some elements.
- *<u>Time Complexity</u>* is __*O(Sqrt(n))*__.
- Algorithm:

  1) &nbsp;&nbsp; arr[],\
  2) &nbsp;&nbsp; n = size,\
  3) &nbsp;&nbsp; m = Steps to Jump,\
  4) &nbsp;&nbsp; x = element to find,\
  5) &nbsp;&nbsp; Search for indexex = arr[0], arr[m], arr[2m], ... , arr[km]\
  6) &nbsp;&nbsp; Find an interval as:\
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __`arr[km] < x < arr[(k+1)m]`__\
  7) &nbsp;&nbsp; Perform 'Linear Search' from arr[km] till we get x.
  {:.success}

- __Best Step size__ = `√arrLength`
  - &nbsp;&nbsp; __`int step = sqrt((double) arrLength);`__

- *<u>Complexity</u>*:
  - __worst case__:
    - `n/m` Jumps
    - if the last checked value is greater than the element to be searched for,	we perform 'm-1' comparisons more for linear search.
    - (n/m)+(m-1)
    - --> n/m --> Jumps
    - --> m-1 --> Linear Search after Jumps
		
  - __Best Case__
    - Step/Jump size ==> (m = √n)
\
&nbsp;

- *<u>IMPORTANT</u>*:
  - The optimal size of a block to be jumped is (√ n).
    - This makes the time complexity of Jump Search O(√ n).
  - *<u>The time complexity of Jump Search is between Linear Search ( ( O(n) ) and Binary Search ( O (Log n) )</u>*.
  - *<u>Binary Search is better than Jump Search</u>*, but Jump search has an advantage that we traverse back only once (Binary Search may require up to O(Log n) jumps.
  - Consider a situation where the element to be searched is the smallest element or smaller than the smallest. So in a system where binary search is costly, we use Jump Search.

<script src="https://gist.github.com/akhileshmoghe/5e5c30f15a2de995cce5cdb448b82303.js"></script>

  ---

### Interpolation Search:
- Interpolation Search needs a __*Sorted Array*__.
- The Interpolation Search is an improvement over Binary Search for instances, where the *<u>values in a sorted array are uniformly distributed</u>*.
- Binary Search always goes to the middle element to check. On the other hand, Interpolation Search may go to different locations according to the value of the key being searched.
  - For example, if the value of the key is closer to the last element, interpolation search is likely to start search toward the end side.

#### Partition Logic for Interpolation Search, as Binary Search makes Partition at 'Mid' position
- To find the position to be searched, Interpolation Search uses following formula.
- The idea of formula is to return:
  - Higher value of 'pos' --> when element to be searched is closer to arr[hi].
	- Smaller value of 'pos'--> when element to be searched is closer to arr[lo]
    `pos = lo + [ (x-arr[lo])*(hi-lo) / (arr[hi]-arr[Lo]) ]`
    arr[] ==> Array where elements need to be searched
    x     ==> Element to be searched
    lo    ==> Starting index in arr[]
    hi    ==> Ending index in arr[]

#### Algorithm:

  Rest of the Interpolation algorithm is same except the above partition logic.\
  1) &nbsp;&nbsp; In a loop, calculate the value of “pos” using above probe position formula.\
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If it is a match, return the index of the item, and exit.\
  3) &nbsp;&nbsp; If the item(x) is Less than arr[pos],\
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; calculate the probe position of the Left sub-array.\
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Otherwise calculate the probe position in the Right sub-array.\
  4) &nbsp;&nbsp; Repeat until a match is found or the sub-array reduces to zero.\
  {:.success}
  
#### Time Complexity:
- If elements are uniformly distributed, then __*O(log log n))*__.
- In worst case it can take upto: __*O(n)*__.

#### Auxiliary Space:
- __*O(1)*__

<script src="https://gist.github.com/akhileshmoghe/f0f4db87db3158798a51d30b122f37dd.js"></script>

  ---

### Exponential Search
- Find range where element is present
- Do Binary Search in above found range.

#### Time Complexity:
- __*O(Log i)*__
  - --> i --> is the index where 'x' searched elemnet will be.
  - further details:
    [Exponential_search#Performance](https://en.wikipedia.org/wiki/Exponential_search#Performance)
- Auxiliary Complexity:
  - Recursive Binary Search - __*O(Log n)*__
  - Iterative Binary Search - __*O(1)*__

#### Usage:
- Exponential Binary Search is particularly useful for "unbounded searches", where size of array is infinite.
- It works better than Binary Search for bounded arrays, and also when the element to be searched is closer to the first element.

<script src="https://gist.github.com/akhileshmoghe/61442fc24e2b8bb277edac6ccdc3fd7e.js"></script>

  ---
  
### Ternary Search
- Time Complexity: __*O(Log3(n))*__

<script src="https://gist.github.com/akhileshmoghe/97831d0578fb9e3947506fa0bbf74ea0.js"></script>

###### References:
  - [big-o-notation-time-and-space-complexity](https://www.interviewcake.com/article/python/big-o-notation-time-and-space-complexity)
  - [Big-O Complexity Chart](https://www.bigocheatsheet.com/)
  - [Algorithms-DataStructures-BigONotation](http://cooervo.github.io/Algorithms-DataStructures-BigONotation/index.html)
  
  
  
