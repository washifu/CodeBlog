---
layout: page
title: Word Search
comments: true
---

## [Word Search](https://leetcode.com/problems/word-search/description/) -- (Medium)

### Description:
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given **board** =
```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```
**word** = ```"ABCCED"```, -> returns ```true```,  
**word** = ```"SEE"```, -> returns ```true```,  
**word** = ```"ABCB"```, -> returns ```false```.
  
### Solution:
DFS from each starting point in the word matrix and invalidate checked letters, revalidate when recursion fails.
    
### My Code:  
Java
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        // Empty Set - Null Case
        if (word == null || word.length() == 0 ||
            board == null || board.length == 0 || board[0].length == 0)
            return false;
        
        // Lengths
        int wordLen = word.length();
        int rowLen = board.length;
        int colLen = board[0].length;
        char[] wordArr = word.toCharArray();
        
        // Word longer than all letters in board
        if (wordLen > rowLen * colLen) 
            return false;
        
        // Word Search
        for (int row = 0; row < rowLen; row++)
            for (int col = 0; col < colLen; col++)
                if ( wordArr[0] == board[row][col] && // Check each letter and DFS
                     wordSearch(row, col, wordArr, board, rowLen, colLen, 0) )
                    return true;
        
        return false;
    }
    
    private static boolean wordSearch(int row, int col, char[] word, char[][] board, 
                                      int rowLen, int colLen, int i) {
        if (i == word.length)
            return true;
        if ( row < rowLen && row >= 0 && col < colLen && col >= 0 &&
             board[row][col] == word[i] ) {
            board[row][col] = 0; // Invalidate Checked Letter
            boolean match = ( wordSearch(row, col + 1, word, board, rowLen, colLen, i + 1) || // Right
                              wordSearch(row, col - 1, word, board, rowLen, colLen, i + 1) || // Left
                              wordSearch(row + 1, col, word, board, rowLen, colLen, i + 1) || // Down
                              wordSearch(row - 1, col, word, board, rowLen, colLen, i + 1) ); // Up
            board[row][col] = word[i]; // Revalidated Checked Letter Since DFS Completed
            return match;
        }
        return false; // Letter Does Not Match or Invalid Position
    }
}
```
