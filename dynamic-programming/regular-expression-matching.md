---
description: LeetCode 10 LintCode 154
---

# Regular Expression Matching

{% embed url="https://leetcode.com/problems/regular-expression-matching/" %}

{% embed url="https://www.lintcode.com/problem/regular-expression-matching/description" %}

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire**input string (not partial).

**Note:**

* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

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
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

**Example 5:**

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

#### Memorization Approach

```java
class Solution {
    public boolean isMatch(String s, String p) {
        //判断s和p为空的情况
        if (s == null || p == null) {
            return false;
        }
        
        //记录遍历过点的状态
        boolean[][] visited = new boolean[s.length()][p.length()];
        boolean[][] memo = new boolean[s.length()][p.length()];
        
        //从零开始搜索所有匹配的情况，返回当前遍历index的状态
        return isMatchHelper(s, 0, p, 0, visited, memo);
    }
    //时间 O(nm), 空间 O(nm)
    private boolean isMatchHelper(String s, int s_Index, String p, int p_Index, boolean[][] visited, boolean[][] memo) {
        //source走到头，判断pattern是不是a*a*a*...的情况
        if (s_Index == s.length()) {
            return pIsEmpty(p, p_Index);
        }
        
        //pattern走到头的情况，判断source是不是走到头
        if (p_Index == p.length()) {
            return s_Index == s.length();
        }
        
        //当前的点有没有被访问过
        if (visited[s_Index][p_Index]) {
            return memo[s_Index][p_Index];
        }
        
        //记录当前状态
        boolean match;
        
        //当p + 1位置是'*'时，匹配1个当前/不匹配当前位置字符
        //⚠️p_Index边界
        if (p_Index + 1 < p.length() && p.charAt(p_Index + 1) == '*') {
            match = isEqualTo(s.charAt(s_Index), p.charAt(p_Index)) &&
                isMatchHelper(s, s_Index + 1, p, p_Index, visited, memo) ||
                isMatchHelper(s, s_Index, p, p_Index + 2, visited, memo);
            //不是*时，判断字符是否相同。
        } else {
            match = isEqualTo(s.charAt(s_Index), p.charAt(p_Index)) &&
                isMatchHelper(s, s_Index + 1, p, p_Index + 1, visited, memo);
        }
        
        visited[s_Index][p_Index] = true;
        memo[s_Index][p_Index] = match;
        
        return match;
    }
    
    private boolean pIsEmpty(String p, int p_Index) {
        for (int i = p_Index; i < p.length(); i += 2) {    //形如"x*x*"形式
            if (i + 1 >= p.length() || p.charAt(i + 1) != '*') {
                //如果不是'*'，无法匹配
                return false;
            }
        }
        return true;
    }
    
    private boolean isEqualTo(char p, char q) {
        return p == q || q == '.';
    }
}
```

```java
class Solution {
      public boolean isMatch(String s, String p) {
          //base case
          if (s.length() == 0 && p.length() == 0) return true;
          else if (p.length() == 0) return false;
          
          boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
          dp[0][0] = true;
          // fill in dp[0][j]
          for (int j = 2; j <= p.length(); ++j) {
              dp[0][j] = dp[0][j - 2] && p.charAt(j - 1) == '*';
          }
          // induction
          for (int i = 1; i <= s.length(); ++i) {
              for (int j = 1; j <= p.length(); ++j) {
                  dp[i][j] = dp[i-1][j-1] &&
                      (s.charAt(i-1) == p.charAt(j-1) ||
                       p.charAt(j-1) == '.' ||
                       (p.charAt(j-1) == '*' && (p.charAt(j-2) == '.' ||
                                                 s.charAt(i-1) == p.charAt(j-2))))
                      || dp[i-1][j] && (p.charAt(j-1) == '*' && i > 1 && (s.charAt(i-1) == 
                                                                          s.charAt(i-2) &&
                                                                          s.charAt(i-1) == p.charAt(j-2) || p.charAt(j-2) == '.'))
                      || dp[i][j-1] && p.charAt(j-1) == '*'
                      || j > 1 && dp[i][j-2] && p.charAt(j-1) == '*';
              }
          }
          return dp[s.length()][p.length()];
      }
  }
```
