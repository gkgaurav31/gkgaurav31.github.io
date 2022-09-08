---
layout: post
title: Merge Sort Linked List
date: 2022-09-09 00:08 +0530
author: "Gaurav Kumar"
tags: "java linkedlist important"
---

### PROBLEM DESCRIPTION

Merge Sort a given Linked List.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/merge_sort_linked_list.png)

```java
public static Node mergeSort(Node h1){

    if(h1 == null || h1.next == null) return h1;

    Node c1 = getCenter(h1);
    Node h2 = c1.next;
    c1.next = null;

    h1 = mergeSort(h1);
    h2 = mergeSort(h2);

    return mergeSortedLinkedList(h1, h2);

}
```

Complete code:  [https://leetcode.com/problems/sort-list/](https://leetcode.com/problems/sort-list/)  

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
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null) return head;

        ListNode c1 = getCenter(head);
        ListNode h2 = c1.next;
        c1.next = null;

        head = sortList(head);
        h2 = sortList(h2);

        return mergeSortedLinkedList(head, h2);
    }
    

    
    public static ListNode getCenter(ListNode head){

        ListNode s=head;
        ListNode f=head;

        while(f.next != null && f.next.next != null){
            s = s.next;
            f = f.next.next;
        }

        return s;

    }
    
    public static ListNode mergeSortedLinkedList(ListNode h1, ListNode h2){

        //If any of the list is empty, return the other sorted list
        if(h1 == null) return h2;
        if(h2 == null) return h1;

        ListNode t=null, h3=null;

        //Initialize
        if(h1.val < h2.val){
            t=h1;
            h3=h1;
            h1=t.next;
        }else{
            t=h2;
            h3=h2;
            h2=t.next;
        }

        
        while(h1 != null && h2 != null){
            if(h1.val < h2.val){
                t.next=h1;
                h1=h1.next;
                t=t.next;
            }else{
                t.next=h2;
                h2=h2.next;
                t=t.next;
            }
        }

        if(h1 == null){
            t.next = h2;
        }else{
            t.next = h1;
        }

        return h3;
    }
}
```