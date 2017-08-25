---
layout: post
title: Single Number
comments: true
---

## Single Number [LeetCode] -- Easy

### Description:
Given an array of integers, every element appears *twice* except for one. Find that single one.  
**Note:**  
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
    
### Solution:
Since every element appears twice except for the target element, a simple bit manipulation technique can be used to solve this. 
Also, avoid using extra memory simply means solving the problem with fixed extra memory consumption 
rather than increasing consumption of memory as the input array increases in size. Basically, O(1) space efficiency.  
  
The trick is to use the XOR operator.  
  
Create a variable. Iterate through the array of integers and XOR with the variable.
XOR essentially flips the bits to effectively "store" the integer into the variable.
If the same integer comes around, it will effectively "delete" or cancel out the previously stored integer.
In this way, all sets of double integers will be stored and deleted, leaving only the target integer.  
  
### Code
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {

        int ns = nums.size();
        if (ns == 0)
            return 0;
        int ans = nums[0];
        for (int i = 1; i < ns; i++)
            ans ^= nums[i];
        return ans;

    }
};
```
