---
description: LeetCode 231
---

# Power of Two

{% embed url="https://leetcode.com/problems/power-of-two/" %}

Given an integer `n`, return _`true` if it is a power of two. Otherwise, return `false`_.

An integer `n` is a power of two, if there exists an integer `x` such that `n == 2x`.

**Example 1:**

```
Input: n = 1
Output: true
Explanation: 2^0 = 1
```

**Example 2:**

```
Input: n = 16
Output: true
Explanation: 2^4 = 16
```

**Example 3:**

```
Input: n = 3
Output: false
```

**Example 4:**

```
Input: n = 4
Output: true
```

**Example 5:**

```
Input: n = 5
Output: false
```

**Constraints:**

* `-231 <= n <= 231 - 1`

{% hint style="info" %}
2的n次幂在二进制表示中，只有1位是1，所以用x & (x - 1)消除最后一位1，如果结果为0，那么它就是2的n次幂。

Time : O(1)
{% endhint %}

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n == 0) {
            return false;
        }
        
        long x = (long) n;
        return (x & (-x)) == x;
    }
}
```
