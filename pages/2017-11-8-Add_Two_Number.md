---
layout: page
title: Add Two Numbers
comments: true
---

## [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/) -- (Medium)

### Description:
You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.  
  
You may assume the two numbers do not contain any leading zero, except the number 0 itself.  
  
**Input:** (2 -> 4 -> 3) + (5 -> 6 -> 4)
**Output:** 7 -> 0 -> 8  
  
### Solution:  
Since the linked lists are stored in reverse order, the problem is quite simple. Just do addition like you would by hand in grade school. Set a variable for carrying over if the sum is greater than 10, so you can add that to the next place digit. Don't forget to add another digit for the carried value at the end if it is not zero.  
  
### My Code:  
  
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *res = new ListNode(0);
        ListNode *start = res;
        int carry = 0;
        
        if ( !(l1 && l2) )
            return NULL;
        if (!l1)
            return l2;
        if (!l2)
            return l1;
        
        while (l1 != NULL || l2 != NULL) {
            int sum = 0;
            if (l1 != NULL) {
                sum += l1->val;
                l1 = l1->next;
            }
            if (l2 != NULL) {
                sum += l2->val;
                l2 = l2->next;
            }
            sum += carry;
            carry = sum / 10;
            res->next = new ListNode(sum % 10);
            res = res->next;
        }
        if (carry) {
            res->next = new ListNode(carry);
        }
        return start->next;
    }
};
```
