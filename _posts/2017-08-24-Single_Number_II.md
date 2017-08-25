---
layout: post
title: Single Number II
comments: true
---

## [Single Number II](https://leetcode.com/problems/single-number-ii/description/) -- (Medium)

### Description:
Given an array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.  
**Note:**  
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
    
### Solution:
Although quite similar in nature to Single Number, this problem requires a storing system with three states.
Therefore, a single variable with binary bits is not enough. The remedy is to use two variables, each representing a bit.
Thus a total of 4 states can be considered, however, only 3 are desired. Ideally the pattern should be 00 -> 01 -> 10 -> 00.
This can be achieved by applying some conditions along with the XOR method of storing the integers into the variables. (See Single Number I)  
  
Create two variables, called ones and twos. Iterate through the array of integers and store the integer into each variable.
Add the following conditions:  
```
1) Store the integer in ones. If twos has the integer, then clear it from ones.
2) Store the integer in twos. If ones has the integer, then clear it from twos.
```
Make sure to update ones's value before assessing twos's condition. Both variables must be updated in this order per iteration.    
  
This condition can be shortened to the following logic:
```
ones = (ones ^ num[i]) & (~twos);
twos = (twos ^ num[i]) & (~ones);
```
Should twos have the integer stored, & with the inverse will clear the integer stored in ones. Otherwise the inverse of 0 
(or whatever else remains) will not clear the specific integer in ones. Vice versa for twos.
  
This follows the state pattern of 00 -> 01 -> 10 -> 00. (Here the ones place digit is represented by the value in ones
and the twos place digit is represented by twos)  
  
Everytime an integer comes up, 
it will update ones and twos according to the states. Therefore, after three occurrences of an integer, 
ones and twos will be cleared of the data, leaving only the desired integer in the ones variable.  
  
### My Code:
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ones = 0, twos = 0;
        for(int num : nums)
        {
            ones = (ones^num) & (~twos);
            twos = (twos^num) & (~ones);
        }
        return ones;
    }
};
```
Source: [Solution by againest1's and Igzvalle and woshidaishu's comments] on LeetCode.(https://discuss.leetcode.com/topic/2031/challenge-me-thx/16?page=1)
