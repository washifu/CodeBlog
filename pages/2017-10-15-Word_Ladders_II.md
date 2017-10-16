---
layout: page
title: Word Ladder II
comments: true
---

## [Word Ladder II](https://leetcode.com/problems/word-ladder-ii/description/) -- (Medium)

### Description:
Given two words (**beginWord** and **endWord**), and a dictionary's word list, find all shortest transformation sequence(s) from **beginWord** to **endWord**, such that:  
  
Only one letter can be changed at a time
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
For example,

Given:
**beginWord** = ```"hit"```
**endWord** = ```"cog"```
**wordList** = ```["hot","dot","dog","lot","log","cog"]```
Return
```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```
Note:
Return an empty list if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume beginWord and endWord are non-empty and are not the same.  
  
### Solution:  
BFS to generate graph of word chains.Starting at the beginWord, get neighbors (generate all words that are off by one letter and check if exist in the dictionary), then add list of neighbors to hashmap of words. DFS through the hashmap words starting from beginWord and going down and backtracking until all paths to endWord are found. Append chain for each step and add final chains to  to generate word chains.
  
### My Code 
Java O( 26<sup>2</sup>w<sup>3</sup>)
```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        // Data Structures
        List<List<String>> chains = new ArrayList<>();        
        HashMap<String, ArrayList<String>> chainMap = new HashMap<String, ArrayList<String>>();
        ArrayList<String> link = new ArrayList<String>();
        
        // Build Dictionary
        HashSet<String> dict = new HashSet<String>(wordList);
        
        // Remove beginWord, avoid Loop
        if ( dict.contains(beginWord) ) { dict.remove(beginWord); }
        
        // Word Ladder
        bfsWordGraph(beginWord, endWord, dict, chainMap);        
        dfsWordChain(beginWord, endWord, chainMap, chains, link);
        return chains;
    }
    
    private void bfsWordGraph(String beginWord, String endWord, HashSet<String> dict, 
                              HashMap<String, ArrayList<String>> chainMap) {
        
        ArrayList<String> toVisit = new ArrayList<String>();
        toVisit.add(beginWord);
        int level = 0;
        boolean[] lastLevel = {false};
        while ( !toVisit.isEmpty() ) {
            ArrayList<String> nextToVisit = new ArrayList<String>();
            HashSet<String> nextToVisitSet = new HashSet<String>();
            level++;
            int count = toVisit.size();
            for (String curr : toVisit) {                
                chainMap.put(curr, getNeighbors(curr, dict, nextToVisitSet, lastLevel, endWord));
            }            
            if (lastLevel[0]) { return; }         
            nextToVisit.addAll(nextToVisitSet);
            toVisit = nextToVisit;
        }
    }
    
    private void dfsWordChain(String curr, String endWord, HashMap<String, ArrayList<String>> chainMap, 
                              List<List<String>> chains, ArrayList<String> link) {
        link.add(curr);
        if ( curr.equals(endWord) ) {
            chains.add(new ArrayList<String>(link));
        } else if ( chainMap.containsKey(curr) ) {
            for ( String neighbor : chainMap.get(curr) ) {
                dfsWordChain(neighbor, endWord, chainMap, chains, link);
            }
        }
        link.remove(link.size() - 1);
    }
    
    private ArrayList<String> getNeighbors(String curr, HashSet<String> dict, HashSet<String> nextToVisitSet, 
                                           boolean[] lastLevel, String endWord) {
        ArrayList<String> neighbors = new ArrayList<String>();
        char currArr[] = curr.toCharArray();
                                                   
        for (int i = 0; i < currArr.length; i++) {
            char originalLetter = currArr[i];
            for (char c = 'a'; c <= 'z'; c++) {                
                if (c == currArr[i]) { continue; }
                
                currArr[i] = c;
                String oneOff = String.valueOf(currArr);
                if ( dict.contains(oneOff) ) {
                    neighbors.add(oneOff);
                    nextToVisitSet.add(oneOff);
                    dict.remove(oneOff);
                } else if ( nextToVisitSet.contains(oneOff) ) {
                    neighbors.add(oneOff);
                }
                
                if ( oneOff.equals(endWord) ) { lastLevel[0] = true; }
            }
            currArr[i] = originalLetter;
        }
        return neighbors;
    }
}
```
