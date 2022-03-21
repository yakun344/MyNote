---
description: LeetCode 1771
---

# Maximize Palindrome Length From Subsequences

{% embed url="https://leetcode.com/problems/maximize-palindrome-length-from-subsequences/" %}



You are given two strings, `word1` and `word2`. You want to construct a string in the following manner:

* Choose some **non-empty** subsequence `subsequence1` from `word1`.
* Choose some **non-empty** subsequence `subsequence2` from `word2`.
* Concatenate the subsequences: `subsequence1 + subsequence2`, to make the string.

Return _the **length** of the longest **palindrome** that can be constructed in the described manner._ If no palindromes can be constructed, return `0`.

A **subsequence** of a string `s` is a string that can be made by deleting some (possibly none) characters from `s` without changing the order of the remaining characters.

A **palindrome** is a string that reads the same forward as well as backward.

**Example 1:**

```
Input: word1 = "cacb", word2 = "cbba"
Output: 5
Explanation: Choose "ab" from word1 and "cba" from word2 to make "abcba", which is a palindrome.
```

**Example 2:**

```
Input: word1 = "ab", word2 = "ab"
Output: 3
Explanation: Choose "ab" from word1 and "a" from word2 to make "aba", which is a palindrome.
```

**Example 3:**

```
Input: word1 = "aa", word2 = "bb"
Output: 0
Explanation: You cannot construct a palindrome from the described method, so return 0.
```

**Constraints:**

* `1 <= word1.length, word2.length <= 1000`
* `word1` and `word2` consist of lowercase English letters.

{% hint style="info" %}
经典题目[Longest Palindromic Subsequence](longest-palindromic-subsequence.md)的变形题。

这个问题可以拆分为两个子问题

1.在word1 + word2的string中找到最大长度的subsequences。

2.这个找到的subsequences必须要分别在word1和word2中至少出一个字母。

Post一下[花花酱](https://www.youtube.com/watch?v=0dbo8C64UFk)的讲义。
{% endhint %}

![](<../.gitbook/assets/image (26).png>)

![](<../.gitbook/assets/image (28).png>)

```java
class Solution {
    public int longestPalindrome(String word1, String word2) {
        int l1 = word1.length();
        int l2 = word2.length();
        int n = l1 + l2;
        
        int[][] dp = new int[n][n];
        String str = word1 + word2;
        for (int i = 0; i < n; ++i) {
            dp[i][i] = 1;
        }
        
        for (int l = 2; l <= n; ++l) {
            for (int i = 0, j = i + l - 1; j < n; ++i, ++j) {
                if (str.charAt(i) == str.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        
        int res = 0;
        for (int i = 0; i < l1; ++i) {
            for (int j = 0; j < l2; ++j) {
                if (word1.charAt(i) == word2.charAt(j)) {
                    res = Math.max(res, dp[i][l1 + j]);
                }
            }
        }
        
        return res;
    }
}
```
