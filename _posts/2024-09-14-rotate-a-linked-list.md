---
layout: post
title: Rotate a Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-14 16:56 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a singly linked list, the task is to rotate the left shift of the linked list by k nodes, where k is a given positive integer smaller than or equal to the length of the linked list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rotate-a-linked-list/1?page=6)

## SOLUTION

We can solve this by first reversing the entire linked list, then splitting it into two parts based on the given rotation value k, and reversing both parts individually. After reversing the two halves, we reconnect them to form the final rotated list.

```java
class Solution {

    public Node rotate(Node head, int k) {

        // Get the length of the linked list
        int n = lengthLinkedList(head);

        // Reverse the entire linked list
        head = reverseLinkedList(head);

        // The linked list will be split into two parts. We will reverse both parts separately.
        Node h1 = head; // First half will start from the head
        Node h2 = head; // Second half will be determined later

        // If k is more than the length of the list, reduce k using modulo operation
        k = k % n;

        // Move h2 pointer n-k-1 steps forward to find the end of the first part
        for(int i = 0; i < n - k - 1; i++){
            h2 = h2.next;
        }

        // Save the tail of the first part of the list (end of first half)
        Node t1 = h2;

        // h2 now points to the head of the second part of the list
        h2 = h2.next;

        // Break the list into two parts by setting the next of t1 to null
        t1.next = null;

        // We'll use this temp variable to link the two parts together later
        Node temp = h1;

        // Reverse the first half of the list
        h1 = reverseLinkedList(h1);

        // Reverse the second half of the list
        h2 = reverseLinkedList(h2);

        // Join the two parts: the tail of the first part will point to the head of the second part
        temp.next = h2;

        // Return the new head of the rotated linked list
        return h1;
    }

    // Helper function to find the length of the linked list
    int lengthLinkedList(Node head){
        int count = 0;
        while(head != null){
            count++;
            head = head.next;
        }
        return count;
    }

    // Helper function to reverse a linked list
    Node reverseLinkedList(Node head){
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
