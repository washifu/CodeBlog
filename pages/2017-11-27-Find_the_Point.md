---
layout: page
title: Find the Point
comments: true
---

## [Find the Point](https://www.hackerrank.com/challenges/find-point/problem) -- (Easy)

### Description:
Consider two points,  and . We consider the inversion or point reflection, , of point  across point  to be a  rotation of point  around .

Given  sets of points  and , find  for each pair of points and print two space-separated integers denoting the respective values of  and  on a new line.

Input Format

The first line contains an integer, , denoting the number of sets of points. 
Each of the  subsequent lines contains four space-separated integers describing the respective values of , , , and  defining points  and .

Constraints

Output Format

For each pair of points  and , print the corresponding respective values of  and  as two space-separated integers on a new line.

Sample Input

2
0 0 1 1
1 1 2 2
Sample Output

2 2
3 3
Explanation

The graphs below depict points , , and  for the  points given as Sample Input:

find-point-0011.png
Thus, we print  and  as 2 2 on a new line.
find-point-1122.png
Thus, we print  and  as 3 3 on a new line.
  
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
