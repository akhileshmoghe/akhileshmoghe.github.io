---
title:  "Sorting Algorithms"
date:   2018-12-14 1:33:22 +0530
permalink: /_posts/c-c++/ds_algorithms/sorting
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

## Visualization is the Best way to Learn Algorithms:
<iframe
  src="https://visualgo.net/en/sorting"
  width="950px" height="650px">
</iframe>

Courtesy: [VISUALGO.NET](https://visualgo.net/en)

## Selection Sort: O(n<sup>2</sup>)
- Selection Sort maintains *<u>2 sub-arrays in same array</u>*:
  - Sorted elements
  - Unsorted elements
- In each iteration One element i.e. minimum is found and is swapped to the Sorted part of array.
- In following implementation, there are 2 for() loops running for 'arrSize' i.e. n,
- *<u>Time Complexity</u>*: __*O(n<sup>2</sup>)*__
- Auxiliary Space Complexity:	__*O(1)*__
  - As everything is accomplished in same array, no extra space required.

### Illustration:

  ![Selection Sort](/assets/images/cpp/algorithms/selection_sort/SelectionSort.jpg){:.shadow}

### Gist

<script src="https://gist.github.com/akhileshmoghe/74c6ab3e8fed63621e4ee82ec8fe9f3b.js"></script>

  ---

### Selection Sort for "Array of Strings"

<script src="https://gist.github.com/akhileshmoghe/d095fa96d28b9403c1921ef1e7f29c7b.js"></script>

  ---

### Stable Selection Sort
- A sorting algorithm is said to be stable: *<u>if the elements with Same Values/Keys retain Same Sequence in Output array as in Input array</u>*.
- e.g.
  - Input : `4A 5 3 2 4B 1`
  - Unstable Selection Sorting Output:
  - Output : `1 2 3 4B 4A 5`
  - Stable Sorting Output:
  - Output : `1 2 3 4A 4B 5`

- Selection sort works by finding the minimum element and then inserting it in its correct position by swapping with the element which is in the position of this minimum element. This makes Selection Sort unstable.
- Swapping might impact in pushing a key(let's say A) to a position greater than the key(let's say B) which are equal keys, which makes them out of the desired order.
- In the above example 4A was pushed after 4B and after complete sorting this 4A remains after this 4B. Hence resulting in unstability.
- Selection sort can be made Stable if instead of swapping,	*<u>the minimum element is placed in its position pushing every element one step forward</u>*.

#### Gist

<script src="https://gist.github.com/akhileshmoghe/8d10136801cfc63af9b5d5f71036c608.js"></script>

  ---

## Insertion Sort: O(n<sup>2</sup>)
- *<u>Time Complexity</u>*: __*O(n<sup>2</sup>)*__
- Auxiliary Space: __*O(1)*__
- Maximum Time to sort  -->  if elements are sorted in Reverse Order.
- Minimum Time (Order of n)  -->  when elements are Already Sorted.
- Algorithmic Paradigm:	Incremental Approach
- *<u>In-Place Sorting</u>*: Yes
- *<u>Stable</u>*: Yes
- Uses:
  - Used when number of elements are "Small".
  - Also be useful when input Array is Almost Sorted, only few elements are misplaced in complete big array.
- Analogy:
  - Insertion sort works the way many people sort a hand of playing cards.
  - We start with an empty left hand and the cards face down on the table.
  - We then remove one card at a time from the table and insert it into the correct position in the left hand.
  - To find the correct position for a card, we compare it with each of the cards already in the hand, from right to left,
  - At all times, the cards held in the left hand are sorted, and these cards were originally the top cards of the pile on the table.

### Illustrations:
  ![Insertion Sort](/assets/images/cpp/algorithms/insertion-sort/insertion-sort.png)

### Psuedo Code:

  ![Psuedo Code](/assets/images/cpp/algorithms/insertion-sort/insertion-sort-psuedo-code.png)
  ![Psuedo Code Explaination](/assets/images/cpp/algorithms/insertion-sort/insertion-sort-psuedo-code-explaination.png)

### Gist

<script src="https://gist.github.com/akhileshmoghe/88ff581b7de0553dbf01fb8e9f7ac3c0.js"></script>

  ---

### Optimizing Insertion Sort using with Binary Search
- In normal Insertion Sort,
  - it takes *<u>O(n) comparisions(at nth iteration) in Worst Case</u>*.
  - We can *<u>reduce it to O(log n) by using Binary Search</u>*.
- The algorithm as a whole still has a running Worst Case running time of O(n<sup>2</sup>), because of the series of swaps required for each insertion.

#### Gist

<script src="https://gist.github.com/akhileshmoghe/d9f7a588077cc8bd977e877d7a2c302b.js"></script>

  ---

## Bubble Sort: *O(n<sup>2</sup>)*
- Sorting is done by: *<u>Comparing Adjacent items</u>* and *<u>Swapping them</u>* if they are not in order.
- Algorithm repeatedly passes through array and Swaps element to make them in order.
- When there is NO Swap in an iteration, that's end of Sorting.
- *<u>Optimization</u>*:
  - Usually Array is sorted in 2nd Last iteration itself, before the NO Swap iteration, but Bubble Sort still iterates again to reach NO Swap iteration.
  - The bubble sort algorithm can be optimized by observing that the n-th pass finds the n-th largest element and puts it into its final place. So, the inner loop can avoid looking at the last n − 1 items when running for the n-th time.
  - i.e., by avoiding the last iteration.
- __*<u>Time Complexity</u>*__:
  - *<u>Worst</u>* and Average Case Time Complexity: __*O(n<sup>2</sup>*__
    - Worst case occurs when *<u>array is Reverse Sorted</u>*.
  - *<u>Best</u>* Case Time Complexity: __*O(n)*__
    - Best case occurs when *array is already Sorted*.
- __*<u>Auxiliary Space</u>*__: __*O(1)*__
- Boundary Cases:
  - Bubble sort takes minimum time (Order of n) when elements are already Sorted.
- *<u>Sorting In Place</u>*, No extra Array: Yes
- *<u>Stable Sorting</u>*: Yes

- *<u>Applications</u>*:
  - As per Wikipedia, Bubble sort is not much useful in practical use-cases, rather Insertion sort performs better with same time complexity.
  - In-spiye of theis, in *<u>Computer Graphics</u>*, Bubble Sort is popular for its *<u>Capability to Detect a very Small Error (like swap of just two elements) in almost-Sorted arrays</u>* and fix it with just Linear Complexity (2n).

### Illustrations:

  ![Bubble Sort gif](/assets/images/cpp/algorithms/bubble_sort/Bubble-sort-example-300px.gif){:.shadow}

### Gist
<script src="https://gist.github.com/akhileshmoghe/18ea0543dda69c52a6b9f5ba368cdbf5.js"></script>


## Merge Sort:
- An efficient, general-purpose Algorithm
- Comparison based Sorting Algorithm
- *<u>Divide and Conquer</u>* Algorithm
  - It divides input array/List in two halves, calls itself for the two halves and then merges the two sorted halves.
- Merge sort is often *<u>preferred for sorting a linked list</u>*.
  - The slow random-access performance of a linked list makes some other algorithms (such as quicksort) perform poorly, and others (such as heapsort) completely impossible.
- Basic Logic:
	
  MergeSort(arr[], l,  r)\
  If r > l\
    1) &nbsp;&nbsp; Find the middle point to divide the array into two halves:\
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; middle m = (l+r)/2\
    2) &nbsp;&nbsp; Call mergeSort for first half:\
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Call mergeSort(arr, l, m)\
    3) &nbsp;&nbsp; Call mergeSort for second half:\
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Call mergeSort(arr, m+1, r)\
    4) &nbsp;&nbsp; Merge the two halves sorted in step 2 and 3:\
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Call merge(arr, l, m, r)\
  {:.success}

- *<u>Time Complexity</u>*: __*O(nLogn)*__
  - Time complexity of Merge Sort is O(nLogn) in all 3 cases (*worst*, *average* and *best*) as Merge Sort always Divides the array in Two Halves and take Linear Time to Merge Two Halves.
	
- *<u>Auxiliary Space</u>*:	__*O(n)*__
  - As we create *<u>2 temporary Sub Arrays</u>*, totaling size of 'n'.

- Algorithmic Paradigm: *<u>Divide and Conquer</u>*
- *<u>Sorting In Place</u>*: No
- *<u>Stable</u>*: Yes

### Illustrations:

  ![Merge-sort-example](/assets/images/cpp/algorithms/merge_sort/Merge-sort-example-300px.gif){:.shadow}
  
  ![Merge-Sort-Illustration](/assets/images/cpp/algorithms/merge_sort/Merge-Sort-Illustration.png){:.shadow}

### Comparison with other algorithms
- Heapsort has the same time bounds as merge sort, it requires only Θ(1) auxiliary space instead of merge sort's Θ(n).
- Quicksort implementations generally outperform merge sort for sorting RAM-based arrays.
- On the other hand, merge sort is a stable sort and is more efficient at handling slow-to-access sequential media.
- Merge sort is often the best choice for sorting a linked list: in this situation it is relatively easy to implement a merge sort in such a way that it requires only Θ(1) extra space, and the slow random-access performance of a linked list makes some other algorithms (such as quicksort) perform poorly, and others (such as heapsort) completely impossible.

### Use-case
- Merge sort is often the best choice for sorting a linked list.

### Gist
  <script src="https://gist.github.com/akhileshmoghe/ffb5feac346aedf8ca2c5caa93daafff.js"></script>

###### References:
  - [Bubble sort](https://en.wikipedia.org/wiki/Bubble_sort)
  - [Merge Sort](https://en.wikipedia.org/wiki/Merge_sort#Comparison_with_other_sort_algorithms)
  - [Merge Sort: GeekforGeeks](https://www.geeksforgeeks.org/merge-sort/)
  
  
  
