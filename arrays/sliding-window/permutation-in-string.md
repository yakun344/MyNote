---
description: LeetCode 567
---

# Permutation in String

{% embed url="https://leetcode.com/problems/permutation-in-string/" %}



Given two strings `s1` and `s2`, return true if `s2` contains the permutation of `s1`.

In other words, one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

**Constraints:**

* `1 <= s1.length, s2.length <= 104`
* `s1` and `s2` consist of lowercase English letters.

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int count = s1.length();
        int left = 0, right = 0;
        int[] nums = new int[26];
        for (char ch : s1.toCharArray()) {
            nums[ch - 'a']++;
        }
        
        while (left < s2.length()) {
            while (right < s2.length() && right - left < s1.length()) {
                char ch = s2.charAt(right);
                if (nums[ch - 'a'] > 0) {
                    count--;
                }
                nums[ch - 'a']--;
                right++;
            }
            
            if (count == 0) return true;
            
            char ch = s2.charAt(left);
            if (nums[ch - 'a'] >= 0) {
                count++;
            }
            nums[ch - 'a']++;
            
            left++;
        }
        return false;
    }
}
```
