---
layout: page
title: Word Break II
comments: true
---

## [Word Break II](https://leetcode.com/problems/word-break-ii/description/) -- (Hard)

### Description:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. You may assume the dictionary does not contain duplicate words.

Return all such possible sentences.

For example, given
s = ```"catsanddog"```,
dict = ```["cat", "cats", "and", "sand", "dog"]```.

A solution is ```["cats and dog", "cat sand dog"]```.  
  
### Solution A:
Store all wordDict in HashSet (unordered_set) for easy checking. Created a Hashmap with separate chaining or a vector of vectors to store the indices of all preceeding words. Populated this starting from n + 1, and iterating back to 0 while a nested loop finds all valid immediately prior words and store indices. This DP and memoization will allow for easy construction of the sentence. Build the sentences from the last word to the first, backtrack when not possible. Before building sentence, since backtracking is DFS, check if word is breakable (see Word Break for details), otherwise the computation is a waste, traversing all deep paths.  
  
### Solution B:
Assume the whole string may be a word, if not, iterate from index 0 until a word(s) is found and recurse. Select the left (non-prior word) for target substring in recursion, while appending the found prior word to a growing sentence string which gets pushed back to the vector of sentences if the recursion reaches a point where the entire substring is the a first word of the sentence. Memoize constructed sentence part into a HashMap before terminating the recursion so future recursion of the same substring can immediately retrieve the sentence parts. This DP and memoization drastically reduces computation. This is different from Solution A in that there is a HashMap and the indices to prior words are not stored. This is at worst ~O(n<sup>2</sup>) in space complexity.
    
### My C++ Solution A
~O(n!) but severely reduce with DP for most cases
```c++
class Solution {
private:
    int n;
    int min = INT_MAX;
    int max = 0;
    string word;
    vector<vector<int>> prevWordIndices; // DP
    int* suffixes;
    unordered_set<string> dict;
    vector<string> sentences;
    
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        
        
        // Build Dictionary
        for (string word : wordDict) { ~O(d)
            dict.insert(word);
            if (word.size() > max)
                max = word.size();
            if (word.size() < min)
                min = word.size();
        }
        
        // Initializations
        n = s.size();
        word = s;
        int suf[n] = {0};
        suffixes = suf;
        vector<vector<int>> prev(n + 1, vector<int>()); // Include extra index for last word
        prevWordIndices = prev;
        
        // DP : Store index of possible previous words in DP ~O(n^2)
        for (int i = 1; i <= n; i++) { // i = 1, since 0 is the first word, include n to for last words
            for (int j = 0; j < i; j++) { // check is letters j to i make a word
                if ( dict.count(word.substr(j, i - j)) ) {
                    prevWordIndices[i].push_back(j);
                }
            }
        }    
        
        // Check if word is breakable
        if (wordBreak(0) != 1)
            return sentences;
        
        // Backtrack from last letter/word to generate all sentences
        buildSentences(n, "");        
        return sentences;
    }
    
private:
    void buildSentences(int curr, string sentence) { // ~O(n!)
        for (int prev : prevWordIndices[curr]) {
            string newSentence = sentence == "" ? word.substr(prev, curr - prev) : 
                                        word.substr(prev, curr - prev) + " " + sentence;
            
            if (prev == 0) {
                sentences.push_back(newSentence);
            } else {
                buildSentences(prev, newSentence);
            }
        }
    }
    
    int wordBreak(int start) { // ~O(n * (max - min) )
        if (start == n)
            return 1;
        if (suffixes[start] != 0)
            return suffixes[start];
        
        for (int i = min - 1; i < max && i < n - start; i++) {
            if ( dict.count(word.substr(start, i + 1)) && wordBreak(start + i + 1) == 1 )
                return suffixes[start] = 1;
        }
        return suffixes[start] = 2;
    }
};
```
