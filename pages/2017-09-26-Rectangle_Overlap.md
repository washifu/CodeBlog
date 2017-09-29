---
layout: post
title: Rectangle Area
comments: true
---

## [Rectangle Area](https://leetcode.com/problems/rectangle-area/description/) -- (Medium)

### Description:
Find the total area covered by two **rectilinear** rectangles in a **2D** plane.  
  
Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

![Diagram][exampleDiagram]

[exampleDiagram] : https://github.com/washifu/codeblog/blob/master/_images/rectangle_area.png?raw=true

Assume that the total area is never beyond the maximum possible value of int.  
  
### Solution:
~
    
### My Code:  
Java
```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        
        /*
        // No Overlap Conditions
        if (C <= A || D <= B) // Rectangle ABCD has 0 area.
            return false;
        if (G <= E || H <= F) // Rectangle DEFG has 0 area.
            return false;
        if (E >= C || A >= G) // Rectangles are totally to the left/right of each other.
            return false;
        if (F >= D || B >= H) // Rectangles are totally above/below each other.
            return false;
        // Overlap
        return true;
        */
        
        // Area of Rectangles 1 and 2
        int area1 = (C - A) * (D - B); // Area of Rectangle 1
        int area2 = (G - E) * (H - F); // Area of Rectangle 2
        int overlap = 0;
        
        // Overlapping Rectangle
        int left = Math.max(A, E);
        int right = Math.min(C, G);
        int bottom = Math.max(B, F);
        int top = Math.min(D, H);
        if (left < right && bottom < top)
            overlap = (right - left) * (top - bottom);
        
        return area1 + area2 - overlap; // Total Area (Not Double Counting Overlap)
    }
}
```
