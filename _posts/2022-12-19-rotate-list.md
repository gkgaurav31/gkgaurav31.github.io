---
layout: post
title: Rotate List
date: 2022-12-19 22:43 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Given the head of a linked list, rotate the list to the right by k places.

[leetcode](https://leetcode.com/problems/rotate-list/description/)

### SOLUTION

```java
class Solution {

    public ListNode rotateRight(ListNode head, int k) {

        //get size of linked list
        int size = getSize(head);

        //if size of list is 0, return head
        if(size == 0) return head;

        //else, update k to k%size since after n rotations we'll get the same list (n is the number of nodes)
        k=k%size;
        //if k is now 0, means no rotation needed
        if(k == 0) return head;

        //reverse the list
        head = reverse(head);

        //reverse first k nodes
        head = reverseK(head, k);

        //move to kth node
        ListNode temp = head;
        for(int i=1; i<k; i++){
            temp = temp.next;
        }

        //next of kth node will be reverse of remaining list
        temp.next = reverse(temp.next);

        return head;
        
    }

    //get the size of linked list
    public int getSize(ListNode head){

        int s = 0;

        while(head != null){
            s++;
            head=head.next;
        }

        return s;
    }

    //reverse first K nodes of the linked list
    public ListNode reverseK(ListNode head, int k){

        ListNode h1= head;
        ListNode h2= null;
        ListNode t = head;
        ListNode h3 = head;

        while(k>0 && h1 != null){
            h1 = h1.next;
            t.next = h2;
            h2 = t;
            t = h1;
            k--;
        }

        h3.next = h1;

        return h2;
    }

    //reverse complete linked list
    public ListNode reverse(ListNode head){

        ListNode h1= head;
        ListNode h2= null;
        ListNode t = head;

        while(h1 != null){
            h1 = h1.next;
            t.next = h2;
            h2 = t;
            t = h1;
        }

        return h2;

    }
}
```
