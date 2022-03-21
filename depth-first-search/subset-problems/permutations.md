---
description: LeetCode 46
---

# Permutations

{% embed url="https://leetcode.com/problems/permutations/" %}

Given a collection of **distinct** integers, return all possible permutations.

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

Time Complexity: N!

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return results;
        }
        List<Integer> permutation = new ArrayList<>();
        //boolean[] visited = new boolean[nums.length];
        int[] visited = new int[nums.length];
        dfs(nums, visited, permutation, results);
        return results;
    }
    private void dfs(int[] nums,
                    int[] visited,
                    List<Integer> permutation,
                    List<List<Integer>> results) {
        if (permutation.size() == nums.length) {
            results.add(new ArrayList<>(permutation));
            return;
        }
        
        for (int i = 0; i < nums.length; ++i) {
            if (visited[i] == 1) {
                continue;
            }
            
            visited[i] = 1;
            permutation.add(nums[i]);
            dfs(nums, visited, permutation, results);
            visited[i] = 0;
            permutation.remove(permutation.size() - 1);
        }
    }
}
```

