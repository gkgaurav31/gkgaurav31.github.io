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

### APPROACH 1

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

### APPROACH 2

```java
class Solution {

    public ListNode getCenter(ListNode head){

        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;

    }

    public ListNode reverseLinkedList(ListNode head){

        ListNode h1 = head;
        ListNode h2 = null;
        ListNode t = head;

        while(h1 != null){
            h1 = h1.next;
            t.next = h2;
            h2 = t;
            t = h1;
        }

        return h2;

    }

    public ListNode mergeAlternative(ListNode h1, ListNode h2){

        if(h1 == null && h2 == null) return null;
        if(h1 == null && h2 != null) return h2;
        if(h1 != null && h2 == null) return h1; 

        //save the next nodes of both lists
        ListNode t1 = h1.next;
        ListNode t2 = h2.next;

        //next node of front node of list1 should point to front node of list2
        h1.next = h2;
        //next node of front node of list2 should point to mergeAlternative(t1, t2) -- recursion
        h2.next = mergeAlternative(t1, t2);

        return h1;

    }

    public void breakList(ListNode head, ListNode center){
        ListNode temp = head;
        while(temp.next != center){
            temp = temp.next;
        }
        temp.next = null;
    }

    public void reorderList(ListNode head) {

        //edge case
        if(head.next == null) return;
        
        //get the center of the list
        //if the length is even, pick the second one
        ListNode center = getCenter(head);

        //break link to divide the list into two parts
        breakList(head, center);

        //reverse 2nd part
        ListNode h2 = reverseLinkedList(center);

        //merge them as required in the question by alternative nodes between the lists
        head = mergeAlternative(head, h2);

    }
}
```
