---
title:  "C++ STL: Associative Containers and Use cases"
date:   2018-11-5 12:33:22 +0530
permalink: /_posts/c-c++/stl/associative_containers
categories:
  - C++
  - Data Structures
  - STL
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - C++
  - Data Structures
  - STL
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will explain different __*<u>Associative containers</u>*__, their respective features and mainly their appropriate use-cases.	Elements in associative containers are __*referenced by their key*__ and not by their absolute position in the container.

	
## Set
  - Features:
    - Value is itself the Key
    - Unique Elements(=Keys)
      - No two elements in the container can have Equivalent Keys.
      - Each Value must be Unique
  - Value of an element *<u>CANNOT be Modified once inserted</u>* (i.e. elements are always constant)
  - Elements CAN be __Inserted/Deleted__ from set
  - Follows Specific Order (__*Sorted*__)
    - elements in a set are always Sorted following a specific "__Strict Weak Ordering__" criterion indicated by its internal comparison object (of type Compare).
  - Sets are typically implemented as "__*<u>Binary Search Trees</u>*__".
  - *<u>Set is Slower than Unordered_set</u>*.
					
### set vs unordered_set:
  - *<u>std::set is generally Slower than unordered_set</u>* in order to access individual elements by their key.
  - std::set allow Direct Iteration on subsets based on their order.
	
### Sets Decision Criteria:
  - Key:Value Same, i.e. Keys Only
  - Key:Value pair (Kind of)
  - NO Duplicates
  - NO Modification once inserted
  - Sorted in Order (of Keys)
  - Order is important
  - Need to find element by Keys


## Multiset:
  - Features:
    - All features as of set, except following:
    - Multiple equivalent keys
      - Multiple elements in the container can have equivalent keys.
      - Multiple elements can have equivalent values.

### Sets Decision Criteria:
  - Key:Value Same, i.e. Keys Only
  - Key:Value pair (Kind of)
  - NO Modification once inserted
  - Sorted in Order (of Keys)
  - Order is important
  - Need to find element by Keys
	- Duplicates Allowed
	
- Note:
  - this container is not defined in its own header, but shares header <set> with set.
	
			
			
## Unordered_set
- Features:
  - All Same features as of set, except following:
    - NO particular Order (NO Sorting)
      - Organized into buckets depending on their "Hash Values", which allow for Fast Retrieval of individual elements based on their value.
  - Less efficient than set, in terms of Iteration of elements
  - Keys are Immutable.

	
## Unordered_Multiset:
- Features:
  - Everything same as unordered_set, except:
  - Multiple equivalent keys
    - Multiple elements in the container can have equivalent keys.
    - Multiple elements can have equivalent values.
  - Elements with equivalent values are grouped together in the same bucket and in such a way that an iterator (see equal_range) can iterate through all of them.
	
- Note:
  - this container is not defined in its own header, but shares header <unordered_set> with unordered_set.


###### Reference
  - 


