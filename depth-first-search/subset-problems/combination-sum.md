---
description: LeetCode 39
---

# Combination Sum

{% embed url="https://leetcode.com/problems/combination-sum/" %}

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

* All numbers (including `target`) will be positive integers.
* The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

{% hint style="info" %}
易错点，results.add(new ArrayList<>(combination));

⚠️把combination(collection)里的元素加入到一个new出来的ArrayList里，然后把这个ArrayList加入到结果里。
{% endhint %}

![](<../../.gitbook/assets/image (43).png>)

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        if (candidates == null || candidates.length == 0) {
            return results;
        }
        List<Integer> combination = new ArrayList<>();
        Arrays.sort(candidates);
        dfs(candidates, target, 0, combination, results);
        return results;
    }
    
    private void dfs(int[] candidates,
                    int remainTarget,
                    int startIndex,
                    List<Integer> combination,
                    List<List<Integer>> results) {
        if (remainTarget == 0) {
            results.add(new ArrayList<>(combination));
            return;
        }
        
        for (int i = startIndex; i < candidates.length; ++i) {
            if (remainTarget < candidates[i]) {
                break;
            }
            if (i > 0 && candidates[i] == candidates[i - 1]) {
                continue;
            }
            combination.add(candidates[i]);
            dfs(candidates, remainTarget - candidates[i], i, combination, results);
            combination.remove(combination.size() - 1);
        }
    }
}
```
