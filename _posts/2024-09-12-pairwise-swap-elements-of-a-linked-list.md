---
layout: post
title: Pairwise swap elements of a linked list (geeksforgeeks - SDE Sheet)
date: 2024-09-12 21:49 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a singly linked list. The task is to swap elements in the linked list pairwise. For example, if the input list is 1 2 3 4, the resulting list after swaps will be 2 1 4 3.

Note: You need to swap the nodes, not only the data. If only data is swapped then the driver code will print -1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/pairwise-swap-elements-of-a-linked-list-by-swapping-data/1?page=6)

## SOLUTION

```java
class Solution {

    public Node pairwiseSwap(Node head) {

        // Base case: if the list is empty or has only one node, no swap needed
        if(head == null || head.next == null)
            return head;

        // save the head for next subproblem which we will solve using recursion
        Node nextHead = head.next.next;

        // break the link between current pair of nodes with rest of the list
        head.next.next = null;

        // Pointers to the current node (first of the pair) and the next node (second of the pair)
        Node current = head;
        Node nextNode = head.next;

        // Swap: the second node points to the first node
        nextNode.next = current;

        // the current first node will need to point to the rest of the list after it has been "processed" using recursion
        current.next = pairwiseSwap(nextHead);

        // Return the new head of the swapped pair, i.e., the second node of the original pair
        return nextNode;
    }
}
```
