---
layout: page
title: Number of Connected Components in an Undirected Graph
comments: true
---

## [Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/description/) -- (Medium)

### Description:
Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), 
write a function to find the number of connected components in an undirected graph.  
  
**Example 1:**
```
     0          3
     |          |
     1 --- 2    4
```
Given `n = 5` and `edges = [[0, 1], [1, 2], [3, 4]]`, return `2`.  
  
**Example 2:**
```
     0           4
     |           |
     1 --- 2 --- 3
```
Given `n = 5` and `edges = [[0, 1], [1, 2], [2, 3], [3, 4]]`, return `1`.  
  
**Note:**
You can assume that no duplicate edges will appear in `edges`. 
Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in `edges`.  
  
### Solution:
~
  
### My Code:
Java
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] set = new int[n]; // Keeps track of root
        for (int i = 0; i < n; i++) // Set all elements as their own root
            set[i] = i;
        
        for (int[] edge : edges) // Union All Edges
            union(set, edge[0], edge[1]);
        
        int components = 0; // Count Components
        for (int i = 0; i < n; i++) {
            if (set[i] == i)
                components++;       
        }
        return components;
    }

    // Find root of node/element
    public static int findRoot(int j, int[] set) {
        if (set[j] == j) // Is itself root
            return j;
        set[j] = findRoot(set[j], set); // Path Compression
        return set[j]; // Return the root
    }
    
    // Union Two Sets
    public static void union(int[] set, int i, int j) {
        int root1 = findRoot(i, set); 
        int root2 = findRoot(j, set);
        if (root1 == root2) // Shares root
            return;
        
        set[root1] = root2; // Set root of set 1 to point to root 2
        return;
    }
}
```

### My Slightly Faster Code:
Java
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] set = new int[n]; // Keeps track of root
        for (int i = 0; i < n; i++) // Set all elements as their own root
            set[i] = i;
        
        for (int[] edge : edges) { // Union All Edges
            int start = edge[0];
            int end = edge[1];
            
            while (set[start] != start)
                start = set[start];
            
            while (set[end] != end)
                end = set[end];
            
            if (start == end)
                continue;

            set[end] = start;
        }
        
        for (int i = 0; i < n; i++) { // Count Components
            if (set[i] == i)
                components++;       
        }
        return components;
    }
}
```
