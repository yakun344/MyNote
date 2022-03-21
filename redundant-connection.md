---
description: LeetCode 684
---

# Redundant Connection

{% embed url="https://leetcode.com/problems/redundant-connection/" %}

In this problem, a tree is an **undirected** graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

**Example 1:**\


```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```

**Example 2:**\


```
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

**Note:**\


The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

**Update (2017-09-26):**\
We have overhauled the problem description + test cases and specified clearly the graph is an **undirected** graph. For the **directed** graph follow up please see [**Redundant Connection II**](https://leetcode.com/problems/redundant-connection-ii/description/)). We apologize for any inconvenience caused.\


{% hint style="info" %}
Union-Find Approach

注意⚠️index不要越界，做法是扩大uf中ids数组的大小。

如果两个点不在一个联通块中，那就union；如果没办法union，那就证明两个点已经是在一个联通块中了。把这条边返回即可。
{% endhint %}

```java
class Solution {
    public class UnionFind {
        private int ids[];
        private int rank[];
        
        public UnionFind(int size) {
            this.ids = new int[size];
            this.rank = new int[size];
            
            for (int i = 0; i < size; ++i) {
                ids[i] = i;
            }
        }
        
        public int find(int x) {
            if (ids[x] == x) {
                return x;
            }
            
            if (ids[x] != x) {
                ids[x] = find(ids[x]);
            }
            return ids[x];
        }
        
        public boolean union(int x, int y) {
            int rootx = find(x);
            int rooty = find(y);
            
            if (rootx == rooty) {
                return false;
            }
            
            if (rank[rootx] < rank[rooty]) {
                ids[rootx] = rooty;
            } else if (rank[rootx] > rank[rooty]) {
                ids[rooty] = rootx;
            } else {
                ids[rootx] = rooty;
                rank[rooty]++;
            }
            return true;
        }
    }
    public int[] findRedundantConnection(int[][] edges) {
        int[] res = new int[2];
        UnionFind uf = new UnionFind(edges.length + 1);
        for (int[] coo : edges) {
            if (!uf.union(coo[0], coo[1])) {
                res[0] = coo[0];
                res[1] = coo[1];
                return res;
            }
        }
        return res;
    }
}
```
