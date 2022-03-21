---
description: LeetCode 33
---

# Search in Rotated Sorted Array

{% embed url="https://leetcode.com/problems/search-in-rotated-sorted-array/" %}

You are given an integer array `nums` sorted in ascending order (with **distinct** values), and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

_If `target` is found in the array return its index, otherwise, return `-1`._

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

**Constraints:**

* `1 <= nums.length <= 5000`
* `-104 <= nums[i] <= 104`
* All values of `nums` are **unique**.
* `nums` is guaranteed to be rotated at some pivot.
* `-104 <= target <= 104`

![](<../.gitbook/assets/image (8).png>)

{% hint style="info" %}
mid切在左边的话，target所在的位置有两种情况，一种在mid左边（这时候把end指针左移到mid），一种在mid右边（这个时候把start指针右移到mid）；

mid切在右边到话，target所在到位置也有两种情况，一种在mid左边（end = mid），一种在mid右边（start = mid）

**注意：target如果存在的话，一定会移动到start或end指向的点。**
{% endhint %}

```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null && nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            
            if (nums[start] < nums[mid]) {
                if (nums[start] <= target && target <= nums[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else {
                if (nums[mid] <= target && target <= nums[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        }
        
        if (nums[start] == target) {
            return start;
        } else if (nums[end] == target) {
            return end;
        } else {
            return -1;
        }
    }
}
```
