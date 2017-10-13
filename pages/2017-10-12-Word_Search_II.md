---
layout: page
title: Word Search II
comments: true
---

## [Word Search II]() -- (Hard)

### Description:
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

For example,
Given **words** = ```["oath","pea","eat","rain"]``` and **board** =
```
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
```
Return ```["eat","oath"]```.
**Note:**
You may assume that all inputs are consist of lowercase letters ```a-z```.
  
### Solution:
DFS from each starting point in the word matrix and invalidate checked letters, revalidate when recursion fails. Iterate for each word in words.
    
### My Code:  
Java  O( 4<sup>mn</sup> * n )  
```java
```
