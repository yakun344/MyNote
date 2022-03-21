---
description: LeetCode 75
---

# Sort Colors

{% embed url="https://leetcode.com/problems/sort-colors/" %}

Given an array `nums` with `n` objects colored red, white, or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place\_algorithm) **** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Example 2:**

```
Input: nums = [2,0,1]
Output: [0,1,2]
```

**Example 3:**

```
Input: nums = [0]
Output: [0]
```

**Example 4:**

```
Input: nums = [1]
Output: [1]
```

**Constraints:**

* `n == nums.length`
* `1 <= n <= 300`
* `nums[i]` is `0`, `1`, or `2`.

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?\


{% hint style="info" %}
3指针，left指向0， mid指向1，right指向2。
{% endhint %}

```java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0, right = nums.length - 1;
        int mid = 0;
        
        while (mid <= right) {
            if (nums[mid] == 0) {
                swap(nums, mid, left);
                left++;
                mid++;
            } else if (nums[mid] == 2) {
                swap(nums, mid, right);
                right--;
            } else {
                mid++;
            }
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
