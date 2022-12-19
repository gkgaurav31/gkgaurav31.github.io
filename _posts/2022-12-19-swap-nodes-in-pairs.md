---
layout: post
title: Swap Nodes in Pairs
date: 2022-12-19 21:28 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

[leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

### SOLUTION

### APPROACH 1

```java
class Solution {

    public ListNode swapPairs(ListNode head) {

        if(head == null || head.next == null) return head;

        ListNode h1 = head;
        ListNode h2 = head.next;
        ListNode h3 = head.next.next;

        //Next node points to first
        h2.next = h1;

        //First Node points to swapPairs(Third Node)
        h1.next = swapPairs(h3);

        //Head will be second node
        return h2;
        
    }
    
}
```

### APPROACH 2

Using the algorithm to reverse nodes in groups, we can pass the size of group as 2.

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        return reverseKGroups(head, 2);
    }

    public ListNode reverseKGroups(ListNode head, int k){

        if(head == null || k == 0) return head;

        ListNode h1 = head;
        ListNode h2 = null;
        ListNode t = head;
        int times = k;

        ListNode h3 = head;

        while(k>0 && h1 != null){
            k--;
            h1 = h1.next;
            t.next = h2;
            h2 = t;
            t = h1;
        }

        h3.next = reverseKGroups(h1, times);

        return h2;

    }
}
```
