---
layout: page
title: Word Search II
comments: true
---

## [Word Search II](https://leetcode.com/problems/word-search-ii/description/) -- (Hard)

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
  
### Naive Solution:
DFS from each starting point in the board and invalidate checked letters, revalidate when recursion fails/completes. Iterate for each word in words.

### Solution:
Create a Trie from the array of words to search. DFS--searching the trie--from each starting point in the board and invalidate checked letters, also remove found words in the trie, revalidate when recursion fails/completes.

### My Naive Code:  
Java  O( 4<sup>mn</sup> * n )  
```java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        
        List<String> correctWords = new Stack<String>();
        
        // Empty - Null Cases
        if ( words == null || words.length == 0 || 
             board == null || board.length == 0 || board[0].length == 0 )
            return correctWords;
        
        // Lengths
        int m = board.length;
        int n = board[0].length;
        
        // Word Search
        HashSet<String> uniqueWords = new HashSet<String>(words.length);
        for (String word : words) {
            if (word == null || word.length() == 0)
                continue;
            int l = word.length();
            char[] wordArr = word.toCharArray();
            if ( !uniqueWords.contains(word) && boardSearch(wordArr, board, m, n) ) {
                uniqueWords.add(word);
                correctWords.add(word);
            }
        }
        
        // Found Words
        return correctWords;
    }
    
    private static boolean boardSearch(char[] word, char[][] board, int m, int n) {
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if ( word[0] == board[row][col] && wordSearch(row, col, m, n, word, board, 0) )
                    return true;
            }
        }
        return false;
    }
    
    private static boolean wordSearch(int row, int col, int m, int n, char[] word, char[][] board, int i)     {
        if (i == word.length) // Word Found
            return true;
        
        if (row < m && row >= 0 && col < n && col >= 0 && word[i] == board[row][col]) {
            board[row][col] = 0; // Invalidate Letter on Board
            boolean match = ( wordSearch(row + 1, col, m, n, word, board, i + 1) || // Right
                              wordSearch(row - 1, col, m, n, word, board, i + 1) || // Left
                              wordSearch(row, col + 1, m, n, word, board, i + 1) || // Down
                              wordSearch(row, col - 1, m, n, word, board, i + 1) ); // Up
            board[row][col] = word[i]; // Revalidate Letter on Board
            return match;
        }
        return false;
    }
}
```

### My Code:  
Java  O( 4<sup>mn</sup> * l )  
```java
class Solution {
    
    private static class Node {
        public HashMap<Character, Node> letters = new HashMap<Character, Node>();
        public String word;
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        
        List<String> correctWords = new Stack<String>();
        
        // Empty - Null Cases
        if ( words == null || words.length == 0 || 
             board == null || board.length == 0 || board[0].length == 0 )
            return correctWords;
        
        // Lengths
        int m = board.length;
        int n = board[0].length;
        
        // Build Trie
        Node root = buildTrie(words, m, n);
        
        // Word Search
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                trieSearch(row, col, m, n, board, root, correctWords);
            }
        }
        
        // Found Words
        return correctWords;
    }
    
    private static void trieSearch(int row, int col, int m, int n, char[][] board,
                                      Node node, List<String> correctWords) {     
        
        char c = board[row][col];
        if ( c == 0 || !node.letters.containsKey(c) ) // No more words match this letter on board
            return;
        
        // Check Trie and Add Words
        Node nextNode = node.letters.get(c);
        if (nextNode.word != null) { // Found a word
            correctWords.add(nextNode.word);
            removeWord(node, nextNode, c); // Remove word from trie to avoid duplicates in correctWords
        }
        
        // DFS on Board, Trie Search
        board[row][col] = 0; // Invalidate Letter on Board
        // Board Bounds Check and DFS trieSearch
        if (row < m - 1)
            trieSearch(row + 1, col, m, n, board, nextNode, correctWords); // Right
        if (row >= 1)
            trieSearch(row - 1, col, m, n, board, nextNode, correctWords); // Left
        if (col < n - 1)
            trieSearch(row, col + 1, m, n, board, nextNode, correctWords); // Up
        if (col >= 1)
            trieSearch(row, col - 1, m, n, board, nextNode, correctWords); // Down
        board[row][col] = c; // Revalidate Letter on Board   
    }
    
    private Node buildTrie(String[] words, int m, int n) {
        Node root = new Node();
        HashSet<String> uniqueWords = new HashSet<String>(words.length);
        for (String word : words) {
            if ( word == null || word.length() == 0 || word.length() > m * n ||
                 uniqueWords.contains(word) ) {
                continue;
            }
            uniqueWords.add(word);
            char[] wordArr = word.toCharArray();
            Node node = root;
            for (char c : wordArr) {
                if (!node.letters.containsKey(c)) { // Trie contains letter path
                    node.letters.put(c, new Node()); // Add new letter path
                }
                node = node.letters.get(c);
            }
            node.word = word; // Add end of word to trie
        }
        return root;
    }
    
    private static void removeWord(Node node, Node nextNode, char c) {
        nextNode.word = null;
        
        //if (nextNode.letters.isEmpty()) // If no more letters beyond this chain
            //node.letters.remove(c);
    }
}
```

### My Slightly Faster Code:  
Java  O( 4<sup>mn</sup> * l )  
```java
class Solution {
    
    private static class Node {
        public Node[] letters = new Node[26]; // Potentially 26 Letters
        public String word;
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        
        List<String> correctWords = new Stack<String>();
        
        // Empty - Null Cases
        if ( words == null || words.length == 0 || 
             board == null || board.length == 0 || board[0].length == 0 )
            return correctWords;
        
        // Lengths
        int m = board.length;
        int n = board[0].length;
        
        // Build Trie
        Node root = buildTrie(words, m, n);
        
        // Word Search
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                trieSearch(row, col, m, n, board, root, correctWords);
            }
        }
        
        // Found Words
        return correctWords;
    }
    
    private static void trieSearch(int row, int col, int m, int n, char[][] board,
                                      Node node, List<String> correctWords) {     
        
        char c = board[row][col];
        if ( c == '*' || node.letters[c - 'a'] == null ) // No more words match this letter on board
            return;
        
        // Check Trie and Add Words
        Node nextNode = node.letters[c - 'a'];
        if (nextNode.word != null) { // Found a word
            correctWords.add(nextNode.word);
            nextNode.word = null; // Remove word from trie to avoid duplicates in correctWords
        }
        
        // DFS on Board, Trie Search
        board[row][col] = '*'; // Invalidate Letter on Board
        // Board Bounds Check and DFS trieSearch
        if (row < m - 1)
            trieSearch(row + 1, col, m, n, board, nextNode, correctWords); // Right
        if (row >= 1)
            trieSearch(row - 1, col, m, n, board, nextNode, correctWords); // Left
        if (col < n - 1)
            trieSearch(row, col + 1, m, n, board, nextNode, correctWords); // Up
        if (col >= 1)
            trieSearch(row, col - 1, m, n, board, nextNode, correctWords); // Down
        board[row][col] = c; // Revalidate Letter on Board   
    }
    
    private Node buildTrie(String[] words, int m, int n) {
        Node root = new Node();
        //HashSet<String> uniqueWords = new HashSet<String>(words.length);
        for (String word : words) {
            //if ( word == null || word.length() == 0 || word.length() > m * n ||
                 //uniqueWords.contains(word) ) {
                //continue;
            //}
            //uniqueWords.add(word);
            char[] wordArr = word.toCharArray();
            Node node = root;
            for (char letter : wordArr) {
                int c = letter - 'a';
                if (node.letters[c] == null) { // Trie contains letter path
                    node.letters[c] = new Node(); // Add new letter path
                }
                node = node.letters[c];
            }
            node.word = word; // Add end of word to trie
        }
        return root;
    }
}
```
