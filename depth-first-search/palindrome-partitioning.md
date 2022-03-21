---
description: LeetCode 131
---

# Palindrome Partitioning

{% embed url="https://leetcode.com/problems/palindrome-partitioning" %}

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

&#x20;

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]
```



**Constraints:**

* `1 <= s.length <= 16`
* `s` contains only lowercase English letters.

```java
class Solution {    //O(N*2^N)
    public List<List<String>> partition(String s) {
        if (s == null || s.length() == 0) {
            return new ArrayList<>();
        }
        
        List<List<String>> result = new ArrayList<>();
        List<String> palindrome = new ArrayList<>();
        dfs(s, 0, palindrome, result);
        return result;
    }
    
    private void dfs(String s,
                    int startIndex,
                    List<String> palindrome,
                    List<List<String>> result) {
        if (startIndex == s.length()) {
            result.add(new ArrayList<>(palindrome));
            return;
        }
        
        for (int i = startIndex; i < s.length(); ++i) {
            String sub = s.substring(startIndex, i + 1);
            if (!check(sub)) {
                continue;
            }
            palindrome.add(sub);
            dfs(s, i + 1, palindrome, result);
            palindrome.remove(palindrome.size() - 1);
        }
    }
    private boolean check(String s) {
        for (int i = 0, j = s.length() - 1; i < j; ++i, --j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
}
```
