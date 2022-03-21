---
description: LeetCode 14
---

# Longest Common Prefix

{% embed url="https://leetcode.com/problems/longest-common-prefix" %}



Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

&#x20;

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

&#x20;

**Constraints:**

* `1 <= strs.length <= 200`
* `0 <= strs[i].length <= 200`
* `strs[i]` consists of only lower-case English letters.

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        
        int start = 0;
        int end = Integer.MAX_VALUE;
        for (String str : strs) {
            end = Math.min(end, str.length());
        }
        
        while (start + 1 < end) {
            int mid = (start + end) / 2;
            if (canForm(mid, strs)) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (canForm(end, strs)) {
            return strs[0].substring(0, end);
        } else if (canForm(start, strs)) {
            return strs[0].substring(0, start);
        } else {
            return "";
        }
    }
    private boolean canForm(int index, String[] strs) {
        if (strs[0] == null || strs[0].length() == 0) return false;
        String str = strs[0].substring(0, index);
        for (String s : strs) {
            if (!s.startsWith(str)) {
                return false;
            }
        }
        
        return true;
    }
}
```
