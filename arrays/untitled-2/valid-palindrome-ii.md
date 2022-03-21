---
description: LeetCode 680 LintCode 891
---

# Valid Palindrome II

{% embed url="https://leetcode.com/problems/valid-palindrome-ii/" %}

{% embed url="https://www.lintcode.com/problem/valid-palindrome-ii/description" %}

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

**Example 1:**\


```
Input: "aba"
Output: True
```

**Example 2:**\


```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**Note:**\


1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

```java
class Solution {
    public boolean validPalindrome(String s) {
        if (s.length() < 2) {
            return true;
        }
        
        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return subPalindrome(s, left + 1, right) || subPalindrome(s, left, right - 1);
            } else {
                left++;
                right--;
            }
        }
        return true;
    }
    private boolean subPalindrome(String s, int left, int right) {
        if (left >= right) {
            return true;
        }
        
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            } else {
                left++;
                right--;
            }
        }
        return true;
    }
}
```

```java
class Solution {
    public boolean validPalindrome(String s) {
        if (s.length() == 0 || s == null) return false;
        
        int left = 0, right = s.length() - 1;
        
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                break;
            }
            left++;
            right--;
        }
        
        return subPalindrome(s, left + 1, right) || subPalindrome(s, left, right - 1);
    }
    
    private boolean subPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
