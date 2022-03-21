---
description: LeetCode 842
---

# Split Array into Fibonacci Sequence

{% embed url="https://leetcode.com/problems/split-array-into-fibonacci-sequence/" %}



Given a string `S` of digits, such as `S = "123456579"`, we can split it into a _Fibonacci-like sequence_ `[123, 456, 579].`

Formally, a Fibonacci-like sequence is a list `F` of non-negative integers such that:

* `0 <= F[i] <= 2^31 - 1`, (that is, each integer fits a 32-bit signed integer type);
* `F.length >= 3`;
* and `F[i] + F[i+1] = F[i+2]` for all `0 <= i < F.length - 2`.

Also, note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number 0 itself.

Return any Fibonacci-like sequence split from `S`, or return `[]` if it cannot be done.

**Example 1:**

```
Input: "123456579"
Output: [123,456,579]
```

**Example 2:**

```
Input: "11235813"
Output: [1,1,2,3,5,8,13]
```

**Example 3:**

```
Input: "112358130"
Output: []
Explanation: The task is impossible.
```

**Example 4:**

```
Input: "0123"
Output: []
Explanation: Leading zeroes are not allowed, so "01", "2", "3" is not valid.
```

**Example 5:**

```
Input: "1101111"
Output: [110, 1, 111]
Explanation: The output [11, 0, 11, 11] would also be accepted.
```

**Note:**

1. `1 <= S.length <= 200`
2. `S` contains only digits.

{% hint style="info" %}
看易错点N^2
{% endhint %}

> Time complexity is O(10^n).
>
> * At each level, we branch at most 10 times, as the for-loop will run until Integer.MAX\_VALUE is reached (which is maximum 10 digits in length).
> * Our recursive calls go to a maximum depth of n.
> * The time complexity of any recursive function is O(branches^depth)
>
> Therefore the upper-bound time complexity is O(10^n)

```java
class Solution {
    public List<Integer> splitIntoFibonacci(String S) {
        List<Integer> res = new ArrayList<>();
        
        dfs(S, res, 0);
        return res;
    }
    
    private boolean dfs(String str, List<Integer> res, int start) {
        if (str.length() == start && res.size() >= 3) {
            return true;
        }
        
        for (int i = start; i < str.length(); ++i) {
            //'0'允许出现在答案里，但是'01','02'这类的不可以。
            if (str.charAt(start) == '0' && i > start) {
                break;
            }
            
            long num = Long.parseLong(str.substring(start, i + 1));
            if (num > Integer.MAX_VALUE) {
                break;
            }
            
            //比较的对象一定要是list里面最后两个
            //易错点：num > res.get(i - 1) + res.get(i - 2)
            int size = res.size();
            if (size >= 2 && num > res.get(size - 1) + res.get(size - 2)) {
                break;
            }
            
            if (size <= 1 || num == res.get(size - 1) + res.get(size - 2)) {
                res.add((int)num);
                if (dfs(str, res, i + 1)) {
                    return true;
                }
                res.remove(res.size() - 1);
            }
        }
        return false;
    }
}
```
