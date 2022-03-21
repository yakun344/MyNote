# Union-Find Algorithm

## Union Find

```
Disjoint Set | Union-Find Algorithm – Union by rank and path compression
```

{% embed url="https://algorithms.tutorialhorizon.com/disjoint-set-union-find-algorithm-union-by-rank-and-path-compression/" %}

{% hint style="info" %}
通过预先设定Disjoint set 和 Size来实现Union-find
{% endhint %}

Uses Find to determine the roots of the trees x and y belong to. If the roots are distinct, the trees are combined by attaching the root of one to the root of the other. If this is done naively, such as by always making x a child of y, the height of the trees can grow as **O(n).** We can optimize it by using union by rank.

**How Disjoint Set is optimized**:

* Union by rank.
* Path by compression.

**Union by rank:**

Uses Find to determine the roots of the trees x and y belong to. If the roots are distinct, the trees are combined by attaching the root of one to the root of the other. If this is done naively, such as by always making x a child of y, the height of the trees can grow as **O(n).** We can optimize it by using union by rank.

_Union by rank_ always attaches the shorter tree to the root of the taller tree.

To implement union by rank, each element is associated with a rank. Initially a set has one element and a rank of zero.

If we union two sets and

* Both trees have the same rank – the resulting set’s rank is one larger
* Both trees have the different ranks – the resulting set’s rank is the larger of the two. Ranks are used instead of height or depth because path compression will change the trees’ heights over time.

Worst case complexity: O(LogN)

#### Explanation

![](<.gitbook/assets/image (19).png>)

**Path compression**:

Path compression is a way of flattening the structure of the tree whenever Find is used on it. Since each element visited on the way to a root is part of the same set, all of these visited elements can be reattached directly to the root. The resulting tree is much flatter, speeding up future operations not only on these elements, but also on those referencing them.

**MakeSet**

The MakeSet operation makes a new set by creating a new element with a unique id, a rank of 0, and a parent pointer to itself. The parent pointer to itself indicates that the element is the representative member of its own set.

The MakeSet operation has O(1) time complexity.

[See Princeton](https://algs4.cs.princeton.edu/15uf/)

```java
public class UnionFind{
        private int[] ids;
        private int[] rank;
        private int count;
        
        private UnionFind(int size) {
            this.ids = new int[size];
            this.rank = new int[size];
            
            //count represents the number of disjoint sets.
            this.count = size;
            
            //初始化时，对每一个数组元素，都指向自己。
            for (int i = 0; i < size; ++i) {
                ids[i] = i;
            }
        }
        
        //返回x的root.
        private int find(int x) {
            if (ids[x] == x) return x;
            if (ids[x] != x) {
                ids[x] = find(ids[x]); //Path compress
            }
            return ids[x];
        }
        
        //union by rank.
        //always attaches the shorter tree to the root of the taller tree
        private void union(int x, int y) {
            int rootx = find(x);
            int rooty = find(y);
            
            //x and y are already in the same set
            if (rootx == rooty) return;
            
            //x and y are not in the same set, so we merge them
            if (rootx < rooty) {
                ids[rootx] = rooty;
            } else if (rootx > rooty) {
                ids[rooty] = rootx;
            } else {
                ids[rootx] = rooty;
            }
            count--;
        }
        private int getCount(){
            return count;
        }
```
