---
description: LeetCode 50
---

# Pow(x, n)

{% embed url="https://leetcode.com/problems/powx-n/" %}

Implement [pow(_x_, _n_)](http://www.cplusplus.com/reference/valarray/pow/), which calculates _x_ raised to the power _n_ (i.e. xn).

**Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

**Constraints:**

* `-100.0 < x < 100.0`
* `-231 <= n <= 231-1`
* `-104 <= xn <= 104`

{% hint style="info" %}
注意 n 可能是负数, 需要求一下倒数, 可以在一开始把x转换成倒数, 也可以到最后再把结果转换为倒数.

那么现在我们实现 pow(x, n), n 是正整数的版本:

二分即可: x ^ n = x ^ n / 2 ∗ x ^ n / 2因此可以在 O(_logn_) 的时间复杂度内实现.
{% endhint %}

```java
class Solution {
    public double myPow(double x, int n) {
        boolean isNegative = false;
        if (n < 0) {
            x = 1 / x;
            isNegative = true;
            n = -(n + 1);              // Avoid overflow when n == MIN_VALUE
        }
        
        double ans = 1, temp = x;
        
        while (n != 0) {
            if (n % 2 == 1) {
                ans *= temp;
            }
            temp *= temp;
            n /= 2;
        }
        
        if (isNegative) {
            ans *= x;
        }
        
        return ans;
    }
}
```
