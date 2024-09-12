---
layout: post
title: Sort a linked list of 0s, 1s and 2s (geeksforgeeks - SDE Sheet)
date: 2024-09-12 21:29 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a linked list where nodes can contain values 0s, 1s, and 2s only. The task is to segregate 0s, 1s, and 2s linked list such that all zeros segregate to the head side, 2s at the end of the linked list, and 1s in the middle of 0s and 2s.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/given-a-linked-list-of-0s-1s-and-2s-sort-it/1?page=6)

## SOLUTION

We can iterate over the nodes in the original list and use these nodes to create three linked lists (only by changing the pointers). The first list will have only nodes with 0 values, 2nd will have only the nodes with 1 as the value and 3rd list with 2 value nodes. Once we have these three lists, we can just point the first list's tail to head of second list. If second list is null, then it should point to third list. Similarly, second list should point to third list.

```java
class Solution {

    static Node segregate(Node head) {

        // Create three head pointers for lists of 0s, 1s, and 2s
        Node h0 = null, h1 = null, h2 = null;

        // Create tail pointers for the lists of 0s, 1s, and 2s
        Node t0 = null, t1 = null, t2 = null;

        // Pointer to traverse the original linked list
        Node current = head;

        // Traverse the original linked list
        while(current != null){

            // Store the next node temporarily, as current node's next will be altered
            Node nextNode = current.next;

            // If the current node contains data 0
            if(current.data == 0){

                // Add the node to the list of 0s
                current.next = h0;
                h0 = current;

                // If tail of 0s list is null, update it to point to the current node
                if(t0 == null)
                    t0 = current;

            } else if(current.data == 1){  // If the current node contains data 1

                // Add the node to the list of 1s
                current.next = h1;
                h1 = current;

                // If tail of 1s list is null, update it to point to the current node
                if(t1 == null)
                    t1 = current;

            } else {  // If the current node contains data 2

                // Add the node to the list of 2s
                current.next = h2;
                h2 = current;

                // If tail of 2s list is null, update it to point to the current node
                if(t2 == null)
                    t2 = current;

            }

            // Move to the next node in the original list
            current = nextNode;

        }

        // Now, connect the three separate lists: 0s -> 1s -> 2s

        // If there's a list of 0s, connect its tail to the head of the 1s list
        if(t0 != null){

            if(h1 != null)
                t0.next = h1;
            else
                t0.next = h2;  // If no 1s, connect it to the 2s list
        }

        // If there's a list of 1s, connect its tail to the head of the 2s list
        if(t1 != null)
            t1.next = h2;

        // Return the head of the new sorted list, starting with 0s, 1s, or 2s
        if(h0 != null)
            return h0;
        else if(h1 != null)
            return h1;
        else
            return h2;

    }
}
```
