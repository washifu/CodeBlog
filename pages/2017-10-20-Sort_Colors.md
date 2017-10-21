---
layout: page
title: Sort Colors
comments: true
---

## [Sort Colors](https://leetcode.com/problems/sort-colors/description/) -- (Medium)

### Description:
Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, 
with the colors in the order red, white and blue.  
  
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.  
  
Note:  
You are not suppose to use the library's sort function for this problem.  
  
Follow up:  
A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, 
then 1's and followed by 2's.  
  
Could you come up with an one-pass algorithm using only constant space?  
   
## Solution  
Set a pointer/iterator on the first element, ```red``` and the last element ```blue```. Iterate through the array and if value is 0, 
swap with ```red``` and increment ```red```. If the value equals 2, swap with ```blue``` and decrement both ```blue``` and ```i```,
so that you check the swapped value at ```i``` again in the next iteration (since this value is ahead of the loop and has not
been checked. Terminate the loop when ```i``` is greater than or equal to ```blue```, because all remaining elements must be 2.  
  
### My Code  
C++  O(n)  
```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        vector<int>::iterator red = nums.begin();
        vector<int>::iterator blue = nums.end() - 1;
        for (vector<int>::iterator i = nums.begin(); i <= blue; i++) {
            if (*i == 0) { swapVal(i, red++); }
            else if (*i == 2) { swapVal(i--, blue--); }
        }
    }

private:
    void swapVal(vector<int>::iterator it1, vector<int>::iterator it2) {
        int temp = *it1;
        *it1 = *it2;
        *it2 = temp;
    }
}; 
```  
