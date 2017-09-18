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
class Solution {
  public int countSubstrings(String s) {
    if (s == null) // Validation
        return 0;
    int count = 0;
    int slen = s.length();
    for (int i = 0; i < slen; i++) {
      count += extend(s, i, i); // Odd
      count += extend(s, i, i + 1); // Even
    }
    return count;
  }
  
  public int extend(String s, int left, int right) {
    int slen = s.length();
    int pcount = 0;
    while ( left >= 0 && right < slen && s.charAt(left) == s.charAt(right) ) {
      pcount++;
      left--;
      right++;
    }
    return pcount;
  }
}
```
