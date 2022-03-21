---
description: LeetCode 493
---

# Reverse Pairs

{% embed url="https://leetcode.com/problems/reverse-pairs/" %}



Given an integer array `nums`, return _the number of **reverse pairs** in the array_.

A reverse pair is a pair `(i, j)` where `0 <= i < j < nums.length` and `nums[i] > 2 * nums[j]`.

**Example 1:**

```
Input: nums = [1,3,2,3,1]
Output: 2
```

**Example 2:**

```
Input: nums = [2,4,3,5,1]
Output: 3
```

**Constraints:**

* `1 <= nums.length <= 5 * 104`
* `-231 <= nums[i] <= 231 - 1`

```java
class Solution {
    public int reversePairs(int[] nums) {
        int[] temp = new int[nums.length];
        return mergeSort(nums, temp, 0, nums.length - 1);
    }
    
    private int mergeSort(int[] nums, int[] temp, int start, int end) {
        if (start >= end) {
            return 0;
        }
        int left = start, mid = (start + end) / 2, right = mid + 1;
        int sum = 0;
        sum += mergeSort(nums, temp, start, mid);
        sum += mergeSort(nums, temp, right, end);
        
        for (int i = start, j = right; i <= mid; ++i) {
            while (j <= end && nums[i] / 2.0 > nums[j]) ++j;
            sum += j - mid - 1;
        }
        
        merge(nums, temp, start, end);
        return sum;
    }
    
    private void merge(int[] nums, int[] temp, int start, int end) {
        if (start >= end) {
            return;
        }
        
        int left = start, mid = (start + end) / 2, right = mid + 1;
        int index = start;
        
        while (left <= mid && right <= end) {
            if (nums[left] <= nums[right]) {
                temp[index++] = nums[left++];
            } else {
                temp[index++] = nums[right++];
            }
        }
        
        while (left <= mid) {
            temp[index++] = nums[left++];
        }
        
        while (right <= end) {
            temp[index++] = nums[right++];
        }
        
        for (int i = start; i <= end; ++i) {
            nums[i] = temp[i];
        }
    }
}
```
