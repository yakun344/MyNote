---
description: LeetCode 44 LintCode 192
---

# Wildcard Matching

{% embed url="https://leetcode.com/problems/wildcard-matching/" %}

{% embed url="https://www.lintcode.com/problem/wildcard-matching/description" %}

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the **entire** input string (not partial).

**Note:**

* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```

**Example 3:**

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

#### Memoization Search:

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if (s == null || p == null) {
            return false;
        }
        
        //visited记录已经访问过的点，memo记录已经访问过点的状态；
        boolean[][] visited = new boolean[s.length()][p.length()];
        boolean[][] memo = new boolean[s.length()][p.length()];
        
        return matchHelper(s, 0, p, 0, visited, memo);
    }
    
    //s从s_Index开始匹配p从p_Index开始的字符串，结果记录在visited和memo中；
    private boolean matchHelper(String s, int s_Index, String p, int p_Index, boolean[][] visited, boolean[][] memo) {
        
        //如果s在s_Index是空的话，p从p_Index之后一定要是*;
        if (s_Index == s.length()) {
            for (int i = p_Index; i < p.length(); ++i) {
                if (p.charAt(i) != '*') {
                    return false;
                }
            }
            return true;
        }
        
        //如果p在p_Index走到头，s在s_Index也一定要走到头
        if (p_Index == p.length()) {
            return s_Index == s.length();
        }
        
        //判断当前的s_Index和p_Index有没有被访问过，访问过直接返回memo的状态
        if (visited[s_Index][p_Index]) {
            return memo[s_Index][p_Index];
        }
        
        boolean match;
        
        if (p.charAt(p_Index) == '*') {
            //'*'匹配一个s中的一个字符
            match = matchHelper(s, s_Index + 1, p, p_Index, visited, memo) ||
                //‘*’没有匹配s中的字符
                matchHelper(s, s_Index, p, p_Index + 1, visited, memo);
        } else {
            //不是'*‘的时候check字符是不是一样，或者pattern是不是'?',依赖后面的匹配
            match = checkMatch(s.charAt(s_Index), p.charAt(p_Index)) &&
                matchHelper(s, s_Index + 1, p, p_Index + 1, visited, memo);
        }
        
        //记录状态到两个二维数组里
        visited[s_Index][p_Index] = true;
        memo[s_Index][p_Index] = match;
        
        return match;
    }
    
    private boolean checkMatch(char p, char q) {
        return p == q || q == '?';
    }
}
```
