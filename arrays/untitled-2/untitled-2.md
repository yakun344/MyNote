---
description: LeetCode 283
---

# Move Zeroes

{% embed url="https://leetcode.com/problems/move-zeroes/" %}

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

{% hint style="info" %}
left指向从左起最后一位非0的数字，right指向右边第一位非0的数字之后，++left， 这时候left就指向第一个0，然后和right交换。然后right向右移动。

目标：遍历数组的过程中保证left和right中间都是0。
{% endhint %}

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if (nums.length <= 1) {
            return;
        }
        
        int left = -1, right = 0;
        while (right < nums.length) {
            if (nums[right] != 0) {
                swap(nums, ++left, right);
            }
            right++;
        }
        return;
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

{% hint style="info" %}
写的操作最小。right碰到0的时候继续往前走，当right指针不是0的时候，当前指向的数字赋值给left，然后移动left和right。最后如果left没走到尾，那么把left - end这一段全部赋值为0.
{% endhint %}

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int left = 0, right = 0;
        while (right < nums.length) {
            if (nums[right] != 0) {
                nums[left] = nums[right];
                left++;
            }
            right++;
        }
        
        while (left < nums.length) {
            nums[left] = 0;
            left++;
        }
        return;
    }
}
```
