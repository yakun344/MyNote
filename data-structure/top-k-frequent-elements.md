---
description: LeetCode 347 LintCode 1281
---

# Top K Frequent Elements

{% embed url="https://leetcode.com/problems/top-k-frequent-elements/" %}

Given a non-empty array of integers, return the **k** most frequent elements.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Note:**&#x20;

* You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
* Your algorithm's time complexity **must be** better than O(n log n), where n is the array's size.
* It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
* You can return the answer in any order.

{% hint style="danger" %}
注意，数组的长度限制🚫不可以用heap.size(),因为heap.size()在每次poll()之后都会改变。
{% endhint %}

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        
        //统计每个Key的频率
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        
        //new一个最小heap，take in一个comparator，把key不停地丢到heap中，当size > k时，poll。
        //留下的elements就是frequency大的key
        Queue<Integer> heap = new PriorityQueue<>((n1, n2) -> (map.get(n1) - map.get(n2)));
        for (int n : map.keySet()) {
            heap.add(n);
            if (heap.size() > k) {
                heap.poll();
            }
        }
        
        //把heap中的elements放到result里。
        int[] result = new int[k];
        for (int i = 0; i < k; ++i) {
            result[i] = heap.poll();
        }
        
        return result;
    }
}
```
