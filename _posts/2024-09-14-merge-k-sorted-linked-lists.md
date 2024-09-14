---
layout: post
title: Merge K sorted linked lists (geeksforgeeks - SDE Sheet)
date: 2024-09-14 18:50 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of sorted linked lists of different sizes. The task is to merge them in such a way that after merging they will be a single sorted linked list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/merge-k-sorted-linked-lists/1?page=7)

## SOLUTION

```java
class Solution {

    Node mergeKLists(List<Node> arr) {

        int n = arr.size();

        if(n == 0)
            return null;

        if(n == 1)
            return arr.get(0);

        Node last = arr.get(n-1);
        arr.remove(n-1);

        Node rest = mergeKLists(arr);

        return sortedMerge(last, rest);

    }

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
