---
layout: page
title: Word Break
comments: true
---

## [Word Break](https://leetcode.com/problems/word-break/description/) -- (Medium)

### Description:
You are a professional robber planning to rob houses along a street. 
Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that 
adjacent houses have security system connected and it will automatically contact the police if two adjacent houses 
were broken into on the same night.  
  
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.  
   
### Solution:  
For each house, there are only two options: rob: ```r```: or not rob or ```o```.
The amount of money in each house is given by the ```int``` array ```nums[]```.
Use memoization to keep track of total money for both scenarios. 
For the current house, if you rob, your total money will be equal to the last ```o``` + nums[i].
On the other hand, if you do not rob, your total money will be equal to nothing + whichever was greater: the last ```r``` or the last ```o```.
Repeat for each house while updating ```r``` and ```o``` to equal the total of each round if you choose rob or not rob. 
Finally, after the final house, ```max(r,o)``` the total if you rob or the total if you do not rob, whichever is greater is the answer.

  
### My Code 
Java  O(n)  
```java
class Solution {
    public int rob(int[] nums) {
        int steal = 0;
        int notSteal = 0;
        for (int i = 0; i < nums.length;  i++) {
            int prevSteal = steal;
            steal = notSteal + nums[i];
            notSteal = Math.max(prevSteal, notSteal);
        }
        return Math.max(steal, notSteal);
    }
}
```
