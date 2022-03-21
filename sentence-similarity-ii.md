---
description: LeetCode 737
---

# Sentence Similarity II

{% embed url="https://leetcode.com/problems/sentence-similarity-ii/" %}



Given two sentences `words1, words2` (each represented as an array of strings), and a list of similar word pairs `pairs`, determine if two sentences are similar.

For example, `words1 = ["great", "acting", "skills"]` and `words2 = ["fine", "drama", "talent"]` are similar, if the similar word pairs are `pairs = [["great", "good"], ["fine", "good"], ["acting","drama"], ["skills","talent"]]`.

Note that the similarity relation **is** transitive. For example, if "great" and "good" are similar, and "fine" and "good" are similar, then "great" and "fine" **are similar**.

Similarity is also symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.

Also, a word is always similar with itself. For example, the sentences `words1 = ["great"], words2 = ["great"], pairs = []` are similar, even though there are no specified similar word pairs.

Finally, sentences can only be similar if they have the same number of words. So a sentence like `words1 = ["great"]` can never be similar to `words2 = ["doubleplus","good"]`.

**Note:**

* The length of `words1` and `words2` will not exceed `1000`.
* The length of `pairs` will not exceed `2000`.
* The length of each `pairs[i]` will be `2`.
* The length of each `words[i]` and `pairs[i][j]` will be in the range `[1, 20]`.

{% hint style="info" %}
自然而然的想到可以用Union-Find来解决。

这里面的单词可以存在HashMap里，用count来记录uf里ids\[]的index。

然后对list里的单词做union操作。

判断有没有近义词，就是找对应的pair是不是在同一个联通块里。
{% endhint %}

```java
class Solution {
    public class UnionFind {
        private int[] ids;
        private int[] rank;
        private int size;
        
        public UnionFind(int size) {
            this.ids = new int[size];
            this.rank = new int[size];
            this.size = size;
            
            for (int i = 0; i < size; ++i) {
                ids[i] = i;
            }
        }
        
        public int find(int x) {
            if (ids[x] == x) {
                return ids[x];
            }
            
            if (ids[x] != x) {
                ids[x] = find(ids[x]);
                return ids[x];
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
                return true;
            }
            
            return true;
        }
    }
    public boolean areSentencesSimilarTwo(String[] words1, String[] words2, List<List<String>> pairs) {
        if (words1.length != words2.length) {
            return false;
        }
        
        //数据规模就这么大
        UnionFind uf = new UnionFind(2002);
        Map<String, Integer> map = new HashMap<>();
        int index = 0;                    //index记录string存放的位置
        for (List<String> list : pairs) {
            for (String str : list) {
                if (!map.containsKey(str)) {
                    map.put(str, index++);
                }
            }
            
            //union这一对pair
            uf.union(map.get(list.get(0)), map.get(list.get(1)));
        }
        
        for (int i = 0; i < words1.length; ++i) {
            String w1 = words1[i], w2 = words2[i];
            if (w1.equals(w2)) {
                continue;
            }
            
            if (!map.containsKey(w1) || !map.containsKey(w2) ||
               uf.find(map.get(w1)) != uf.find(map.get(w2))) {
                return false;
            }
        }
        
        return true;
    }
}
```
