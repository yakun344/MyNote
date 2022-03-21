---
description: LeetCode 706
---

# Design HashMap

{% embed url="https://leetcode.com/problems/design-hashmap" %}



Design a HashMap without using any built-in hash table libraries.

Implement the `MyHashMap` class:

* `MyHashMap()` initializes the object with an empty map.
* `void put(int key, int value)` inserts a `(key, value)` pair into the HashMap. If the `key` already exists in the map, update the corresponding `value`.
* `int get(int key)` returns the `value` to which the specified `key` is mapped, or `-1` if this map contains no mapping for the `key`.
* `void remove(key)` removes the `key` and its corresponding `value` if the map contains the mapping for the `key`.

&#x20;

**Example 1:**

```
Input
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
Output
[null, null, null, 1, -1, null, 1, null, -1]

Explanation
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]
```

&#x20;

**Constraints:**

* `0 <= key, value <= 106`
* At most `104` calls will be made to `put`, `get`, and `remove`.

```java
class MyHashMap {
    class Node{
        public int key, val;
        public Node next;
        public Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }
    final Node[] nodes = new Node[10000];
    public MyHashMap() {
        
    }
    
    public void put(int key, int value) {
        int i = idx(key);
        if (nodes[i] == null) {//类似dummy，方便操作链表
            nodes[i] = new Node(-1, -1);
        }
        
        Node prev = find(nodes[i], key);
        if (prev.next == null) {
            prev.next = new Node(key, value);
        } else {
            prev.next.val = value;
        }
    }
    
    public int get(int key) {
        int i = idx(key);
        if (nodes[i] == null) {
            return -1;
        }
        Node node = find(nodes[i], key);
        return node.next == null ? -1 : node.next.val;
    }
    
    public void remove(int key) {
        int i = idx(key);
        if (nodes[i] == null) {
            return;
        }
        Node prev = find(nodes[i], key);
        if (prev.next == null) return;
        prev.next = prev.next.next;
    }
    
    private int idx(int key) {
        return Integer.hashCode(key) % nodes.length;
    }
    
    private Node find(Node head, int key) {//找到当前key的前继节点
        Node node = head, prev = null;
        while (node != null && node.key != key) {
            prev = node;
            node = node.next;
        }
        return prev;
    }
}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```
