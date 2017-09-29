---
layout: post
title: Friend Circle
comments: true
---

## [Friend Circle](https://leetcode.com/problems/friend-circles/description/) -- (Medium)

### Description:
There are N students in a class. Some of them are friends, while some are not. 
Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, 
then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.  
  
Given a N*N matrix M representing the friend relationship between students in the class. 
If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. 
And you have to output the total number of friend circles among all the students.  
  
**Example 1:**
```
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```
**Example 2:**
```
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```
**Note:**
1. N is in range [1,200].
2. M[i][i] = 1 for all students.
3. If M[i][j] = 1, then M[j][i] = 1.

### Solution:
~
  
### My Code:
Java
```java
class Solution {
    public int findCircleNum(int[][] M) {       
        int circles = 0; // Number of Friend Circles
        boolean[] visitedPeople = new boolean[M.length]; // List of Checked People
        for (int i = 0; i < M.length; i++) { // Check All People
            if ( !visitedPeople[i] ) { // If not visited, check friends
                visitedPeople[i] = true; // Now visited!
                findFriends(i, visitedPeople, M); // Find all this person's friends and friends' friends
                circles++; // This is a new friend circle
            }
        }
        
        return circles;
    }
    
    /**
     * Find Friends and Friends' Friends, etc
     * Updates visitedPeople
     */
    public static void findFriends(int i, boolean[] visitedPeople, int[][] M) {
        for (int j = 0; j < M[i].length; j++){
            if ( !visitedPeople[j] && M[i][j] == 1 ) { // If never visited and is a friend
                visitedPeople[j] = true; // Now visited!
                findFriends(j, visitedPeople, M); // Find all this person's friends and friends' friends
            }
        }
        // All of i's friends (and indirect friends) have been check
    }
}
```
