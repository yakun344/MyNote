---
description: LintCode 140
---

# Fast Power

{% embed url="https://www.lintcode.com/problem/140/" %}

#### Description

Calculate the $$a^n$$ % b where a, b and n are all 32bit non-negative integers.Have you met this question in a real interview?  YesProblem Correction

#### Example

For 2 ^ 31 % 3 = 2

For 100^1000 % 1000 = 0

#### Challenge

O(logn)

```java
public class Solution {
    /**
     * @param a: A 32bit integer
     * @param b: A 32bit integer
     * @param n: A 32bit integer
     * @return: An integer
     */
    public int fastPower(int a, int b, int n) {
        // write your code here
        if (n == 1) {
            return a % b;
        }
        
        if (n == 0) {
            return 1 % b;
        }
        
        long product = fastPower(a, b, n / 2);
        product = (product * product) % b;
        if (n % 2 == 1) {
            product = (product * a) % b;
        }
        return (int)product;
    }
}
```
