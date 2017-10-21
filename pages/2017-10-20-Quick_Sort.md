---
layout: page
title: Quick Sort
comments: true
---

## Quick Sort

### Description:
Identify a pivot value, then set a left pointer at the start of the array and a right pointer at the end of the array.
Partition the array such that all the values less the pivot value is to the left and all the values more are to the right
and return the pivot index after swapping the value with the pivot value. 
Then recursively call Quick Sort on the subarray to the left and the subarray to the right of the pivot index, 
not including the pivot index. The pivot index is actually in its final position. If a call to Quick Sort has a left pointer
that is equal to or greater than the right pointer, simply return. This happens when the array is of length 1 or is an array
out of bounds.  
  
### My Code  
C++  
```c++
class QuickSort {
    public:
        static void quickSort(vector<int>::iterator left, vector<int>::iterator right) {
            if (left >= right) { 
                 return; 
                
            }
            vector<int>::iterator pivot = right;
            vector<int>::iterator partIndex = partition(left, right - 1, pivot);
            quickSort(left, partIndex - 1);
            quickSort(partIndex + 1, right);
        }
        
    private:
        static vector<int>::iterator partition(vector<int>::iterator left, vector<int>::iterator right, 
                                               vector<int>::iterator pivot) {
            while (left < right) {
                while (*left < *pivot) { 
                    left++; 
                }
                while (*right > *pivot && right > left) { 
                    right--; 
                }
                if (left < right) { 
                    swapVal(left, right);
                    left++;
                    right--;
                }
            }
            swapVal(left, pivot);
            return left;
        }
        
        static void swapVal(vector<int>::iterator it1, vector<int>::iterator it2) {
            int temp = *it1;
            *it1 = *it2;
            *it2 = temp;
        }
};
```
