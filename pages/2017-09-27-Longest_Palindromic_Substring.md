---
layout: page
title: Longest Palindromic Substring
comments: true
---

## [Longest Palindromic Substring](https://leetcode.com/problems/palindromic-substrings/description/) -- (Medium)

### Description:
Given a string, your task is to count how many palindromic substrings in this string.  
  
The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.
      
### Solution:
~
  
### My Code:
O(n^2)
Java
```java
class Solution {
    public String longestPalindrome(String s) {
        int slen = s.length(); // Avoid Multiple Calculation of Input String Length
        if (s == null || slen == 0) return ""; // Null or Empty String
        if (slen <= 1) return s; // Unit String

        int [] max = new int[2]; // max[0] is Longest Palindrom Index, max[1] is Length
        max[1] = 0; // Set to 0 for Comparisons
        // Check All Palindromes For Longest Palindrome
        for (int i = 0; i < slen; i++) {
            extend(s, slen, i, i, max); // Odd
            extend(s, slen, i, i + 1, max); // Even
        }
        return s.substring(max[0], max[0] + max[1]);
    }

    public static void extend(String s, int slen, int left, int right, int[] max) {
        while ( left >= 0 && right < slen && s.charAt(left) == s.charAt(right) ) {
            if (right - left + 1 > max[1]) {
                max[0] = left;
                max[1] = right - left + 1;
            }
            left--;
            right++;
        }
   }
}
```

### My Optimal Code
O(n^2) - Fastest Quadratic Solution (Totally Not Worth It)
Java
```java
class Solution {
    public String longestPalindrome(String s) {
        int slen = s.length();
        if (s == null || slen == 0) return ""; // Null or Empty String
        if (slen <= 1) return s; // Unit String
        
        // Check All Palindromes Substrings
        int[] max =  new int[2]; // max[0] is the starting index, max[1] is length.
        max[1] = 0; // Set max length to zero for comparisons.
        for (int i = 0; i < slen; i++) {
            i = extend(s, slen, i, max); // Skip Rechecking Palindromes Same Central Identical Letters
        }
        return s.substring(max[0], max[0] + max[1]); // Rebuild Longest Palindrome
    }
    
    public static int extend(String s, int slen, int index, int[] max) {
        int left = index;
        int right = index;
        while (right < slen && s.charAt(left) == s.charAt(right)) { // Central Identical Letters
            right++;
        }
        right--; // Adjust for extra increment.
        int nextIndex = right; // Skip All Next Palindromes with Central Identical Letters.
        
        while (left >= 0 && right < slen && s.charAt(left) == s.charAt(right)) { // Symmetric Letters
            left--;
            right++;
        }
        left++;  // Adjust for extra increment.
        right--; // Adjust for extra increment.
        int newLength = right - left + 1; 
        if (newLength > max[1]) {
            max[1] = newLength;
            max[0] = left;
        }
        return nextIndex;
    }
}
```
