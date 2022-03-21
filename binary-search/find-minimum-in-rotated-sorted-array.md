---
description: LeetCode 153
---

# Find Minimum in Rotated Sorted Array

{% embed url="https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/" %}

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

* `[4,5,6,7,0,1,2]` if it was rotated `4` times.
* `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums`, return _the minimum element of this array_.

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```

**Constraints:**

* `n == nums.length`
* `1 <= n <= 5000`
* `-5000 <= nums[i] <= 5000`
* All the integers of `nums` are **unique**.
* `nums` is sorted and rotated between `1` and `n` times.

{% hint style="info" %}
三种情况

1.是一个ascending sorted array的时候，nums\[start]\<nums\[mid]\<nums\[end]

2.当target在左边，中点切的位置比首尾都小

3.当target在右边，中点切的位置比首尾都大
{% endhint %}

```java
class Solution {
    public int findMin(int[] nums) {
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < nums[end] && nums[mid] > nums[start]) {
                return nums[0];
            } else if (nums[mid] < nums[start] && nums[mid] < nums[end]) {
                end = mid;
            } else {
                start = mid;
            }
        }
        
        if (nums[start] > nums[end]) {
            return nums[end];
        } else {
            return nums[start];
        }
    }
}
```

{% hint style="info" %}
只要考虑mid和nums\[end]的大小关系就可以确定mid指针向哪儿移动
{% endhint %}

```java
class Solution {
    public int findMin(int[] nums) {
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < nums[end]) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (nums[start] > nums[end]) {
            return nums[end];
        } else {
            return nums[start];
        }
    }
}
```
