---
description: LeetCode 90
---

# Subsets II

{% embed url="https://leetcode.com/problems/subsets-ii/" %}

{% hint style="danger" %}
Evaluate Order!

⚠️透过递归过程的本质来看问题，要去重的是哪一块。
{% endhint %}

Given a collection of integers that might contain duplicates, _**nums**_, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

![](<../../.gitbook/assets/image (20).png>)

**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new ArrayList<>();
        }
        List<List<Integer>> results = new ArrayList<>();
        List<Integer> subSet = new ArrayList<>();
        Arrays.sort(nums);
        dfs(nums, 0, subSet, results);
        return results;
    }
    
    private void dfs(int[] nums,
                    int startIndex,
                    List<Integer> subSet,
                    List<List<Integer>> results) {
        results.add(new ArrayList<>(subSet));
        
        for (int i = startIndex; i < nums.length; ++ i) {
            if (i > startIndex && nums[i] == nums[i - 1]) {
                continue;
            }
            subSet.add(nums[i]);
            dfs(nums, i + 1, subSet, results);
            subSet.remove(subSet.size() - 1);
        }
    }
}
```
