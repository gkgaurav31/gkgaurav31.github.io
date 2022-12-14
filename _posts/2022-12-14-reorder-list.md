---
layout: post
title: Reorder List
date: 2022-12-14 23:11 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

You are given the head of a singly linked-list. The list can be represented as:  

> L0 → L1 → … → Ln - 1 → Ln

Reorder the list to be on the following form:  

> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.  

### SOLUTION

One way to solve this problem is to take a node and update it's next to reverse of remaining list. Do this recursively for each node.

```java
class Solution {

    public void reorderList(ListNode head) {
        reorderListHelper(head);
    }

    public ListNode reorderListHelper(ListNode head){
        if(head == null) return null;
        head.next = reverseList(head.next);
        reorderListHelper(head.next);
        return head;
    }

    public ListNode reverseList(ListNode head) {
        
        ListNode h1=head;
        ListNode t=head;

        ListNode h2 = null;

        while(h1!=null){
            h1=h1.next;
            t.next=h2;
            h2=t;
            t=h1;
        }

        return h2;

    }

}
```
