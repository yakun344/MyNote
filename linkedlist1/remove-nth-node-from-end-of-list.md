---
description: LeetCode 19
---

# Remove Nth Node From End of List

{% embed url="https://leetcode.com/problems/remove-nth-node-from-end-of-list/" %}

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Follow up:** Could you do this in one pass?

**Example 1:**![](https://assets.leetcode.com/uploads/2020/10/03/remove\_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

**Constraints:**

* The number of nodes in the list is `sz`.
* `1 <= sz <= 30`
* `0 <= Node.val <= 100`
* `1 <= n <= sz`

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode cur = head, prev = head;
        for (int i = 0; i < n; ++i) {
            cur = cur.next;
        }
        if (cur == null) {
            return head.next;
        }
        while (cur.next != null) {
            cur = cur.next;
            prev = prev.next;
        }
        prev.next = prev.next.next;
        return head;
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        dummy.next = head;
        while (n > 0) {
            head = head.next;
            n--;
        }
        ListNode cur = head;
        head = prev.next;
        while (cur != null) {
            cur = cur.next;
            head = head.next;
            prev = prev.next;
        }
        prev.next = head.next;
        return dummy.next;
    }
}
```
