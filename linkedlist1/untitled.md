---
description: LeetCode 23
---

# Merge k Sorted Lists

{% embed url="https://leetcode.com/problems/merge-k-sorted-lists/" %}



You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

**Constraints:**

* `k == lists.length`
* `0 <= k <= 10^4`
* `0 <= lists[i].length <= 500`
* `-10^4 <= lists[i][j] <= 10^4`
* `lists[i]` is sorted in **ascending order**.
* The sum of `lists[i].length` won't exceed `10^4`.

#### Divide & Conquer Approach

![](<../.gitbook/assets/image (55).png>)

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        
        return merge(lists, 0, lists.length - 1);
    }
    
    public ListNode merge(ListNode[] lists, int start, int end) {
        if (start >= end) {
            return lists[start];
        }
        
        int mid = (start + end) / 2;
        ListNode left = merge(lists, start, mid);
        ListNode right = merge(lists, mid + 1, end);
        
        ListNode dummy = new ListNode(-1);
        ListNode prev = dummy;
        
        while (left != null && right != null) {
            if (left.val <= right.val) {
                prev.next = left;
                left = left.next;
            } else {
                prev.next = right;
                right = right.next;
            }
            prev = prev.next;
        }
        
        prev.next = (left != null) ? left : right;
        
        return dummy.next;
    }
}
```

#### Priority Queue Approach

{% hint style="info" %}
??????dummy???????????????????????????prev?????????prev.next?????????????????????node???

?????????????????????????????????????????????offer?????????????????????lists\[i]?????????????????????????????????

???heap??????????????????poll?????????List Node????????????List Node?????????????????????node??????????????????node???next offer???heap??????

???heap?????????????????????list???????????????

Time: O(Nlog(k)).----------------------------------------------------------------------------------------------------------4 ms

Initialize the priority queue takes O(Klogk);

Pop N nodes from the priority queue takes O(Nlogk).
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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        
        PriorityQueue<ListNode> heap = new PriorityQueue<>((a, b) -> {
            return a.val - b.val;
        });
        
        for (ListNode head : lists) {
            if (head == null) {
                continue;
            }
            heap.offer(head);
        }
        
        while (!heap.isEmpty()) {
            ListNode cur = heap.poll();
            prev.next = cur;
            prev = prev.next;
            
            if (cur.next != null) {
                heap.offer(cur.next);
            }
        }
        
        return dummy.next;
        
    }
}
```

#### Brout-Force

{% hint style="info" %}
??????????????????????????????min?????????index???

??????????????????continue??????

??????????????????node??????min??????????????????????????????index???

???prev.next??????min??????????????????

???????????????????????????node???

Time Complexity: O(kN), k is the number of linked-lists, N is the length of list.
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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        
        while (true) {
            ListNode min = null;
            int minIdx = -1;
            
            for (int i = 0; i < lists.length; ++i) {
                if (lists[i] == null) {
                    continue;
                }
                
                if (min == null || lists[i].val <= min.val) {
                    min = lists[i];
                    minIdx = i;
                }
            }
            
            if (min == null) {
                break;
            }
            
            prev.next = min;
            prev = prev.next;
            
            lists[minIdx] = min.next;
        }
        
        return dummy.next;
    }
}
```
