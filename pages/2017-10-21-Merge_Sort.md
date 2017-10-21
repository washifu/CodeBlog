---
layout: page
title: Merge Sort
comments: true
---

## Merge Sort

### Description:
Take the array, split in half, apply Merge Sort on the left half and apply Merge Sort on the right half.
Merge the two halves by walking through each merge sorted half (which should have all elements in order) and populate a 
new temporary array. One of these halves may run out of elements, then copy the rest of the other half into the temp array.
Copy temp array into the corresponding range of the original array. At the ends of the recursion, the arrays are of only one element
which is easily sorted. Thus all the merge sorted arrays are sorted.
  
### My Code  
C++  
```c++
class Sort {
    public:
        static void mergeSort(vector<T> &nums) {
            vector<T> temp(nums.size(), 0);
            ms(nums, temp, 0, nums.size() - 1);
        }
        
    private:
        static void ms(vector<T> &nums, vector<T> &temp, 
                       int leftStart, int rightEnd) {
            if (leftStart >= rightEnd) {
                return;
            }
            int mid = (leftStart + rightEnd) / 2;
            ms(nums, temp, leftStart, mid);
            ms(nums, temp, mid + 1, rightEnd);
            mergeHalves(nums, temp, leftStart, rightEnd, mid);
        }
        
        static void mergeHalves(vector<T> &nums, vector<T> &temp,
                                int leftStart, int rightEnd, int mid) {
            int leftEnd = mid;
            int rightStart = mid + 1;
            int index = leftStart;
            int left = leftStart;
            int right = rightStart;
            while (left <= leftEnd && right <= rightEnd) {
                if (nums[left] <= nums[right]) {
                    temp[index] = nums[left];
                    left++;
                } else {
                    temp[index] = nums[right];
                    right++;
                }
                index++;
            }
            copy(nums.begin() + left, nums.begin() + leftEnd + 1, temp.begin() + index);
            copy(nums.begin() + right, nums.begin() + rightEnd + 1, temp.begin() + index);
            copy(temp.begin() + leftStart, temp.begin() + rightEnd + 1, nums.begin() + leftStart);
        }
};
```
