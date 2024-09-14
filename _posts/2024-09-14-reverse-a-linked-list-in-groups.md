---
layout: post
title: Reverse a Linked List in Groups (geeksforgeeks - SDE Sheet)
date: 2024-09-14 20:23 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a linked list, the task is to reverse every k node (where k is an input to the function) in the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should be considered as a group and must be reversed (See Example 2 for clarification).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/reverse-a-linked-list-in-groups-of-given-size/1?page=7)

## SOLUTION

```java
class Solution {

    public static Node reverse(Node node, int k) {

        if(node == null)
            return null;

        int count = 0;

        Node p1 = node;

        for(int i=0; i<k-1; i++){

            if(p1 == null)
                break;

            count++;
            p1 = p1.next;

        }

        // last set of nodes
        if(p1 == null)
            return reverseLinkedList(node);

        Node p2 = p1.next;
        p1.next = null;

        Node newHead = reverseLinkedList(node);

        node.next = reverse(p2, k);

        return newHead;

    }

    public static Node reverseLinkedList(Node head){
        Node h1 = head;
        Node h2 = null;

        while(h1 != null){
            Node t = h1;
            h1 = h1.next;
            t.next = h2;
            h2 = t;
        }
        return h2;
    }

}
```
