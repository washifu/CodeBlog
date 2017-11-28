---
layout: page
title: Cut the Sticks
comments: true
---

## [Cut the Sticks](https://www.hackerrank.com/challenges/cut-the-sticks/problem) -- (Easy)

### Description:
You are given  sticks, where the length of each stick is a positive integer. A cut operation is performed on the sticks such that all of them are reduced by the length of the smallest stick.

Suppose we have six sticks of the following lengths:

5 4 4 2 2 8
Then, in one cut operation we make a cut of length 2 from each of the six sticks. For the next cut operation four sticks are left (of non-zero length), whose lengths are the following: 

3 2 2 6
The above step is repeated until no sticks are left.

Given the length of  sticks, print the number of sticks that are left before each subsequent cut operations.

Note: For each cut operation, you have to recalcuate the length of smallest sticks (excluding zero-length sticks).

### Solution:  
~
  
### My Code:  
  
```c++
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <sstream>
#include <limits>
using namespace std;


int main() {
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */ 
    string input;
    getline(cin, input);
    int n = stoi(input);
    if (n == 0) { return 0; } // No Sticks
    getline(cin, input);
    stringstream line(input);
    vector<int> sticks;
    // Generate Sticks
    for (int i = 0; i < n; i++) {
        int stick;
        line >> stick;
        sticks.push_back(stick);
    }
    // Sort Sticks
    sort(sticks.begin(), sticks.end());
    reverse(sticks.begin(), sticks.end());
    // Cut the Sticks
    int sticksCut = 0;
    do {
        int smallest = sticks[sticks.size() - 1];
        sticksCut = 0;
        for (int i = sticks.size() - 1; i >= 0; --i) {
            if (sticks[i] ==  smallest) {
                sticksCut++;
                sticks.pop_back();
                continue;
            }
            sticks[i] = sticks[i] - smallest;
            sticksCut++;
        }
        if (sticksCut) {
            cout << sticksCut << endl;
        }
    } while (sticksCut > 0);
    
    return 0;
}
```
