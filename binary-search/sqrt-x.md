---
description: LeetCode 69
---

# Sqrt(x)

{% embed url="https://leetcode.com/problems/sqrtx/" %}

Given a non-negative integer `x`, compute and return _the square root of_ `x`.

Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Example 1:**

```
Input: x = 4
Output: 2
```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

**Constraints:**

* `0 <= x <= 231 - 1`

```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0) {
            return 0;
        } else if (x < 0) {
            return -1;
        }
        
        int start = 0, end = x;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (x / mid == mid) {
                return mid;
            } else if (x / mid < mid) {
                end = mid;
            } else {
                start = mid;
            }
        }
        
        if (x / end == end) {
            return end;
        } else {
            return start;
        }
    }
}
```
