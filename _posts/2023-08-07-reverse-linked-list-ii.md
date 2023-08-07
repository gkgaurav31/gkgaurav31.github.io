---
layout: post
title: Reverse Linked List II
date: 2023-08-07 22:24 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode leetcode150 important"
categories: "linkedlist"
---
### PROBLEM DESCRIPTION

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

[leetcode](https://leetcode.com/problems/reverse-linked-list-ii/)

### SOLUTION

This problem is best understood if you try it out by hand. So just go ahead an take an example to walk-through the below code.

![snapshot]({{ site.baseurl }}/assets/img/reverse_linked_list_sublist.png)

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
    
    public ListNode reverseBetween(ListNode head, int left, int right) {

        //create a dummy node pointing to the head 
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        //"before" will point to the Node right before the "left" positioned Node
        ListNode before = dummy;
        for(int i=1; i<left; i++){
            before = before.next;
        }

        //now, we will iteratively reverse the nodes from position "left"
        ListNode previous = before;
        ListNode current = previous.next;

        for(int i=left; i<=right; i++){
            
            //to store the next node as we will be changing the next pointer of current node
            ListNode tempNext = current.next;

            current.next = previous;
            previous = current;
            current = tempNext;

        }

        //after reversing the sublist, two more modifications will be needed
        before.next.next = current;
        before.next = previous;

        return dummy.next;

    }

}
```
