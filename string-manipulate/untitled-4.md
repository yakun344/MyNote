---
description: LeetCode 266
---

# Palindrome Permutation

{% embed url="https://leetcode.com/problems/palindrome-permutation/" %}

Given a string `s`, return `true` if a permutation of the string could form a palindrome.

**Example 1:**

```
Input: s = "code"
Output: false
```

**Example 2:**

```
Input: s = "aab"
Output: true
```

**Example 3:**

```
Input: s = "carerac"
Output: true
```

**Constraints:**

* `1 <= s.length <= 5000`
* `s` consists of only lowercase English letters.

{% hint style="info" %}
用HashMap统计所有character出现的次数，如果出现频率奇数个的character大于1，那么就没有palindrome。
{% endhint %}

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }
        
        int odd = 0;
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            if (entry.getValue() % 2 != 0) {
                odd++;
            }
            if (odd > 1) {
                return false;
            }
        }
        return true;
    }
}
```
