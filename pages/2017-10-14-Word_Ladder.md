---
layout: page
title: Word Ladder
comments: true
---

## [Word Ladder](https://leetcode.com/problems/word-ladder/description/) -- (Medium)

### Description:
Given two words (*beginWord* and *endWord*), and a dictionary's word list, find the length of shortest transformation sequence from *beginWord* to *endWord*, such that:  
  
Only one letter can be changed at a time.
Each transformed word must exist in the word list. Note that *beginWord* is not a transformed word.
For example,

Given:
*beginWord* = ```"hit"```
*endWord* = ```"cog"```
wordList = ```["hot","dot","dog","lot","log","cog"]```
As one shortest transformation is ```"hit" -> "hot" -> "dot" -> "dog" -> "cog"```,
return its length ```5```.

Note:
Return 0 if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume *beginWord* and *endWord* are non-empty and are not the same.
  
### Solution:  
  
### My Slow Code (No DP)  
C++
```c++
---
layout: page
title: Word Break
comments: true
---

## [Word Ladder](https://leetcode.com/problems/word-break/description/) -- (Medium)

### Description:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.  
  
For example, given  
s = ```"leetcode"```,  
dict = ```["leet", "code"]```.  
  
Return true because ``"leetcode"``` can be segmented as ```"leet code"```.  
  
### Solution:  
  
### My Slow Code (No DP)  
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

### My Slow Simple Code (No DP)  
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

### My Java Solution  
O(n<sup>2</sup>)  
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

### My C++ Solution  
O(n<sup>2</sup>)  
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        
        // Build Dictionary
        for (string word : wordDict) {
            dict.insert(word);
            if (word.size() > max)
                max = word.size();
            if (word.size() < min)
                min = word.size();
        }
        n = s.size();
        word = s;
        int dp[n] = {0};
        suffixes = dp;
        return wordSearch(0) == 1;
    }
    
private:
    unordered_set<string> dict;
    int n;
    string word;
    int max = 0;
    int min = INT_MAX;
    int* suffixes;
    
    int wordSearch(int start) {        
        if (start == n) 
            return 1;
        
        if (suffixes[start] != 0) 
            return suffixes[start];
        
        for (int i = min - 1; i < max && i < n - start; i++) {
            if ( dict.count(word.substr(start, i + 1)) && wordSearch(start + i + 1) == 1 ) { 
                return suffixes[start] = 1;
            }
        }
        return suffixes[start] = 2;
    }
};
```

```
