---
layout: page
title: Word Break
comments: true
---

## [Word Break](https://leetcode.com/problems/word-break/description/) -- (Medium)

### Description:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.  
  
For example, given  
s = ```"leetcode"```,  
dict = ```["leet", "code"]```.  
  
Return true because ``"leetcode"``` can be segmented as ```"leet code"```.  
  
### Solution:

    
### My Slow Code:  
Java  ~O( 26<sup>l</sup> * n )  
```java
class Solution {
    private Node root = new Node();
    
    private class Node {
        HashMap<Character, Node> letters = new HashMap<Character, Node>();
        boolean word = false;
    }
    
    public boolean wordBreak(String s, List<String> wordDict) {
        buildTrie(wordDict);
        return trieSearch(root, s.toCharArray(), 0);
    }
    
    private boolean trieSearch(Node node, char[] word, int i) {
        if (i == word.length) { // Word break complete
            if (node.word)
                return true;
            else // Last set of letters do not make word in wordDict
                return false;
        }

        char c = word[i]; // Current letter        
        if ( !node.letters.containsKey(c) ) // No such word in wordDict
            return false;
        
        Node nextNode = node.letters.get(c);
        if ( nextNode.word && trieSearch(root, word, i + 1) ) // Word Break
            return true;
        
        return trieSearch(nextNode, word, i + 1);
    }
    
    private void buildTrie(List<String> wordDict) {
        HashSet<String> uniqueWords = new HashSet<String>();
        for (String word : wordDict) {
            Node node = root;
            if (uniqueWords.contains(word))
                continue;
            else
                uniqueWords.add(word);
            for ( char c : word.toCharArray() ) {
                if ( !node.letters.containsKey(c) ) {
                    node.letters.put( c, new Node() );
                }
                node = node.letters.get(c);
            }
            node.word = true;
        }
    }
}
```

### My Slow Simple Code
Java ~O(n!)
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // HashSet dictionary for quick comparison
        HashSet<String> dict = new HashSet<String>();
        for (String word : wordDict) {
            dict.add(word);
        }
        
        return wordSearch(s, dict);
    }
    
    private static boolean wordSearch(String word, HashSet<String> dict) {
        if ( dict.contains(word) ) // Last sub word is in wordDict
            return true;
        
        for (int i = 1; i < word.length(); i++) {
            if ( dict.contains(word.substring(0, i)) && wordSearch(word.substring(i), dict) )
                return true; // Word Search Complete
        }
        return false; // No substrings make a word in wordDict
    }
}
```

### My Solution
Java O(n<sup>2</sup>)
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // HashSet dictionary for quick comparison
        int max = 0;
        int min = Integer.MAX_VALUE;
        HashSet<String> dict = new HashSet<String>();
        for (String word : wordDict) {
            dict.add(word);
            if (word.length() > max)
                max = word.length();
            if (word.length() < min)
                min = word.length();
        }
        
        // Trivial Case
        if (dict.contains(s))
            return true;
        
        // Easier Variable Names
        String word = s;
        int n = s.length();
        
        boolean[] prefixIsWords = new boolean[n];
        for (int i = min - 1; i < n; i++) {
            if ( !prefixIsWords[i] && dict.contains(word.substring(0, i + 1)) )
                prefixIsWords[i] = true;
            
            if (prefixIsWords[i]) {
                for (int j = i + min; j <= i + max + 1 && j < n; j++) {
                    if ( !prefixIsWords[j] && dict.contains(word.substring(i + 1, j + 1)) )
                        prefixIsWords[j] = true;
                    
                    if (j == n - 1 && prefixIsWords[j]) // Last sub word is in wordDict
                        return true;
                }
            }
        }
        return false; // Word cannot be broken
    }
}
```
