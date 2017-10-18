---
layout: page
title: House Robber II
comments: true
---

## [House Robber II](https://leetcode.com/problems/house-robber-ii/description/) -- (Medium)

### Description:
You are a professional robber planning to rob houses along a street. 
Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that 
adjacent houses have security system connected and it will automatically contact the police if two adjacent houses 
were broken into on the same night.  
  
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.  
   
### Solution:  
Break the circle by assuming do not rob first house, remainder of problem is now the linear house case of [House Robber](https://washifu.github.io/codeblog/pages/2017-10-16-House_Robber/). Since there are only two possible situations for each house, next assume rob first house, thus second house is not robbed, reducing the remainder of the problem to the linear house case of [House Robber](https://washifu.github.io/codeblog/pages/2017-10-16-House_Robber/). Choose the best of these two solutions. If a house is not robbed, it guarantees the [House Robber](https://washifu.github.io/codeblog/pages/2017-10-16-House_Robber/) solution since the next immediate house, all following houses, and last house may or may not be robbed as if the house were just in a straight line.
  
### My Code 
Java  O(n)  
```java
class Solution {
    public int rob(int[] nums) {
        // No Houses
        if (nums == null || nums.length == 0)
            return 0;
        // One House
        if (nums.length == 1)
            return nums[0];
        // Two Houses
        if (nums.length == 2)
            return Math.max(nums[0], nums[1]);
        
        //---   Rob First   ---//
        // r0 = 0
        // o0 = 0
        // r1 = nums[0]                     <-- Must Rob First
        // o1 not possible
        // r2 not possible
        // o2 = Math.max(r1, o1) = nums[0]  <-- Must Not Rob Second
        int r = nums[0];
        int o = nums[0];
        for (int i = 2; i < nums.length; i++) { // Start at third
            int prevr = r;
            r = o + nums[i];
            o = Math.max(prevr, o);
        }
        int robFirst = o; // Cannot rob last house
        //int robLast = r - nums[0]; // Rob Last not First
        
        //---   Do Not Rob First   ---///
        // r0 = 0
        // o0 = 0
        // r1 not possible
        // o1 = 0;
        // r2 = nums[1] + o1        = nums[1]
        // o2 = Math.max(o1, r1)    = 0
        r = nums[1];
        o = 0;
        for (int i = 2; i < nums.length; i++) {
            int prevr = r;
            r = o + nums[i];
            o = Math.max(prevr, o);
        }
        int doNotRobFirst = Math.max(r, o); // May or May not Rob Last

        return Math.max(robFirst, doNotRobFirst);
    }
}
```

### My Code
Java ~O(n)
```java
class Solution {
    public int rob(int[] nums) {
        // No Houses
        if (nums == null || nums.length == 0)
            return 0;
        // One House
        if (nums.length == 1)
            return nums[0];
        
        // Do Not Rob First House
        int doNotRobFirst = robLineOfHouses(nums, 0);
        
        // Rob First House <==> Do Not Rob Second House
        int robFirst = robLineOfHouses(nums, 1);
        
        return Math.max(doNotRobFirst, robFirst);
    }
    
    private static int robLineOfHouses(int[] nums, int doNotRob) {
        int r = 0;
        int o = 0;
        for (int i = doNotRob + 1; i < nums.length; i++) {
            int prevr = r;
            r = nums[i] + o;
            o = Math.max(prevr, o);
        }
        for (int i = 0; i < doNotRob; i++) {
            int prevr = r;
            r = nums[i] + o;
            o = Math.max(prevr, o);
        }
        return Math.max(r, o);
    }
}
```
