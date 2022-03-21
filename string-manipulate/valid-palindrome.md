---
description: LeetCode 125
---

# Valid Palindrome

{% embed url="https://leetcode.com/problems/valid-palindrome/" %}

Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Example 1:**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Constraints:**

* `1 <= s.length <= 2 * 105`
* `s` consists only of printable ASCII characters.

{% hint style="info" %}
这道题的关键在于start和end指针分别从两端跳过不符合条件的点，遇到合法的character再比较，如果相同就移动指针，不同就return false。
{% endhint %}

```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s.length() < 2) {
            return true;
        }
        
        int start = 0, end = s.length() - 1;
        while (start < end) {
            while (start < s.length() - 1 && !isValid(s.charAt(start))) {
                start++;
            }
            
            if (start == s.length() - 1) {
                return true;
            }
            
            while (end >= 0 && !isValid(s.charAt(end))) {
                end--;
            }
            
            if (Character.toLowerCase(s.charAt(start)) != Character.toLowerCase(s.charAt(end))) {
                return false;
            } else {
                start++;
                end--;
            }
        }
        
        return true;
    }
    
    private boolean isValid(Character ch) {
        return Character.isDigit(ch) || Character.isLetter(ch);
    }
}
```
