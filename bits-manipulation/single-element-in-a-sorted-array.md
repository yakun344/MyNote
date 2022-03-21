---
description: LeetCode 540
---

# Single Element in a Sorted Array

{% embed url="https://leetcode.com/problems/single-element-in-a-sorted-array/" %}

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

**Example 1:**

```
Input: [1,1,2,3,3,4,4,8,8]
Output: 2
```

**Example 2:**

```
Input: [3,3,7,7,10,11,11]
Output: 10
```

**Note:** Your solution should run in O(log n) time and O(1) space.

#### XOR Approach

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int ret = 0;
        for (int i = 0; i < nums.length; ++i) {
            ret ^= nums[i];
        }
        return ret;
    }
}
```

#### Binary Search Approach

{% hint style="info" %}
让mid落在偶数index上，判断mid和它的下一位mid + 1的大小

1,1,2,2,3,3..如果相等，说明前面没有单独出现的数字，start越过mid这一对相等数字。

0,1,1,2,2,3..如果不等，说明前面有单独出现的数字。
{% endhint %}

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int start = 0, end = nums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (mid % 2 == 1) {
                mid--;
            }
            if (nums[mid] == nums[mid + 1]) {
                start = mid + 2;
            } else {
                end = mid;
            }
        }
        
        return nums[start];
    }
}
```

