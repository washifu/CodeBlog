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
BFS from *beginWord* until *endWord* is found. For massive dictionaries, realize that the only possible next word are words where only one letter off. For small dictionaries, you can just check for one letter off words, but at most there will be 26 * w (the number of letters in the words) possible next words. So, just check all permutations of the current word in dictionary to generate words in next frontier/toVisit for each word in the current frontier.
  
### My Code 
C++ O( 26<sup>2</sup>w<sup>3</sup>)
```c++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // Build Dictionary
        unordered_set<string> dict;
        for (string word : wordList)
            dict.insert(word);

        // endWord not in dict - No chain to end
        if ( !dict.count(endWord) )
            return 0;
        
        // Initializations
        if ( dict.count(beginWord) ) {
            dict.erase(beginWord);
        }
        
        // Word Ladder
        vector<string> toVisit;
        toVisit.push_back(beginWord);
        int level = 1;
        while(!toVisit.empty()) {
            for (string curr : toVisit) {
                if (curr.compare(endWord) == 0) { // Found shortest chain
                    return level;
                }
            }
            vector<string> nextToVisit;
            addNextWords(toVisit, nextToVisit, dict);
            toVisit = nextToVisit;
            level++;
        }
        return 0;
    }

private:
    void addNextWords(vector<string> &toVisit, vector<string> &nextToVisit, unordered_set<string> &dict) {
        for (string curr : toVisit) {
            for (int i = 0; i < curr.size(); i++) {
                char letter = curr[i];
                for (int c = 0; c < 26; c++) {
                    curr[i] = 'a' + c;
                    if (dict.count(curr)) {
                        nextToVisit.push_back(curr);
                        dict.erase(curr);
                    }
                }
                curr[i] = letter;
            }
        }
    }
};
```

### My Code
Java (Same Algorithm)
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // Build Dictionary
        HashSet<String> dict = new HashSet<String>();
        for (String word : wordList) 
            dict.add(word);
        
        // endWord in dict ?
        if ( !dict.contains(endWord) )
            return chains;
        
        // Initializations
        ArrayList<String> toVisit = new ArrayList<String>();
        toVisit.add(beginWord);
        int level = 1;
            
        while ( !toVisit.isEmpty() ) {
            for (String curr : toVisit) {
                if ( curr.compareTo(endWord) == 0 ) {
                    return level; // Shortest chain to endWord
                }
            }
            ArrayList<String> nextToVisit = new ArrayList<String>();
            addNewWords(toVisit, nextToVisit, dict);
            toVisit = nextToVisit;
            level++;
        }
        
        return 0;
    }
    
    private void addNewWords(ArrayList<String> toVisit, ArrayList<String> nextToVisit, HashSet<String> dict) {
        for (String word : toVisit) {
            for (int i = 0; i < word.length(); i++) {  
                for (int n = 0; n < 26; n++) {
                    char c = (char) ('a' + n);
                    String oneOffWord = new StringBuilder(word.substring(0, i)).append(c).
                                                          append(word.substring(i + 1)).toString();
                    if ( dict.contains(oneOffWord) ) {
                        nextToVisit.add(oneOffWord);
                        dict.remove(oneOffWord);
                    }
                }
            }
        }
    }
}
```
