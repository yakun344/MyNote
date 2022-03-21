---
description: LeetCode 125 LintCode415
---

# Valid Palindrome

{% embed url="https://leetcode.com/problems/valid-palindrome/" %}

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**

```
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```
Input: "race a car"
Output: false
```

{% hint style="info" %}
考虑特殊情况，如果所有字符都是非法字符，例如符号，那么在left指针走到尾时， 就应该left == s.length() - 1，此时也是valid palindtrom
{% endhint %}

```java
class Solution {
    public boolean isPalindrome(String s) {
    
        //空字符串或长度为1的字符串
        if (s.length() < 2) {
            return true;
        }
        
        int left = 0, right = s.length() - 1;
        while (left < right) {
        
            //左指针，向右，最终走到最右
            while (left < s.length() - 1 && !isValid(s.charAt(left))) {
                left++;
            }
            
            //这里注意注意，跳过n个非法字符，判断是否是空字符串
            //例如：".,"
            if (left == s.length() - 1) {
                return true;
            }
            
            //右指针，向左，最终走到最左
            while (right >= 0 && !isValid(s.charAt(right))) {
                right--;
            }
            
            //当前左右指针的小写字母不一样，返回false
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            } else {
                left++;
                right--;
            }
        }
        return true;
    }
    private boolean isValid(char s) {
        return Character.isDigit(s) || Character.isLetter(s);
    }
}
```
