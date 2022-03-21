---
description: LeetCode 216
---

# Combination Sum III

{% embed url="https://leetcode.com/problems/combination-sum-iii/" %}

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

* All numbers will be positive integers.
* The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> results = new ArrayList<>();
        if (k <= 0) {
            return results;
        }
        List<Integer> combination = new ArrayList<>();
        dfs(k, n, 1,combination, results);
        return results;
    }
    
    private void dfs(int k,
                    int remainTarget,
                    int startIndex,
                    List<Integer> combination,
                    List<List<Integer>> results) {
        if (combination.size() == k && remainTarget == 0) {
            results.add(new ArrayList<>(combination));
            return;
        }
        for (int i = startIndex; i < 10; ++i) {
            if (i > remainTarget) {
                break;
            }
            
            combination.add(i);
            dfs(k, remainTarget - i, i + 1, combination, results);
            combination.remove(combination.size() - 1);
        }
    }
}
```
