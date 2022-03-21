---
description: LeetCode 42
---

# Trapping Rain Water

{% embed url="https://leetcode.com/problems/trapping-rain-water" %}

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2:**

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

&#x20;

**Constraints:**

* `n == height.length`
* `1 <= n <= 2 * 104`
* `0 <= height[i] <= 105`

```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int res = 0;
        int leftMax = 0, rightMax = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    res += leftMax - height[left];
                }
                left++;
            } else {
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    res += rightMax - height[right];
                }
                right--;
            }
        }
        
        return res;
    }
}
```
