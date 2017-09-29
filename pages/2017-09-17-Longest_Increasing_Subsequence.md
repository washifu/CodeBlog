---
layout: page
title: Longest Increasing Subsequence
comments: true
---

## [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/) -- (Medium)

### Description:
Given an unsorted array of integers, find the length of longest increasing subsequence.  
  
For example,  
Given [10, 9, 2, 5, 3, 7, 101, 18],  
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.
  
Your algorithm should run in O(n2) complexity.  
  
Follow up: Could you improve it to O(n log n) time complexity?  
      
### Solution:
~
  
### My Code:
The n^2 solution.
Java
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 0) // Validation
            return 0;
        int[] count = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            count[i] = 1;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] > nums[j]) {
                    if (count[j] >= count[i])
                        count[i] = count[j] + 1;
                }
            }
        }

        Arrays.sort(count);
        return count[nums.length - 1];
    }
}
```

### My Code:
The nlog(n) solution.
Java
 ```java
 
 ```
