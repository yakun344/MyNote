---
description: LeetCode 25
---

# Reverse Nodes in k-Group

{% embed url="https://leetcode.com/problems/reverse-nodes-in-k-group" %}

Given a linked list, reverse the nodes of a linked list _k_ at a time and return its modified list.

_k_ is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of _k_ then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

**Example 3:**

```
Input: head = [1,2,3,4,5], k = 1
Output: [1,2,3,4,5]
```

**Example 4:**

```
Input: head = [1], k = 1
Output: [1]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `sz`.
* `1 <= sz <= 5000`
* `0 <= Node.val <= 1000`
* `1 <= k <= sz`

&#x20;

**Follow-up:** Can you solve the problem in O(1) extra memory space?

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
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode prev = dummy;
        while (prev != null) {
            prev = reverse(prev, k);
        }
        
        return dummy.next;
    }
    
    private ListNode reverse(ListNode head, int k) {
        ListNode n1 = head.next;
        ListNode nk = head;
        for (int i = 0; i < k; ++i) {
            nk = nk.next;
            if (nk == null) {
                return null;
            }
        }
        
        ListNode nkNext = nk.next;
        ListNode prev = null;
        ListNode cur = n1;
        while (cur != nkNext) {
            ListNode temp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        head.next = nk;
        n1.next = nkNext;
        return n1;
    }
}
```
