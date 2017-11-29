---
layout: page
title: Arrays Left Rotation
comments: true
---

## [Arrays: Left Rotation](https://www.hackerrank.com/challenges/ctci-array-left-rotation/problem) -- (Easy)  
  
### Description:  
A left rotation operation on an array of size  shifts each of the array's elements  unit to the left. 
For example, if left rotations are performed on array , then the array would become .  
  
Given an array of  integers and a number, , perform  left rotations on the array. 
Then print the updated array as a single line of space-separated integers.  
  
Input Format  
  
The first line contains two space-separated integers denoting the respective values of  (the number of integers) 
and  (the number of left rotations you must perform).  
The second line contains  space-separated integers describing the respective elements of the array's initial state.  
  
Constraints  
  
Output Format  
  
Print a single line of  space-separated integers denoting the final state of the array after performing  left rotations.  
  
Sample Input  
```
5 4
1 2 3 4 5
```  
Sample Output  
```
5 1 2 3 4
```  
  
Explanation  
  
When we perform  left rotations, the array undergoes the following sequence of changes:  
  
Thus, we print the array's final state as a single line of space-separated values, which is `5 1 2 3 4`.  
  
### Solution:  
~
  
### My Code:  
  
```c++
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>

#include <string>
#include <sstream>
#include <iterator>

using namespace std;


int main() {
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    string line;
    int problems;
    getline(cin, line);
    if (line == "") { return -1; } // No STDIN
    stringstream(line) >> problems;
    if (!problems) { return 0; } // No points
    for (int i = 0; i < problems; i++) {
        int px, py, qx, qy;
        getline(cin, line); // Get a line and store entirity in a string
        istringstream iss(line); // Create an istringstream object which is parsed by space (" ") from input string.
        
        /*
        vector<string> points((istream_iterator<string>(iss)),
                              istream_iterator<string>());
        stringstream(points[0]) >> px;
        stringstream(points[1]) >> py;
        stringstream(points[2]) >> qx;
        stringstream(points[3]) >> qy;
        */
        
        iss >> px;
        iss >> py;
        iss >> qx;
        iss >> qy;
        int rx = 2 * qx - px; // qx - (px - qx)
        int ry = 2 * qy - py; // qy - (py - qy)
        cout << rx << " " << ry << endl;
    }
    return 0;
}
```
