# Introduction

## Introduction - Singly Linked List

Each node in a singly-linked list contains not only the value but also `a reference field` to link to the next node. By this way, the singly-linked list organizes all the nodes in a sequence.

Here is an example of a singly-linked list:

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/12/screen-shot-2018-04-12-at-152754.png)

The blue arrows show how nodes in a singly linked list are combined together.

## Add Operation - Singly Linked List

If we want to add a new value after a given node `prev`, we should:&#x20;

1. Initialize a new node `cur` with the given value;![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/26/screen-shot-2018-04-25-at-163224.png)
2. Link the "next" field of `cur` to prev's next node `next`;![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/26/screen-shot-2018-04-25-at-163234.png)
3. Link the "next" field in `prev` to `cur`.![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/26/screen-shot-2018-04-25-at-163243.png)

Unlike an array, we don’t need to move all elements past the inserted element. Therefore, you can insert a new node into a linked list in `O(1)` time complexity, which is very efficient.

## Delete Operation - Singly Linked List

If we want to delete an existing node `cur` from the singly linked list, we can do it in two steps:

1. Find cur's previous node `prev` and its next node `next`;![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/27/screen-shot-2018-04-26-at-203558.png)
2. Link `prev` to cur's next node `next`.![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/26/screen-shot-2018-04-26-at-203640.png)

In our first step, we need to find out `prev` and `next`. It is easy to find out `next` using the reference field of `cur`. However, we have to traverse the linked list from the head node to find out `prev` which will take `O(N)` time on average, where N is the length of the linked list. So the time complexity of deleting a node will be `O(N)`.

The space complexity is `O(1)` because we only need constant space to store our pointers.

{% hint style="info" %}
Java中引用数据类型传递的是对象的引用。在insert & delete操作要注意顺序。
{% endhint %}

## Reverse Linked List

In this article, we will talk more about details of our algorithm to reverse the linked list.

In the solution we mentioned previously, there are two nodes which we should keep track of: `the original head node` and `the new head node`.

Therefore, we need to use two pointers in one linked list at the same time. One pointer `head` always points at our original head node while another pointer `curHead` always points at our newest head node.

Let's focus on a single step (the 2nd step in the [previous article](https://leetcode.com/explore/learn/card/linked-list/219/linked-list-classic-problem/1204/)). Our goal is to move the next node of `head`, which is 15, to the head of the list.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/15/screen-shot-2018-04-14-at-181603.png)

1\. First, we use a temporary pointer `p` to indicate the next node of the head node. And link the "next" field of `head` to the "next" field of `p`.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/15/screen-shot-2018-04-14-at-182107.png)

2\. Then, we link the "next" field of `p` to the `curHead`.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/14/screen-shot-2018-04-14-at-182301.png)

3\. Now our linked list actually looks like the picture below. And we set `curHead` to be `p`.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/15/screen-shot-2018-04-14-at-182507.png)

By this way, we successfully move node 15 to the head of the list. And we can repeat this process until the next node of `head` is null.

{% hint style="info" %}
循环退出条件，当最后一个使用的点为空的时候退出，使用的点意思是要对它的引用变更。

初始化cur从head开始，它指向当前Linked List的头，head指向真正的头，每次iterate都用p来记录head的next，每次把p反转到开头之后，cur指向当前的头（也就是p）。直到循环结束，cur就指向已经反转好Linked List的头。
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

## Summary

We have provided several exercises for you. You might have noticed the similarities between them. Here we provide some tips for you:

**1. Going through some test cases will save you time.**

It is not easy to debug when using a linked list. Therefore, it is always useful to try several different examples on your own to validate your algorithm before writing code.

**2. Feel free to use several pointers at the same time.**

Sometimes when you design an algorithm for a linked-list problem, there might be several nodes you want to track at the same time. You should keep in mind which nodes you need to track and feel free to use several different pointers to track these nodes at the same time.

If you use several pointers, it will be better to give them suitable names in case you have to debug or review your code in the future.

**3. In many cases, you need to track the previous node of the current node.**

## **Tips**

It is similar to what we have learned in an array. But it can be trickier and error-prone. There are several things you should pay attention:

**1. Always examine if the node is null before you call the next field.**

Getting the next node of a null node will cause the null-pointer error. For example, before we run `fast = fast.next.next`, we need to examine both `fast` and `fast.next` is not null.

**2. Carefully define the end conditions of your loop.**

Run several examples to make sure your end conditions will not result in an endless loop. And you have to take our first tip into consideration when you define your end conditions.

## Complexity Analysis

It is easy to analyze the space complexity. If you only use pointers without any other extra space, the space complexity will be `O(1)`. However, it is more difficult to analyze the time complexity. In order to get the answer, we need to analyze `how many times we will run our loop` .

In our previous finding cycle example, let's assume that we move the faster pointer 2 steps each time and move the slower pointer 1 step each time.

1. If there is no cycle, the fast pointer takes `N/2 times` to reach the end of the linked list, where N is the length of the linked list.
2. If there is a cycle, the fast pointer needs `M times` to catch up the slower pointer, where M is the length of the cycle in the list.

Obviously, M <= N. So we will run the loop `up to N times`. And for each loop, we only need constant time. So, the time complexity of this algorithm is `O(N)` in total.

Analyze other problems by yourself to improve your analysis skill. Don't forget to take different conditions into consideration. If it is hard to analyze for all situations, consider the worst one.
