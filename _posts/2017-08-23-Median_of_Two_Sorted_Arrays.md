---
layout: post
title: Median of Two Sorted Arrays
comments: true
---

## Median of Two Sorted Arrays [LeetCode] -- (Hard)

### Description:
There are two sorted arrays **nums1** and **nums2** of size m and n respectively.
Find the median of the two sorted arrays. The overall run time complexity should be O(log(m + n)).

**Example 1:**
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3) / 2 = 2.5
```

### Solution:
The key is that the input arrays are *sorted* and to optimize you want to use binary search.  
  
Start by identifying the size of each array to rule out the trivial cases and identify the smaller and larger arrays.  
Then determine whether the total number of elements are even or odd--so at the end you can average if necessary. Throw in a couple quick checks for when one array is totally greater or less than another array for a quick route to the solution.

Okay, for the non-trivial case, first consider the O(n) solution.  

### O(n):
Realize that the median divides the total set of numbers such that half of them are less than the median and half are more. Also, each array may be divided into two parts--for simplicity let's say left (smaller) and right (larger) parts.  
```
      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

We must simply ensure two conditions to acquire the true median:  
  
1) The total number of elements in the two left parts must equal the total number of elements in the two right parts.
  
2) The largest element in the left part of the first array must be smaller than the smallest element in the right part of the second array. Also, the largest element in the left part of the second array must be smaller than the smallest element in the right part of the first array.  
  
Although the two arrays are sorted, there are several ways to cut the two arrays such that the number of elements are equal but there remain elements in the left parts that are larger than elements in the right parts or there remain elements in the right parts that are smaller than elements in the left parts.    
  
### Detailed Explanation:
The second condition seems like a mouthful, so let me try to elucidate it a bit. Assume the two arrays are **A** and **B** and have been labelled such that the length of **A** is smaller than or equal to **B**. Also, the length of array **A** is **n**, and length of array **B** is **m**. Array **A** may be cut at some index **i** and array **B** at some index **j**. The relationship between **i** and **j** is simply that **i** + **j** must equal (**m** - **i**) + (**n** - **j**) (or (**m** - **i**) + (**n** - **j**) + 1). This satisifies the first condition.  
  
The second condition requires that all the elements in the left halves are less than all the elements in the right halves. To check for this, take advantage of the fact that the arrays are sorted. Thus, all the elements in **A**'s left half is obviously less than all the elements in **A**'s right half. If the largest element in **A**'s left half, **A[i-1]**, is less than the smallest element in **B**'s right half, **B[j]**, then certainly, all the elements in **A**'s left half are smaller than all the elements in **B**'s right half. Symmetrically, if the largest element in **B**'s left half, **B[j-1]**, is smaller than the smallest element in **A**'s right half, **A[i]**, then certainly, all the elements in **B**'s left half are smaller than all the elements in **A**'s right half.  
  
More succintly, **A[i-1]** < **B[j]** and **B[j-1]** < **A[i]** satisfies the second condition.  
  
So basically, i and j must simply be adjusted to keep the left halves total and right halves total equal and the **A[i-1]** < **B[j]** and **B[j-1]** < **A[i]** must be checked to find the true median.  
  
This can be done by setting **i** to 0 and iterating up to **i** equals **m**, all the while reducing **j** accordingly and checking for the second condition. If at anypoint the second condition is met, the true median is found.  
  
Since the linear time solution iterates through **A**, only the **B[j-1]** < **A[i]** condition must be checked. **i** is checked from the 0 and upwards, so **A[i-1]** < **B[j]** need not be checked, since all smaller **i**'s will have been checked. Also, care must be taken to address the boundary conditions where **i = 0**, **j = 0**, **i = m**, and **j = n**, because at these cases **A[i-1]**, **B[j-1]**, **A[i]**, and **B[j]** do not exist.  
  
### My O(n) Code:
Java
```
class Solution {
    private double average(int a, int b) 
    {
        return (a + b) / 2.0;
    }
    
    public double findMedianSortedArrays(int[] nums1, int[] nums2) 
    {
        int m = nums1.length;
        int n = nums2.length;
        if (m > n) // Ensure nums1 is the smaller array.
            return findMedianSortedArrays(nums2, nums1);
        
        // Relabel the Arrays
        int[] A = nums1; // smaller
        int[] B = nums2; // bigger
        
        // Odd or Even Total Elements
        boolean even = true;
        if ( ((m + n) % 2) != 0 )
            even = false;
        
        // Trivial Cases
        if ( (m | n) == 0 ) // Both Arrays Empty
            return 0.0;
        if (n == 0) // B is Empty
        {
            int m_mid = m / 2;  // Integer Division!
            if (even)           // Even Number of Elements
                return average(A[m_mid - 1], A[m_mid]);
            else                // Odd Number of Elements
                return A[m_mid];
        }
        if (m == 0) // A is Empty
        {
            int n_mid = n / 2;  // Integer Division!
            if (even)           // Even Number of Elements
                return average(B[n_mid - 1], B[n_mid]);
            else                // Odd Number of Elements
                return B[n_mid];
        }
    
        // Non-Trivial Case
        int mid = (m + n + 1) / 2; // Integer Division!          
        
        // Boundary Cases
        int i = 0;
        int j = mid - i;      
        if (B[j - 1] > A[i])
        { }
        else
        {
            int left_parts_max = B[j - 1]; // A[i - 1] Does Not Exist
            if (even)
            {
                int right_parts_min;
                if (j != n)
                    right_parts_min = Math.min(A[i], B[j]);
                else
                    right_parts_min = A[i]; // B[j] Does Not Exist
                return average(left_parts_max, right_parts_min);
            }
            else
                return left_parts_max;
        }
        i = m;
        j = mid - i;
        if (A[i - 1] > B[j])
        { }
        else
        {
            int left_parts_max;
            if (j != 0)
                left_parts_max = Math.max(A[i - 1], B[j - 1]);
            else
                left_parts_max = A[i - 1]; // B[j - i] Does Not Exist
            if (even)
            {
                int right_parts_min = B[j]; // A[i] Does Not Exist
                return average(left_parts_max, right_parts_min);
            }
            else
                return left_parts_max;
        }
        
        // Main Iteration
        i = 1;
        while (i < m)
        {
            j = mid - i;
            if (B[j - 1] > A[i])
                i++;
            //else if (A[i - 1] > B[j])
                //i--;
            else
            {
                int left_parts_max = Math.max(A[i - 1], B[j - 1]);
                if (!even)
                    return left_parts_max;
                
                int right_parts_min = Math.min(A[i], B[j]);
                return average(left_parts_max, right_parts_min);    
            }
        }
        
        return 404; // This should never occur
    }
}
```  
  
### O( log(m + n) ):
The strategy is the same, with the exception of optimizing the iteration through **i** instead with a binary search. Furthermore binary search inherently stars in the middle and goes left or right, so both **A[i-1]** < **B[j]** and **B[j-1]** < **A[i]** must be checked and the search range must be adjusted accordingly to find the true median. Also, by starting in the middle the boundary cases may be checked last.  
  
### My O( log(m + n) ) Code:
Java
```
class Solution {
    private double average(int a, int b) 
    {
        return (a + b) / 2.0;
    }
    
    public double findMedianSortedArrays(int[] nums1, int[] nums2) 
    {
        int m = nums1.length;
        int n = nums2.length;
        if (m > n) // Ensure nums1 is the smaller array.
            return findMedianSortedArrays(nums2, nums1);
        
        // Relabel the Arrays
        int[] A = nums1; // smaller
        int[] B = nums2; // bigger
        
        // Odd or Even Total Elements
        boolean even = true;
        if ( ((m + n) % 2) != 0 )
            even = false;
        
        // Trivial Cases
        if ( (m | n) == 0 ) // Both Arrays Empty
            return 0.0;
        if (n == 0) // B is Empty
        {
            int m_mid = m / 2;  // Integer Division!
            if (even)           // Even Number of Elements
                return average(A[m_mid - 1], A[m_mid]);
            else                // Odd Number of Elements
                return A[m_mid];
        }
        if (m == 0) // A is Empty
        {
            int n_mid = n / 2;  // Integer Division!
            if (even)           // Even Number of Elements
                return average(B[n_mid - 1], B[n_mid]);
            else                // Odd Number of Elements
                return B[n_mid];
        }
    
        // Non-Trivial Case
        int mid = (m + n + 1) / 2; // Integer Division!          
        int i = m / 2;
        int imax = m;
        int imin = 0;
        while (imin <= imax) // Binary Search
        {
            i = (imin + imax) / 2;
            int j = mid - i;
            if ( i < m && B[j - 1] > A[i] ) // i is too small, i must increase
                imin = i + 1; // look in larger i's
            else if ( i > 0 && A[i - 1] > B[j] ) // i is too large, i must decrease
                imax = i - 1; // look in smaller i's
            else
            {
                // Boundary and Even Element Cases
                int left_parts_max;
                if (i == 0)
                    left_parts_max = B[j - 1]; // A[i - 1] Does Not Exist
                else if (j == 0)
                    left_parts_max = A[i - 1]; // B[j - 1] Does Not Exist
                else
                    left_parts_max = Math.max(A[i - 1], B[j - 1]);

                if (!even)
                    return left_parts_max;
                
                int right_parts_min;
                if (i == m)
                    right_parts_min = B[j];
                else if (j == n)
                    right_parts_min = A[i];
                else
                    right_parts_min = Math.min(A[i], B[j]);
                return average(left_parts_max, right_parts_min);
            }
        }
        
        return 404; // This should never occur
    }
}
```
  
Source: [Solution and explanation by MissMary](https://discuss.leetcode.com/topic/4996/share-my-o-log-min-m-n-solution-with-explanation) on LeetCode.
