---
description: LeetCode 316
---

# Remove Duplicate Letters

{% embed url="https://leetcode.com/problems/remove-duplicate-letters/" %}

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

**Example 1:**

```
Input: s = "bcabc"
Output: "abc"
```

**Example 2:**

```
Input: s = "cbacdcbc"
Output: "acdb"
```

**Constraints:**

* `1 <= s.length <= 104`
* `s` consists of lowercase English letters.

**Intuition**

First we should make sure we understand what "lexicographical order" means. Comparing strings doesn't work the same way as comparing numbers. Strings are compared from the first character to the last one. Which string is greater depends on the comparison between _the first unequal corresponding character_ in the two strings. As a result any string beginning with `a` will always be less than any string beginning with `b`, regardless of the ends of both strings.

Because of this, the optimal solution will have _the smallest characters as early as possible_. We draw two conclusions that provide different methods of solving this problem in O(N)O(N):

1.  The leftmost letter in our solution will be the smallest letter such that the suffix from that letter contains every other. This is because we know that the solution must have one copy of every letter, and we know that the solution will have the lexicographically smallest leftmost character possible.

    If there are multiple smallest letters, then we pick the leftmost one simply because it gives us more options. We can always eliminate more letters later on, so the optimal solution will always remain in our search space.
2. As we iterate over our string, if character `i` is greater than character `i+1` and another occurrence of character `i` exists later in the string, deleting character `i` will **always** lead to the optimal solution. Characters that come later in the string `i`don't matter in this calculation because `i` is in a more significant spot. Even if character `i+1` isn't the best yet, we can always replace it for a smaller character down the line if possible.

Since we try to remove characters as early as possible, and picking the best letter at each step leads to the best solution, "greedy" should be going off like an alarm.

**Approach 1: Greedy - Solving Letter by Letter**

**Algorithm**

We use idea number one from the intuition. In each iteration, we determine leftmost letter in our solution. This will be **the smallest character such that its suffix contains at least one copy of every character in the string**. We determine the rest our answer by recursively calling the function on the suffix we generate from the original string (leftmost letter is removed).

**Complexity Analysis**

* Time complexity : O(N). Each recursive call will take O(N). The number of recursive calls is bounded by a constant (26 letters in the alphabet), so we have O(N)∗C=O(N)∗C=O(N).
* Space complexity : O(N). Each time we slice the string we're creating a new one (strings are immutable). The number of slices is bound by a constant, so we have O(N)∗C=O(N)∗C=O(N).

**Implementation**

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        // find pos - the index of the leftmost letter in our solution
        // we create a counter and end the iteration once the suffix doesn't have each unique character
        // pos will be the index of the smallest character we encounter before the iteration ends
        int[] cnt = new int[26];
        int pos = 0;
        for (int i = 0; i < s.length(); i++) cnt[s.charAt(i) - 'a']++;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) < s.charAt(pos)) pos = i;
            if (--cnt[s.charAt(i) - 'a'] == 0) break;
        }
        // our answer is the leftmost letter plus the recursive call on the remainder of the string
        // note that we have to get rid of further occurrences of s[pos] to ensure that there are no duplicates
        return s.length() == 0 ? "" : s.charAt(pos) + removeDuplicateLetters(s.substring(pos + 1).replaceAll("" + s.charAt(pos), ""));
    }
}
//s = "cbacdcbc";
//cnt = {1, 2, 4, 1};
//       a, b, c, d
//
```
