---
description: LeetCode 1190
---

# Reverse Substrings Between Each Pair of Parentheses

{% embed url="https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses" %}



You are given a string `s` that consists of lower case English letters and brackets.&#x20;

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should **not** contain any brackets.

&#x20;

**Example 1:**

```
Input: s = "(abcd)"
Output: "dcba"
```

**Example 2:**

```
Input: s = "(u(love)i)"
Output: "iloveu"
Explanation: The substring "love" is reversed first, then the whole string is reversed.
```

**Example 3:**

```
Input: s = "(ed(et(oc))el)"
Output: "leetcode"
Explanation: First, we reverse the substring "oc", then "etco", and finally, the whole string.
```

**Example 4:**

```
Input: s = "a(bcdefghijkl(mno)p)q"
Output: "apmnolkjihgfedcbq"
```

&#x20;

**Constraints:**

* `0 <= s.length <= 2000`
* `s` only contains lower case English characters and parentheses.
* It's guaranteed that all parentheses are balanced.

{% hint style="info" %}
**Brute** Force(O(n^2))
{% endhint %}

```java
class Solution {
    public String reverseParentheses(String s) {
        Stack<Character> stack = new Stack<>();
        for (char ch : s.toCharArray()) {
            if (ch == ')') {
                Queue<Character> q = new LinkedList<>();
                while (!stack.isEmpty() && stack.peek() != '(') {
                    q.add(stack.pop());
                }
                
                //去掉当前后括号的前括号
                if (!stack.isEmpty()) stack.pop();
                while (!q.isEmpty()) {
                    stack.push(q.remove());
                }
            } else {
                stack.push(ch);
            }
        }
        
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        
        return sb.reverse().toString();
    }
}
```

![](<../.gitbook/assets/image (54).png>)

```java
class Solution {
    public String reverseParentheses(String s) {
        int n = s.length();
        Stack<Integer> opened = new Stack<>();
        int[] pair = new int[n];
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '(')
                opened.push(i);
            if (s.charAt(i) == ')') {
                int j = opened.pop();
                pair[i] = j;
                pair[j] = i;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0, d = 1; i < n; i += d) {
            if (s.charAt(i) == '(' || s.charAt(i) == ')') {
                i = pair[i];
                d = -d;
            } else {
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }
}
```
