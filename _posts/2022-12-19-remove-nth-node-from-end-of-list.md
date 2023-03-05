---
layout: post
title: Remove Nth Node From End of List
date: 2022-12-19 17:02 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Given the head of a linked list, remove the nth node from the end of the list and return its head.

[leetcode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

### SOLUTION

```java
class Solution {

    public int getListSize(ListNode node){
        int size = 0;
        while(node!=null){
            size++;
            node=node.next;
        }
        return size;
    }

    public ListNode removeNthFromEnd(ListNode head, int n) {
            
        int size = getListSize(head);
        int k = (size-n); 

        if(k == 0) return head.next;

        ListNode temp = head;

        while(k>1 && head !=null){
            k--;
            temp = temp.next;
        }

        if(temp.next != null)
            temp.next = temp.next.next;

        return head;

    }
}
```

### Another way to code using Dummy head (Two Pass)

```java
class Solution {

    public int getListSize(ListNode node){
        int size = 0;
        while(node!=null){
            size++;
            node=node.next;
        }
        return size;
    }

    public ListNode removeNthFromEnd(ListNode head, int n) {

        int size = getListSize(head);

        int k = size-n+1; //kth node from the start

        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;

        ListNode temp = dummyHead;

        while(k>1 && temp != null){
            k--;
            temp = temp.next;
        }

        temp.next = temp.next.next;

        return dummyHead.next;
        
    }
}
```

### Optimization (Single Pass | Two Pointers)

```java
class Solution {

    public ListNode removeNthFromEnd(ListNode head, int n) {

        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;

        ListNode t1 = dummyHead;
        ListNode t2 = dummyHead;

        //Move t1 N steps ahead
        for(int i=0; i<n; i++){
            t1 = t1.next;
        }

        //Keep moving t1 and t2 until t1 reaches last Node in the list
        //At that point, t2 will be before the Node which needs to be removed
        while(t1.next != null){
            t1 = t1.next;
            t2 = t2.next;
        }

        t2.next = t2.next.next;

        return dummyHead.next;
        
    }
}
```
