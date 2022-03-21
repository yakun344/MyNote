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

![](<../.gitbook/assets/image (30).png>)

#### Recursion(TLE)

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 0) {
            return 1;
        }
        if (n == 1) {
            return 1;
        }
        
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```

#### Memoization

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 1) {
            return 1;
        }
        
        int[] memo = new int[n + 1];
        
        search(n, memo);
        return memo[n];
    }
    
    private int search(int n, int[] memo) {
        if (n <= 1) {
            return 1;
        }
        
        if (memo[n] == 0) {
            return memo[n] = search(n - 1, memo) + search(n - 2, memo);
        }
        
        return memo[n];
    }
}
```

#### Dynamic Programming(1D Array)

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        
        for (int i = 2; i < dp.length; ++i) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }
}
```

#### Dynamic Programming(O(1) Space)

```java
class Solution {
    public int climbStairs(int n) {
        int dp1 = 1, dp2 = 1;
        int temp = 0;
        
        for (int i = 2; i < n + 1; ++i) {
            temp = dp2;
            dp2 = dp1 + dp2;
            dp1 = temp;
        }
        
        return dp2;
    }
}
```
