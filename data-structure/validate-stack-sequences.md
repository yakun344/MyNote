---
description: LeetCode 946
---

# Validate Stack Sequences

{% embed url="https://leetcode.com/problems/validate-stack-sequences" %}

Given two integer arrays `pushed` and `popped` each with distinct values, return `true` _if this could have been the result of a sequence of push and pop operations on an initially empty stack, or_ `false` _otherwise._

&#x20;

**Example 1:**

```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4),
pop() -> 4,
push(5),
pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**Example 2:**

```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.
```

&#x20;

**Constraints:**

* `1 <= pushed.length <= 1000`
* `0 <= pushed[i] <= 1000`
* All the elements of `pushed` are **unique**.
* `popped.length == pushed.length`
* `popped` is a permutation of `pushed`.

模拟栈的push和pop，核对完数组A后，如果stack还有数字，那么证明序列不符合。

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int index = 0;
        for (int i = 0; i < pushed.length; ++i) {
            stack.push(pushed[i]);
            while (!stack.isEmpty() && stack.peek() == popped[index]) {
                stack.pop();
                index++;
            }
        }
        
        return stack.isEmpty();
    }
}
```
