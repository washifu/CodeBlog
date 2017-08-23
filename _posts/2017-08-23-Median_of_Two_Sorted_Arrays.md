---
layout: post
title: Wasif's Code Blog Home
comments: true
---

## Median of Two Sorted Arrays [LeetCode]

### Description:
There are two sorted arrays **nums1** and **nums2** of size m and n respectively.
Find the median of the two sorted arrays. The overall run time complexity should be O(log(m + n)).

**Example 1:**
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3) / 2 = 2.5
```

### Solution:
The key is that the input arrays are *sorted*. Start by identifying the size of each to rule out the trivial cases.
Then determine if the total number of elements are even or odd--so at the end you can average if necessary.
