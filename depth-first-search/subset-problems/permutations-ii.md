---
description: LeetCode 47
---

# Permutations II

{% embed url="https://leetcode.com/problems/permutations-ii/" %}

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

**Example:**

```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

{% hint style="info" %}
有重复数字的时候，去重的是前几次出现的数字，最后一次出现的数字将会被保留。

考虑\[1, 1', 2]的情况：(红色圈着的nums\[i]是被continue掉的数字)。
{% endhint %}

![](<../../.gitbook/assets/image (21).png>)

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new ArrayList<>();
        }
        
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> permutation = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        
        dfs(result, permutation, visited, nums);
        
        return result;
    }
    
    private void dfs(List<List<Integer>> result,
                    List<Integer> permutation,
                    boolean[] visited,
                    int[] nums) {
        if (permutation.size() == nums.length) {
            result.add(new ArrayList<>(permutation));
            return;
        }
        
        for (int i = 0; i < nums.length; ++i) {
            if (visited[i]) continue;
            if (i > 0 && nums[i - 1] == nums[i] && visited[i - 1]) continue;
            visited[i] = true;
            permutation.add(nums[i]);
            dfs(result, permutation, visited, nums);
            visited[i] = false;
            permutation.remove(permutation.size() - 1);
        }
    }
}
```
