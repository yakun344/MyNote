---
description: LeetCode 186
---

# Reverse Words in a String II

{% embed url="https://leetcode.com/problems/reverse-words-in-a-string-ii" %}

Given a character array `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by a single space.

Your code must solve the problem **in-place,** i.e. without allocating extra space.

&#x20;

**Example 1:**

```
Input: s = ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```

**Example 2:**

```
Input: s = ["a"]
Output: ["a"]
```

&#x20;

**Constraints:**

* `1 <= s.length <= 105`
* `s[i]` is an English letter (uppercase or lowercase), digit, or space `' '`.
* There is **at least one** word in `s`.
* `s` does not contain leading or trailing spaces.
* All the words in `s` are guaranteed to be separated by a single space.

```java
class Solution {
    //eht yks si eulb -> blue is sky the
    public void reverseWords(char[] s) {
        int left = 0, right = 0;
        while (right < s.length) {
            while (right < s.length && Character.isLetter(s[right])) {
                right++;
            }
            if (right == s.length) {
                reverse(s, left, right - 1);
                break;
            }
            reverse(s, left, right - 1);
            right++;
            left = right;
        }
        reverse(s, 0, s.length - 1);
    }
    private void reverse(char[] s, int left, int right) {
        while (left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```
