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
      
### O(n<sup>2</sup>) Solution:
Consider the subproblem of longest increasing subsequence for the sub array containing only the first element. The LIS length is 1. Incrase this array to include the next element. No matter what, you may assume the LIS length for the sub array of size 2 to be 1, but if the second element is bigger than the previous, the LIS length is 2. Continue to incrase this sub array as you visit the remaining elements and keep track of the LIS for each larger subarray. These sub arrays are indicative of the LIS up to that specific index element. For a sub array of size 5, for any element prior, if it is smaller and has an LIS length larger than 1, the 5th LIS length is at least that length + 1. In this way, memoize the LIS of the subarrays until you build the whole array. If an prior element is smaller and has a LIS length equal to or greater than the current LIS length, update the LIS length.

### O( nlog(n) ) Solution:
As you iterate through the array, build the longest increasing subsequence by checking if the next element is larger than the largest element, if so, append it to the LIS array. If the element is smaller, replace the first element in the LIS array that is larger than this new element. In this way, you don't add to the count, but if another element shows up that is slightly larger (but smaller than the replaced element) it may also be inserted (by replacing or appending) and maintain the optimal LIS array. This way effectively generates the LIS array dynamically. However, the searching for the next largest element is at best a binary search which takes log(n) time. Thus, O( nlog(n) ).
  
### My Code:
The n<sup>2</sup> solution.  
Java  
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 0) // Validation
            return 0;
        int[] lisLength = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            lisLength[i] = 1;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] > nums[j]) {
                    if (lisLength[j] >= lisLength[i])
                        lisLength[i] = lisLength[j] + 1;
                }
            }
        }

        Arrays.sort(lisLength);
        return lisLength[nums.length - 1];
    }
}
```  
C++  
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = nums.size();
        if (len <= 1) { return len; }
        vector<int> lisLength;
        for (int i = 0; i < len; i++) {
            lisLength.push_back(1);
            for (int j = i - 1; j >= 0; j--) {
                if (nums[j] < nums[i] && lisLength[j] >= lisLength[i]) {
                    lisLength[i] = lisLength[j] + 1;
                }
            }
        }
        sort(lisLength.begin(), lisLength.end());
        return lisLength[len - 1];
    }
};
```  
  
### My Code:
The nlog(n) solution  
Java  
<pre>
 ```java
 class Solution {
    public int lengthOfLIS(int[] nums) {
        // Empty Set or Null Case
        if (nums == null || nums.length == 0)
            return 0;
        
        // Construct Ordered List of LIS
        ArrayList<Integer> ordered = new ArrayList<Integer>(nums.length);
        for (int i = 0; i < nums.length; i++) {
            // Ordered list is empty
            if (ordered.size() == 0)
                ordered.add(nums[i]);
            // Number is greater, append to ordered list
            else if ( nums[i] > ordered.get(ordered.size() - 1) )
                ordered.add(nums[i]);
            // Number is equal to greatest number in ordered list, do nothing
            else if ( nums[i] == ordered.get(ordered.size() - 1) )
                continue;
            // Number is less than greatest number is ordered list, 
            // find smallest number in list greater than number and replace
            // This will identify following small numbers that are in increasing order
            else
                ordered.set( -Collections.binarySearch(ordered, nums[i]) - 1, nums[i]) ;
        }
        return ordered.size();
    }
}
```
</pre>
  
C++  
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = nums.size();
        if (len <= 1) { return len; }
        vector<int> lisArr;
        lisArr.push_back(nums[0]);
        for (int i = 1; i < len; i++) {
            if (nums[i] > *(lisArr.end() - 1) ) {
                lisArr.push_back(nums[i]);
            } else {
                if ( !binary_search(lisArr.begin(), lisArr.end(), nums[i]) ) {
                    *upper_bound(lisArr.begin(), lisArr.end(), nums[i]) = nums[i];
                }
            }
        }
        return lisArr.size();
    }
};
```
