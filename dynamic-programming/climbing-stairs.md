---
description: LeetCode 70
---

# Climbing Stairs

{% embed url="https://leetcode.com/problems/climbing-stairs/" %}

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

**Constraints:**

* `1 <= n <= 45`

#### Dynamic Programming

{% hint style="info" %}
考虑最后一步走1阶还是走2阶。 方案数Dp\[n]= 最后一步走1阶的方案数 + 最后一步走2阶的方案数。dp\[n] = dp\[n - 1] + dp\[n - 2]。

初始值dp\[0] = 1, dp\[1] = 1;

这个可以从dp\[3]，也就是第三个台阶的所有可能反推。第三个台阶有\[1,1,1] \[1, 2] \[2, 1]三种情况，所以dp\[2]只能是2，dp\[0]和dp\[1]都是1。

时间复杂度为 $$O(n)$$
{% endhint %}

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 1) {
            return n;
        }
        
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i < n + 1; ++i) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

**Recursion with Memoization**

```java
public class Solution {
    public int climbStairs(int n) {
        int memo[] = new int[n + 1];
        return climb_Stairs(0, n, memo);
    }
    public int climb_Stairs(int i, int n, int memo[]) {
        if (i > n) {
            return 0;
        }
        if (i == n) {
            return 1;
        }
        if (memo[i] > 0) {
            return memo[i];
        }
        memo[i] = climb_Stairs(i + 1, n, memo) + climb_Stairs(i + 2, n, memo);
        return memo[i];
    }
}
```

**Fibonacci Formula**

![](<../.gitbook/assets/image (17).png>)
