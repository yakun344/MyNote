---
description: LeetCode 445
---

# Add Two Numbers II

{% embed url="https://leetcode.com/problems/add-two-numbers-ii" %}

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg)

```
Input: l1 = [7,2,4,3], l2 = [5,6,4]
Output: [7,8,0,7]
```

**Example 2:**

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [8,0,7]
```

**Example 3:**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

&#x20;

**Constraints:**

* The number of nodes in each linked list is in the range `[1, 100]`.
* `0 <= Node.val <= 9`
* It is guaranteed that the list represents a number that does not have leading zeros.

&#x20;

**Follow up:** Could you solve it without reversing the input lists?



**The intuition**

1. Since input lists may have different size it make sense to determine the sizes. At this step we also could normalize lists by prepending zero-nodes to the smaller list. But this approach will require additional memory, so we are going to ignore the normalization.
2. Iterate over the lists, compute the sum of items and put it into the resulting list. But here are the couple tricks: we are going to build the resulting list in the reversed order and we don't keep the carry value. For example:\

3. Next we are going to normalize the resulting list. Starting form it's head (which contains the lowest order number) we will iterate over all nodes normalizing the value (`val % 10`), remembering the carry value and reversing the resulting list.
4. At the last step the carry value may be greater than 0. In that case we have to create a new node and make it a header of the result.

![](https://assets.leetcode.com/users/peppered/image\_1592152162.png)

![image](https://assets.leetcode.com/users/peppered/image\_1592209145.png)

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        
        //记录l1和l2的长度
        int len1 = getLength(l1);
        int len2 = getLength(l2);
        
        ListNode cur = null;    //当前循环需要新加的点
        ListNode tempHead = null;   //当前链表的头
        //每次迭代，得到在对应的位上相加的数字
        //循环之后，得到一个以tempHead为头的链表
        while (l1 != null || l2 != null) {
            int value1 = 0;
            int value2 = 0;
            
            if (len1 >= len2) {
                value1 = l1 == null ? 0 : l1.val;
                l1 = l1.next;
                len1--;
            }
            
            if (len2 >= len1 + 1) {
                value2 = l2 == null ? 0 : l2.val;
                l2 = l2.next;
                len2--;
            }
            
            cur = new ListNode(value1 + value2);
            cur.next = tempHead;
            tempHead = cur;
        }
        
        //把大于10的节点进位，并且反转链表
        int carry = 0;
        tempHead = null;
        
        while (cur != null) {
            cur.val += carry;
            if (cur.val >= 10) {
                cur.val = cur.val % 10;
                carry = 1;
            } else {
                carry = 0;
            }
            
            ListNode temp = cur.next;
            cur.next = tempHead;
            tempHead = cur;
            cur = temp;
        }
        
        //查看最高位有没有进位，有的话更新
        if (carry > 0) {
            cur = new ListNode(1);
            cur.next = tempHead;
            tempHead = cur;
        }
        
        return tempHead;
    }
    private int getLength(ListNode head) {
        if (head == null) {
            return 0;
        }
        
        int length = 0;
        while (head != null) {
            head = head.next;
            length++;
        }
        
        return length;
    }
}
```

{% hint style="info" %}
Intuition:

Reverse two lists, then get the sum of two reversed list. Reverse the result list.
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p1 = reverse(l1);
        ListNode p2 = reverse(l2);
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        int carry = 0;
        while (p1 != null || p2 != null) {
            int x = p1 == null ? 0 : p1.val;
            int y = p2 == null ? 0 : p2.val;
            
            int sum = x + y + carry;
            carry = sum / 10;
            prev.next = new ListNode(sum % 10);
            
            prev = prev.next;
            
            if (p1 != null) {
                p1 = p1.next;
            }
            if (p2 != null) {
                p2 = p2.next;
            }
        }
        
        if (carry > 0) {
            prev.next = new ListNode(1);
        }
        return reverse(dummy.next);
    }
    private ListNode reverse(ListNode head) {
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
