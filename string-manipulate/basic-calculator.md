---
description: LeetCode 224
---

# Basic Calculator

{% embed url="https://leetcode.com/problems/basic-calculator/" %}



Given a string `s` representing an expression, implement a basic calculator to evaluate it.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

**Constraints:**

* `1 <= s.length <= 3 * 105`
* `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
* `s` represents a valid expression.

{% hint style="info" %}
用num记录当前的数字；

用sign = -1 / 1这两个数字来记录当前的操作+ / -；

用stack来记录要进行的操作和数字，res和sign；

遇到‘(‘，就把结果push到stack，更新res和sign；

遇到')'，更新当前括号里计算出来的数字res，重置num，然后计算和括号前面数字计算的结果，加入到res里。

Finally if there is only one number, from the above solution, we haven't add the number to the result, so we do a check see if the number is zero.
{% endhint %}

```java
class Solution {
    public int calculate(String s) {
        int res = 0;
        Stack<Integer> stack = new Stack<>();
        int num = 0;
        int sign = 1;
        
        
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (Character.isDigit(ch)) {
                num = num * 10 + (ch - '0');
            }
            if (!Character.isDigit(ch)) {
                if (ch == '+') {
                    res += sign * num;
                    num = 0;
                    sign = 1;
                } else if (ch == '-') {
                    res += sign * num;
                    num = 0;
                    sign = -1;
                } else if (ch == '(') {
                    stack.push(res);
                    stack.push(sign);
                    sign = 1;
                    res = 0;
                } else if (ch == ')') {
                    res += sign * num;
                    num = 0;
                    res *= stack.pop();
                    res += stack.pop();
                }
            }
        }
        
        if (num != 0) {
            res += sign * num;
        }

        return res;
    }
}
// ( 1 
```
