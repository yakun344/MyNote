---
description: LeetCode 409
---

# Longest Palindrome

{% embed url="https://leetcode.com/problems/longest-palindrome/" %}



Given a string `s` which consists of lowercase or uppercase letters, return _the length of the **longest palindrome**_ that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

**Example 1:**

```
Input: s = "abccccdd"
Output: 7
Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

**Example 2:**

```
Input: s = "a"
Output: 1
```

**Example 3:**

```
Input: s = "bb"
Output: 2
```

**Constraints:**

* `1 <= s.length <= 2000`
* `s` consists of lowercase **and/or** uppercase English letters only.

{% hint style="info" %}
用set记录出现次数为奇数的字母，如果set.size() > 0证明奇数次数的字母是大于0的。组成palindrome最多只能有一个奇数次数的字母。结果用s.length() - ood即可。
{% endhint %}

```java
class Solution {
    public int longestPalindrome(String s) {
        Set<Character> set = new HashSet<>();
        for (char ch : s.toCharArray()) {
            if (!set.contains(ch)) {
                set.add(ch);
            } else {
                set.remove(ch);
            }
        }
        
        int odd = set.size();
        if (odd > 0) {
            odd--;
        }
        
        return s.length() - odd;
    }
}
```
