---
description: LeetCode 227
---

# Basic Calculator II

{% embed url="https://leetcode.com/problems/basic-calculator-ii/" %}



Given a string `s` which represents an expression, _evaluate this expression and return its value_.&#x20;

The integer division should truncate toward zero.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

```
Input: s = "3+2*2"
Output: 7
```

**Example 2:**

```
Input: s = " 3/2 "
Output: 1
```

**Example 3:**

```
Input: s = " 3+5 / 2 "
Output: 5
```

**Constraints:**

* `1 <= s.length <= 3 * 105`
* `s` consists of integers and operators `('+', '-', '*', '/')` separated by some number of spaces.
* `s` represents **a valid expression**.
* All the integers in the expression are non-negative integers in the range `[0, 231 - 1]`.
* The answer is **guaranteed** to fit in a **32-bit integer**.

{% hint style="info" %}
用栈模拟运算过程。

num记录当前的数字，用operation记录上一个运算符。

对栈顶的数字和num来进行operation运算符的操作，将当前运算符计算的结果push到stack中。

遍历整个string之后，把栈中的数字pop出来并相加就是所得到的结果。

实现细节注意⚠️当走到末尾的时候，要对当前的数字和栈顶的数字进行operation操作。
{% endhint %}

```java
class Solution {
    public int calculate(String s) {
        Stack<Integer> stack = new Stack<>();
        int res = 0;
        char operation = '+';
        int num = 0;
        
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (Character.isDigit(ch)) {
                num = num * 10 + (ch - '0');
            }
            if (!Character.isDigit(ch) && !Character.isWhitespace(ch) ||
                i == s.length() - 1) {
                if (operation == '-') {
                    stack.push(-num);
                } else if (operation == '+') {
                    stack.push(num);
                } else if (operation == '*') {
                    stack.push(stack.pop() * num);
                } else if (operation == '/') {
                    stack.push(stack.pop() / num);
                }
                
                operation = ch;
                num = 0;
            }
        }
        
        while (!stack.isEmpty()) {
            res += stack.pop();
        }
        
        return res;
    }
}
```
