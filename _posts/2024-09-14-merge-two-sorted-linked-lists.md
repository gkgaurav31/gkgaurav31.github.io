---
layout: post
title: Merge two sorted linked lists (geeksforgeeks - SDE Sheet)
date: 2024-09-14 18:44 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two sorted linked lists consisting of nodes respectively. The task is to merge both lists and return the head of the merged list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/merge-two-sorted-linked-lists/1?page=6)

## SOLUTION

```java
class Solution {

    Node sortedMerge(Node head1, Node head2) {

        Node h1 = head1;
        Node h2 = head2;

        Node dummyHead = new Node(0);
        Node tail = dummyHead;

        while (h1 != null && h2 != null) {

            Node temp = null;

            if (h1.data < h2.data) {
                temp = h1;
                h1 = h1.next;
            } else {
                temp = h2;
                h2 = h2.next;
            }

            temp.next = null;

            tail.next = temp;
            tail = tail.next;
        }

        if (h1 != null)
            tail.next = h1;
        else
            tail.next = h2;

        return dummyHead.next;

    }

}
```
