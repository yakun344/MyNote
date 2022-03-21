---
description: LeetCode 168
---

# Excel Sheet Column Title

{% embed url="https://leetcode.com/problems/excel-sheet-column-title" %}



Given an integer `columnNumber`, return _its corresponding column title as it appears in an Excel sheet_.

For example:

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

&#x20;

**Example 1:**

```
Input: columnNumber = 1
Output: "A"
```

**Example 2:**

```
Input: columnNumber = 28
Output: "AB"
```

**Example 3:**

```
Input: columnNumber = 701
Output: "ZY"
```

**Example 4:**

```
Input: columnNumber = 2147483647
Output: "FXSHRXW"
```

&#x20;

**Constraints:**

* `1 <= columnNumber <= 231 - 1`

```java
class Solution {
    public String convertToTitle(int n) {
        StringBuilder result = new StringBuilder();

        while (n > 0){
            n--;
            result.insert(0, (char)('A' + n % 26));
            n /= 26;
        }

        return result.toString();
    }
}
```
