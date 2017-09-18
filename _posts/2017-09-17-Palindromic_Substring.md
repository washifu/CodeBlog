---
layout: post
title: Palindromic Substring
comments: true
---

## [Palindromic Substring](https://leetcode.com/problems/palindromic-substrings/description/) -- (Medium)

### Description:
Given a string, your task is to count how many palindromic substrings in this string.  
  
The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.
      
### Solution:
~
  
### My Code:
The n^3 version
Java
```java
    static class Solution {
        public int countSubstrings(String s) {
            int count = 0;
            
            int slen = s.length();
            for (int i = 0; i < slen - 1; i++) { // first loop
                for (int j = slen - 1; j > i; j--) { // second loop
                    if ( checkPalindrome(s, i, j) ) // recursion
                        count++;
                        // break;
                }
            }
            return count;
        }
        // Instead of boolean, return int
        public boolean checkPalindrome(String s, int i, int j) {
            if (j <= i)
                return true;
            if ( s.charAt(i) == s.charAt(j) )
                return checkPalindrome(s, i + 1, j - 1);
            else
                return false;
        }
    }
```
