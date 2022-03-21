---
description: LeetCode 22 LintCode 427
---

# Generate Parentheses

{% embed url="https://leetcode.com/problems/generate-parentheses/" %}

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> results = new ArrayList<>();
        if (n <= 0) {
            return results;
        }
        
        search(results, "", n, n);
        return results;
    }
    
    private void search(List<String> results, String cur, int left, int right) {
        if (left == 0 && right == 0) {
            results.add(cur);
            return;
        }
        
        //先左边👈再右边👉
        if (left > 0) {
            search(results, cur + "(", left - 1, right);
        }
        
        //每次产生括号的时候，一定要先产生左边的，再生成右边的。
        //否则就会出现))((这种情况
        //所以要left < right判断一下
        if (right > 0 && left < right) {
            search(results, cur + ")", left, right - 1);
        }
    }
}
```
