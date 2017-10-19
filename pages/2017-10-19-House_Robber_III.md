---
layout: page
title: House Robber III
comments: true
---

## [House Robber III](https://leetcode.com/problems/house-robber-iii/description/) -- (Medium)

### Description:
The thief has found himself a new place for his thievery again. There is only one entrance to this area, 
called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart 
thief realized that "all houses in this place forms a binary tree". It will automatically contact the police 
if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

Example 1:
```
    [3]
    / \
   2   3
    \   \ 
    [3] [1]
```
Maximum amount of money the thief can rob = ```3``` + ```3```+ ```1``` = **7**.
Example 2:
```
     3
    / \
  [4] [5]
  / \   \ 
 1   3   1
```
Maximum amount of money the thief can rob = ```4``` + ```5``` = **9**.
   
### Naive Solution:
Determine max profit from a node. Only two options **r: rob** or **o: do not rob**. So, the max of **r** and **o**
is the max profit from a node. The max profic if the robber does not rob is simply the max profit of the 
children, ```node->left``` and ```node->right```. (Which can be acquired through recursion).
The robber may rob only if the children, ```node->left``` and ```node->right``` were not robbed.
To get this information, we must get the max profit from the grandchildren. The max profit of ```node->left->left``` and
```node->left->right``` is the max profit of the **left child**, ```node->left```, if it were not robbed.
Similarly, the max profit ```node->right->left``` and ```node->right->right``` is the max profit of the **right child**,
```node->right```, if it were not robbed. Lastly, or rather firstly, the termination condition of the recursion is when
the node does not exist. So if the ```node``` is NULL, the max profit is simply 0.  
  
Essentially, the max profit from a node is the max of its not robbed value and its robbed value. Not robbed value is attained by
the max profit of the left and right children. Robbed value is attained by the current node's value and the not robbed values 
of the left and right children (since these houses are directly linked to the current node, they must not be robbed), which equal
the max profit of the grandchildren.  
  
### DP Solution
The naive solution may be memoized. Use an unordered_map/HashMap and memoize the max profit for all visited nodes.

### Faster Greedy Solution
Each time the max profit is computed, the information of robbed and not robbed is lost, since the max is taken. Therefore,
there are repeated subproblems. For example, computing the not robbed value, requires looking up the not robbed values of 
the grandchildren (which had previously been looked up for computing the robbed value of the children).  
  
A greedy solution--very much like the solution to  
[House Robber I](https://washifu.github.io/codeblog/pages/2017-10-16-House_Robber/) and
[House Robber II](https://washifu.github.io/codeblog/pages/2017-10-16-House_Robber_II/) simply depends on the "previous" houses
or the linked houses, in this case the left and right children. The **o** of the current node is just the max of the 
**r** and **o** of the children. The **r** of the current node is current node value plus the **o** of the children. In order
to do this greedy algorithm, simply returning the max profit of each node is not enough (as this is where the information 
of **r** and **o** are lost, yielding the repeated subproblems). Each call to the recursion function should instead return both
the **r** and **o** so that greedily the parent nodes may be computed without repeating calls (to grandchildren).  
  
### Given
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 ```  
  
### My Naive Code  
C++ ~O(2<sup>n</sup>)  
```c++
class Solution {
public:
    int rob(TreeNode* root) {
        if (root == NULL) { return 0; }
        int r = root->val;
        if (root->left != NULL) {
            r += rob(root->left->left) + rob(root->left->right);
        }
        if (root->right != NULL) {
            r += rob(root->right->left) + rob(root->right->right);
        } 
        int o = rob(root->left) + rob(root->right);
        return max(r,o);
    }
};
```  
  
### My DP Code  
C++  ~O(2n)  
```c++
class Solution {
public:
    int rob(TreeNode* root) {
        unordered_map<TreeNode*, int> visited;
        return robTree(root, visited);
    }

private:
    int robTree(TreeNode* node, unordered_map<TreeNode*, int> &visited) {
        if (node == NULL) { return 0; }
        
        if ( visited.count(node) ) {
            return visited.at(node);
        }
        
        int o = robTree(node->left, visited) + robTree(node->right, visited);
        int r = node->val;
        if (node->left != NULL) {
            r += robTree(node->left->left, visited) + robTree(node->left->right, visited);
        }
        if (node->right != NULL) {
            r += robTree(node->right->left, visited) + robTree(node->right->right, visited);
        }
        
        int ro = max(r,o);
        visited.insert(make_pair(node, ro));
        return ro;
    }
};
```  
  
### My Greedy Code  
C++ ~O(n)  
```c++
class Solution {
public:
    int rob(TreeNode* root) {
        pair<int, int> ro = robGreedy(root);
        return max(ro.first, ro.second);
    }
    
private:
    pair<int, int> robGreedy(TreeNode* node) {
        if (node == NULL) {
            return make_pair(0,0);
        }        
        pair<int, int> roLeft = robGreedy(node->left);
        pair<int, int> roRight = robGreedy(node->right);
        int r = node->val + roLeft.second + roRight.second;
        int o = max(roLeft.first, roLeft.second) + max(roRight.first, roRight.second);
        return make_pair(r,o);
    }
};
```
