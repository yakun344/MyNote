---
description: LeetCode 234
---

# Palindrome Linked List

{% embed url="https://leetcode.com/problems/palindrome-linked-list/" %}

Given the `head` of a singly linked list, return `true` if it is a palindrome.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
Input: head = [1,2]
Output: false
```

**Constraints:**

* The number of nodes in the list is in the range `[1, 105]`.
* `0 <= Node.val <= 9`

&#x20;**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

{% hint style="info" %}
双指针，快慢指针把list分成均等的两部分，然后反转后面那一部分，再从前往后对比是不是一样。
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
    public boolean isPalindrome(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        
        //当链表长度是奇数当时候，slow要多向前走一步
        if (fast != null) {
            slow = slow.next;
        }
        slow = reverse(slow);
        fast = head;
        while (slow != null) {
        
            //比较值是不是相等，而不是对象
            if (fast.val != slow.val) {
                return false;
            }
            slow = slow.next;
            fast = fast.next;
        }
        return true;
    }
    private ListNode reverse(ListNode head) {
        
        //一定要看head是不是空。
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
