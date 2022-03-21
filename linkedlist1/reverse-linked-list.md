---
description: LeetCode 206
---

# Reverse Linked List

{% embed url="https://leetcode.com/problems/reverse-linked-list" %}

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

**Constraints:**

* The number of nodes in the list is the range `[0, 5000]`.
* `-5000 <= Node.val <= 5000`

{% hint style="info" %}
用cur指针记录向后遍历链表最新的链表头；

head指针记录旧链表的头节点，这个节点是不移动的；

当head指针的下一个节点为null时，遍历结束。
{% endhint %}

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        
        ListNode cur = head;
        while (head.next != null) {
            ListNode p = head.next;
            head.next = p.next;
            p.next = cur;
            cur = p;
        }
        return cur;
    }
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode newNode = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = newNode;
            newNode = head;
            head = temp;
        }
        return newNode;
    }
}
```

#### 解释 ⚠️ [看过来](linked-list.md#reverse-linked-list)
