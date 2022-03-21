---
description: LeetCode40
---

# Combination Sum II

{% embed url="https://leetcode.com/problems/combination-sum-ii/" %}

iven a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

* All numbers (including `target`) will be positive integers.
* The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

去重情况：\[1, 1, 2, 5] target = 8

\[1, 2, 5] \[1', 2, 5]是一种情况

这种情况下1'被continue掉，因为startIndex从0开始的时候，1满足条件，1'不满足条件，所以1要被continue掉，否则会出现两个相同的解。

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        if (candidates == null || candidates.length == 0) {
            return results;
        }
        List<Integer> combination = new ArrayList<>();
        Arrays.sort(candidates);
        dfs(candidates, 0, combination, target, results);
        return results;
    }
    private void dfs(int[] candidates,
                    int startIndex,
                    List<Integer> combination,
                    int remainTarget,
                    List<List<Integer>> results) {
        if (remainTarget == 0) {
            results.add(new ArrayList<>(combination));
            return;
        }
        
        for (int i = startIndex; i < candidates.length; ++i) {
            if (candidates[i] > remainTarget) {
                break;
            }
            if (i > startIndex && candidates[i] == candidates[i - 1]) {
                continue;
            }
            combination.add(candidates[i]);
            dfs(candidates, i + 1, combination, remainTarget - candidates[i], results);
            combination.remove(combination.size() - 1);
        }
    }
}
```
