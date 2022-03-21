---
description: LeetCode 254
---

# Factor Combinations

{% embed url="https://leetcode.com/problems/factor-combinations/" %}



Numbers can be regarded as the product of their factors.

* For example, `8 = 2 x 2 x 2 = 2 x 4`.

Given an integer `n`, return _all possible combinations of its factors_. You may return the answer in **any order**.

**Note** that the factors should be in the range `[2, n - 1]`.

**Example 1:**

```
Input: n = 1
Output: []
```

**Example 2:**

```
Input: n = 12
Output: [[2,6],[3,4],[2,2,3]]
```

**Example 3:**

```
Input: n = 37
Output: []
```

**Example 4:**

```
Input: n = 32
Output: [[2,16],[4,8],[2,2,8],[2,4,4],[2,2,2,4],[2,2,2,2,2]]
```

**Constraints:**

* `1 <= n <= 108`

{% hint style="danger" %}
注意⚠️返回值不包括本身。而本身作为因子的返回值，path.size()为1，只包括n本身。所以要在size  > 1的情况下返回符合条件的path。



Time complexity: O(NlogN). The explain are in the following:

formula 1: time = (the number of nodes in the recursive tree) \* (the time each node takes up)

formula 2: the number of nodes in the recursive tree  = (the most number of branches among each node) ^ (the height of the tree)
{% endhint %}

```java
class Solution {
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        
        dfs(res, path, 2, n);
        return res;
    }
    private void dfs(List<List<Integer>> res,
                    List<Integer> path,
                    int start,
                    int remain) {
        if (remain <= 1) {
            if (path.size() > 1) {
                res.add(new ArrayList<>(path));
            }
            
            return;
        }
        
        for (int i = start; i <= remain; ++i) {
            if (remain % i == 0) {
                path.add(i);
                dfs(res, path, i, remain / i);
                path.remove(path.size() - 1);
            }
        }
    }
}
```
