---
layout: post
title: Delete N Nodes After M Nodes of a Linked List
date: 2023-10-11 20:57 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode leetcodealgo100"
categories: "linkedlist"
---

## PROBLEM DESCRIPTION

You are given the head of a linked list and two integers m and n.

Traverse the linked list and remove some nodes in the following way:

- Start with the head as the current node.
- Keep the first m nodes starting with the current node.
- Remove the next n nodes
- Keep repeating steps 2 and 3 until you reach the end of the list.

Return the head of the modified list after removing the mentioned nodes.

[leetcode](https://leetcode.com/problems/remove-interval/)

## SOLUTION

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

    public ListNode deleteNodes(ListNode head, int m, int n) {

        // Initialize two pointers, p1 and p2, to traverse the linked list.
        ListNode p1 = head;
        ListNode p2 = head;

        // Start iterating through the linked list with p1.

        while (p1 != null) {

            // Keep the first m nodes starting with the current node.
            for (int i = 0; i < m - 1; i++) {

                // Move p1 to the next node if it's not null.
                if (p1 == null) break;

                p1 = p1.next;

            }

            // Remove the next n nodes.
            p2 = p1;

            for (int i = 0; i < n + 1; i++) {

                // Move p2 to the next node if it's not null.
                if (p2 == null) break;

                p2 = p2.next;

            }

            // If p1 is not null, connect p1 to the node after p2 (effectively removing n nodes).
            if (p1 != null) p1.next = p2;

            // Move p1 to the new position after removing the nodes.
            p1 = p2;
        }

        // Return the head of the modified linked list after processing.
        return head;
    }
}
```
