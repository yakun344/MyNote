---
description: LintCode 625
---

# Partition Array II

{% embed url="https://www.lintcode.com/problem/625/" %}



Description

Partition an unsorted integer array into three parts:

1. The front part < _low_
2. The middle part >= _low_ & <= _high_
3. The tail part > _high_

Return any of the possible solutions.

low <= high in all testcases.Example

Example 1:

```
Input:
[4,3,4,1,2,3,1,2]
2
3
Output:
[1,1,2,3,2,3,4,4]
Explanation:
[1,1,2,2,3,3,4,4] is also a correct answer, but [1,2,1,2,3,3,4,4] is not
```

Example 2:

```
Input:
[3,2,1]
2
3
Output:
[1,2,3]
```

Challenge

1. Do it in place.
2. Do it in one pass (one loop).

{% hint style="info" %}
三指针做法，left指向最接近low的点，right指向最接近high的点。

遍历当前mid，mid从0开始。

如果nums\[mid] > high，交换mid 和right，right--

如果nums\[mid] < low，交换mid和left，left++, mid++

如果low <= nums\[mid] <= high，mid++。
{% endhint %}

```java
public class Solution {
    /**
     * @param nums: an integer array
     * @param low: An integer
     * @param high: An integer
     * @return: nothing
     */
    public void partition2(int[] nums, int low, int high) {
        // write your code here
        int left = 0, right = nums.length - 1;
        int mid = 0;
        while (mid <= right) {
            if (nums[mid] > high) {
                swap(nums, mid, right);
                right--;
            } else if (nums[mid] < low) {
                swap(nums, mid, left);
                left++;
                mid++;
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
