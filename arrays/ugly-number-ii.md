---
description: LeetCode 264
---

# Ugly Number II

{% embed url="https://leetcode.com/problems/ugly-number-ii/" %}



An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return _the_ `nth` _**ugly number**_.

**Example 1:**

```
Input: n = 10
Output: 12
Explanation: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.
```

**Example 2:**

```
Input: n = 1
Output: 1
Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.
```

**Constraints:**

* `1 <= n <= 1690`

{% hint style="info" %}
如果n是ugly number， 那么2 \* n, 3 \* n, 5 \* n也都是ugly number。

求第n个丑数， 用最小堆，弹出当前堆中的最小值，分别乘以2， 3， 5。如果这些数生成过，那么就不放进去堆中，如果是第一次生成，那么放到堆中。

直到n为1时，堆顶的数字就是第n个丑数。

时间复杂度O(nlog(n)),弹出每个最小值时，时间复杂度都为堆的高度，因此时间复杂度为O(nlog(n)).

空间复杂度O(n)
{% endhint %}

```java
class Solution {
    public int nthUglyNumber(int n) {
        Set<Long> set = new HashSet<>();
        PriorityQueue<Long> heap = new PriorityQueue<>();
        set.add(1L);
        heap.offer(1L);
        
        List<Long> factors = new ArrayList<>();
        factors.add(2L);
        factors.add(3L);
        factors.add(5L);
        
        while (n > 1) {
            long cur = heap.poll();
            for (long factor : factors) {
                long num = cur * factor;
                if (!set.contains(num)) {
                    set.add(num);
                    heap.offer(num);
                }
            }
            n--;
        }
        
        long res = heap.peek();
        return (int)res;
    }
}
```

{% hint style="info" %}
**动态规划：**

用一个有序数组`dp`记录前`n`个丑数。三个指针`l2`，`l3`和`l5`指向`dp`中的元素，最小的丑数只可能出现在`dp[l2]`的2倍、`dp[l3]`的3倍和`dp[l5]`的5倍三者中间。通过移动三个指针，就能保证生成的丑数是有序的。

TC : O(n)

SC : O(n)

#### 算法流程

* 初始化数组`dp`和三个指针`l2`、`l3`和`l5`。`dp[0] = 1`，表示最小的丑数为`1`。三个指针都指向`dp[0]`。
* 重复以下步骤`n`次，`dp[i]`表示第`i + 1`小的丑数：
  * 比较`2 * dp[l2], 3 * dp[l3], 5 * dp[l5]`三者大小，令`dp[i]`为其中的最小值。
  * 如果`dp[i] == 2 * dp[l2]`，`l2`指针后移一位。
  * 如果`dp[i] == 3 * dp[l3]`，`l3`指针后移一位。
  * 如果`dp[i] == 2 * dp[l5]`，`l5`指针后移一位。
* `dp[n - 1]`即为第`n`小的丑数，返回。
{% endhint %}

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        
        int l2 = 0, l3 = 0, l5 = 0;
        for (int i = 1; i < n; ++i) {
            dp[i] = Math.min(Math.min(2 * dp[l2], 3 * dp[l3]), 5 * dp[l5]);
            if (dp[i] == 2 * dp[l2]) {
                l2++;
            }
            if (dp[i] == 3 * dp[l3]) {
                l3++;
            }
            if (dp[i] == 5 * dp[l5]) {
                l5++;
            }
        }
        
        return dp[n - 1];
    }
}
```
