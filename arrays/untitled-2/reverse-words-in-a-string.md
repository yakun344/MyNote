# Reverse Words in a String

{% embed url="https://leetcode.com/problems/reverse-words-in-a-string" %}

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return _a string of the words in reverse order concatenated by a single space._

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

&#x20;

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Example 4:**

```
Input: s = "  Bob    Loves  Alice   "
Output: "Alice Loves Bob"
```

**Example 5:**

```
Input: s = "Alice does not even like bob"
Output: "bob like even not does Alice"
```

&#x20;

**Constraints:**

* `1 <= s.length <= 104`
* `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
* There is **at least one** word in `s`.

&#x20;

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?

```java
class Solution {
    public String reverseWords(String s) {
        Stack<String> stack = new Stack<>();
       
        int left = 0;
        while (left < s.length()) {
            int right = left;
            while (left < s.length() && s.charAt(left) == ' ') {
                left++;
            }
            if (left == s.length()) break;
            right = left;
            while (right < s.length() && s.charAt(right) != ' ') {
                right++;
            }
            
            String str = s.substring(left, right);
            stack.push(str);
            
            left = right;
        }
        
        String res = "";
        while (!stack.isEmpty()) {
            String cur = stack.pop();
            res += cur + " ";
        }
        return res.substring(0, res.length() - 1);
    }
}
```
